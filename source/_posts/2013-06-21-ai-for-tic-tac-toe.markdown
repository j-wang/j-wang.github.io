---
layout: post
title: "AI for Tic-Tac-Toe"
date: 2013-06-21 20:28
comments: true
categories: [hacker school, python, AI, programming]
---

As part of my Hacker School application, I wrote a simple [Tic-Tac-Toe application][ttt] for my "program written from scratch." My choice was driven primarily by the fact that the application itself suggested a Tic-Tac-Toe application. As such, I expected many other people would write Tic-Tac-Toe (I was very right in this regard) and wanted to do something unique. My original ambitious plan was to write an AI for it. Unfortunately, I had to quickly abandon this plan, since I realized 1) I had no idea how I would go about creating one, and 2) I didn't have enough time to figure it out before the application was due. As such, I had to settle for infinitely expandable Tic-Tac-Toe (theoretically anyway -- at greater than 15 x 15, it becomes quite visually ugly).

Now that I do have the time, I definitely wanted to tackle writing an AI for Tic-Tac-Toe. However, I didn't want to just find and apply an algorithm. I wanted to have the full challenge and experience of working through how one would create an AI.<!-- more -->

My basic idea for an AI was that it would play out possible future moves and pick the one that resulted in the best outcome for itself. Of course, the trick is how one calculates "best outcome."

My first attempt involved counting the number of ways that a player could win vs. the number of ways an opponent could win and using this as the critera for determining the best next move. The main reason I decided to go this direction was simplicity of implementation: I had already implemented a complicated tallying/coordinate system in my Tic-Tac-Toe game's `Grid` class that I thought I could commandeer for this purposes. It also let me calculate best next moves for intermediate game-states, which appealed to me for implementing difficulty levels. The naive version of this method didn't work (the algorithm ignored one-move wins, preferring many ways to win over just winning), so I tried ramping up the value of a win to sys.maxint and penalizing longer chains to victory. Below, I show the primary logic behind the AI (full code is on [Github][gh]).

The basic algorithm involved is iterative recursive, which is a pattern that I'm finding I like a lot in Python. Iteratively traverse all cells of the grid (breadth):

{% codeblock lang:python %}
def traverse(grid, depth, XO, move_acc):
    open_cells = self._open_cells(grid.cells)
    dig_outcomes = []
    for cell in open_cells:
        dig_outcomes.append(dig(cell, grid, depth - 1, XO, move_acc))
    solution = max(dig_outcomes, key=lambda x: x[0])
    return solution
{% endcodeblock %}

And then recursively "dig" into each of these cells by simulating the player whose simulated turn it was moving there on a deepcopied `Grid` (depth).

The complicated code below (difficult to read mainly because it builds off of elements of the `Grid` class that I never exposed or expected to expose) shows the digging. It returns a tuple with win-loss "scores", and the moves it took to reach that outcome. The "scores" are actually a pair of scores: the first is based on the outcome (win/loss/tie -- though wins with fewer moves are given higher scores) and the second is based on how many more ways the AI could have won (and how close it was) vs. the player. Since many outcomes can be the same (win in same number of moves), I use the relative number of paths and closeness to victory as a tiebreaker.

{% codeblock lang:python %}
def dig(move, grid, depth, XO, move_acc):
    if depth == 0 or grid.winner():
        win_flag = 0

        if grid.winner() == self._XO:
            win_flag = sys.maxint - len(move_acc)  # short wins best
        elif grid.winner() == self._flip_player(self._XO):
            win_flag = -1  # losses are worst

        # remember that victory_routes are centered around 0
        adj_victory_route = map(lambda x: x * self._multiplier,
                                grid._victory_routes)
        opponent_closeness = min(adj_victory_route)
        opponent_ways = adj_victory_route.count(opponent_closeness)
        player_closeness = max(adj_victory_route)
        player_ways = adj_victory_route.count(player_closeness)
        diff = (player_closeness * player_ways +
                opponent_closeness * opponent_ways)

        return ((win_flag, diff), move_acc)
    else:
        new_grid = copy.deepcopy(grid)
        new_moves = copy.deepcopy(move_acc)
        r, c = move
        new_grid.fill_cell(r, c, XO)
        new_moves.append(move)
        return traverse(new_grid, depth, self._flip_player(XO),
                        new_moves)
{% endcodeblock %}

