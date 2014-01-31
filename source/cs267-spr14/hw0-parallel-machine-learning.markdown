---
layout: page
title: "HW0 - Parallel Machine Learning"
date: 2014-01-31 01:25
comments: true
sharing: true
footer: true
---

## Personal Bio:

I’m a first year MBA student at UC Berkeley Haas School of Business. For the past few years, I worked at a large, systematic macroeconomic hedge fund and am personally interested in data analysis and distributed computing. Within these areas, I have a particular interest in machine learning and programming paradigms that make asynchronous/parallel computing easier (continuation-passing style, go-channels, etc.).

What I’m hoping to get out of CS267 is a better theoretical grasp of the problems inherent in parallel computing, especially since my own learning in this area has been self-study and “as I’ve encountered it,” and as a result my understanding is patchy. Of course, it’ll also be interesting running programs on a real supercomputer, which I’ve never actually done before.

## Machine Learning

### Background
A very broad area where parallel computing is becoming important is machine learning. From my own experience and observations of industry, more and more applications of data analysis and machine learning that used to be run from a single machine now are run from multiple machines and require parallel computing to run in an acceptable amount of time. One of the biggest drivers of the increasing commonality is extremely large datasets, exploded along the lines of data, features, or models. Given the breadth of applications and different algorithms involved, it’s hard to generalize successes, weaknesses, and challenges to the entire field. As such, I will be speaking more broadly about the area, specifically highlighting issues that apply to large classes of machine learning algorithms.

### Parallelization
On the most basic operational level, machine learning has benefited significantly from the development of standard libraries that efficiently execute computationally intensive operations in parallel/distributed fashion (ScaLAPACK, etc.), and improvements in these libraries. Most machine learning algorithms involve a significant number of linear algebra operations, though certain common algorithms, like linear regression and principal component analysis, are bottlenecked by matrix operations (from a seven/thirteen “dwarves” perspective) and especially benefit from speedups in these libraries (Gray 2009). However, this only addresses basic operations within machine learning.

On a higher level, the primary area of interest in generalizable parallelization techniques in machine learning has been around either applying or reformulating algorithms to fit within a MapReduce framework. There are many algorithms that have “embarrassingly parallel” elements. Chu et al. showed that ten commonly used algorithms could be rewritten in an easily parallelizable form. In an experiment on multicore machines (specifically, an Intel X86 dual-core and Sun Enterprise 6000) using these rewritten parallelized forms, they were able to achieve with locally weighted linear regression (LWLR), k-means, logistic regression, naive Bayes, SVM, ICA, PCA, Gaussian discriminant analysis, EM, and backpropagation in neural networks (Chu, Kim, Lin, Yu, Bradski, Ng, Olukotun 2006).

However, while this work is extremely helpful for some algorithms, computing across the largest datasets is done on distributed systems of thousands of networked machines, which changes the practicality of certain algorithms parallelized in this way. For example, k-means is an iterative algorithm and each MapReduce operation only advances the algorithm by one iteration (Guestrin 2013). While this may have been acceptable in a multicore context where all processors exist on the same machine, this is problematic when hundreds or thousands of iterations are involved, each iteration requiring the completion of a reduce step (bottlenecking the iteration to the slowest process to return), and inevitable delays that occur in network communication.

One example of work trying to deal with this type of delay is in Vowpal Wabbit, a parallelized online learning library. To scale to large datasets, this library uses a feedforward neural network and shards the feature set to individual nodes. This allows each node to do its full computation and learning in parallel and only recombine outputs at the end. Additionally, each feature shard is stateless, which means they can be easily parallelizable and won’t cause cache conflicts on shared memory machines with caches. Unfortunately, this algorithm is no longer the same as the normal online learning descent (Langford 2010). In return though, this algorithm was able to learn on a terascale dataset with trillions of features, billions of training examples, and millions of parameters in an hour using a cluster of 1000 machines (Alekh, Chapelle, Dudik, and Langford 2013). Nevertheless, while there are some interesting techniques that may be generalizable from Vowpal Wabbit, it ultimately is a “hand-adjusted” learning algorithm (like cascade SVM), versus a generalizable parallel programming technique that can be applied to other learning algorithms—which appears to be the general state of this application area, where the most effective tools for parallelization are still relatively low-level and significant speedups often require the creation of entirely new parallel algorithms.

## Citations
<small>

- Agarwal, Alekh, Olivier Chapelle, Miroslav Dudik, and John Langford. "A Reliable Effective Terascale Linear Learning System." ArXiv.org. Cornell University, 12 July 2013. Web. <http://arxiv.org/abs/1110.4198>.

- Chu, Chen-Tao, Sang Kyun Kim, Yi-An Lin, YuanYuan Yu, Gary Bradski, Andrew Y. Ng, and Kunle Olukotun. "Map-Reduce for Machine Learning on Multicore." Neural Information Processing Systems Foundation 19 (2006): n. pag. Web. <http://books.nips.cc/papers/files/nips19/NIPS2006_0725.pdf>.

- Gray, Alex. "Large-Scale Machine Learning: The Problems, Algorithms, and Challenges." Proc. of NIPS 2009 (Neural Information Processing Systems), Hyatt Regency Vancouver, Vancouver, BC, Canada. N.p., n.d. Web. <http://videolectures.net/nipsworkshops09_gray_lsml/>.

- Guestrin, Carlos. "Parallel K-Means on Map-Reduce." Lecture. 31 Jan. 2013. Web. <http://courses.cs.washington.edu/courses/cse599c1/13wi/slides/mapreduce-kmeans.pdf>.

- Langford, John. "A Method for Parallel Online Learning." Proc. of MMDS 2010 (Workshop on Algorithms for Modern Massive Data Sets), Stanford, CA. N.p., n.d. Web. <http://www.stanford.edu/group/mmds/slides2010/Langford.pdf>.

</small>