This kind of worked. The AI very often made the "right" move, but sometimes did stupid things. I'm pretty sure it was because I just multiplied together closeness to victory and ways to victory naively (implicitly weighting sure victories the same as many possible, but unlikely victories). I could have refined this further, and I still think there is some value to a path like this, since I can actually create more or less smart AIs by this method, but it would have taken much more trial-and-error tweaking of how I weighted various factors. The bigger problem was acutally that it was too slow -- turns out copying huge `Grid` objects over and over again down an entire tree of possible outcomes is really slow. Who would have thunk it? I didn't want to reimplement all of the logic I created to track number of ways to win, so I decided to go down a different route.

One thing I realized was that I didn't have to track _every_ move. I just needed to track the opponent's _best_ moves and my AI's responses to those moves. Now, what about best? As said, I didn't want to reimplement a bunch of `Grid`'s logic. If I gave up the idea of intermediate scoring, I could just determine the best move (for either player) by the one that ultimately resulted in victory at the last move... and then recursively choose the moves that give each player a better chance of victory all the way up the chain. I didn't quite realize it until I talked to other fellow Hacker Schoolers, but in trying to not reimplement a bunch of victory logic, I appeared to have reinvented a form of the mini-max algorithm. It's also much shorter and cleaner (called `LeanComputerPlayer` in the Github code).

The new iterative breadth traversal (the board is now just a flattened list representation of the board, e.g. 0th element is the top left cell, 4th element is the center cell) using a list comprehension:

{% codeblock lang:python %}
def descendants(moves, board, player):
    def copy_ret(brd, mv, plyr):
        new_board = copy.copy(board)
        new_board[mv] = plyr
        return new_board

    return [solve_tree(copy_ret(board, mv, player), flip(player), mv)
            for mv in moves]  # flip just switches players
{% endcodeblock %}

And the new recursive depth/comparison function, which sums up 1s for AI wins and -1s for human wins. It chooses max ultimate win outcomes if player is AI, choose min if player is human -- since the game is played to its end conditions, these are perfect inverses of one another:

{% codeblock lang:python %}
def solve_tree(board, player, move=None):
    moves = self._pos_moves(board)
    winner = self._winner(board)
    if moves == [] or winner is not False:
        if winner is not False:
            return (outcome_val[winner], move)
        else:
            return (self._draw_value, move)
    else:
        results = descendants(moves, board, player)
        min_or_max = max if player == self.XO else min
        result = min_or_max(results)
        return (result[0], move if move is not None else result[1])
{% endcodeblock %}

These two are helper functions within the `next_move` method in `LeanComputerPlayer`. They are kicked off by the last line in the method:

{% codeblock lang:python %}
return solve_tree(self._board, self.XO)[1]
{% endcodeblock %}

And it works (AI always wins or ties) and it's fast! The one optimization I made after this was to have the AI automatically pick the middle cell at the beginning of the game. Since traversing down the entire tree is an operation that takes n! steps where n is the number of starting cells (9 possible moves, with 8 possible next moves for each of the 9 starting moves, and then 7 moves for each of the 8 moves... and so on), calculating the first move is much slower than subsequent moves (9! moves to check vs. 8! which is 362,880 moves vs. 40,320) and also pointless since if the AI has the first move, we know the center cell is always the optimal solution. Again, this code is on [Github][gh].

And thus, week three ends! I'm looking forward to week four though at the moment, I don't quite know what I'll turn to next.

[ttt]: https://github.com/j-wang/hacker-school/blob/master/tictactoe.py
[gh]: https://github.com/j-wang/tictactoe_ai
