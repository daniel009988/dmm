# Digital Memcomputing Machine (DMM) based SAT-Solver

In the 'Science' article "Memcomputing NP-complete problems in polynomial time using polynomial resources and collective states" (https://www.science.org/doi/10.1126/sciadv.1500031), Memcomputing is introduced as a novel non-Turing paradigm of computation that uses interacting memory cells (memprocessors for short) to store and process information on the same physical platform. It is claimed that it was recently proven mathematically that memcomputing machines have the same computational power of nondeterministic Turing machines. Therefore, they should solve NP-complete problems in polynomial time and, using the appropriate architecture, with resources that only grow polynomially with the input size. Our implementation aims to reproduce the results as shown in the paper:

![Figure 2](https://github.com/daniel009988/dmm/blob/main/doc/paper.jpeg?raw=true)

This repository is a research implementation of the Digital Memcomputing Machine (DMM) described in this paper with the goal to verify if the results of the paper can be reproduced as well as to investigate if the performance extends to other SAT problem classes as well. Based on the boost library, it supports multiple ODE integration schemes and has a few parameter tuning options. Please note that the implementation is research driven, so we are happy to receive feedback / improvements at any time. 

## Requirements

We have tested the implementation on Ubuntu Linux and MacOS Monterey and expect to work on any modern Linux or Mac based platform. To compile, a properly installation of the boost library is required (on MacOS, use `brew install boost`). For installing boost on your Linux based system, we refer to the libraries' website: https://www.boost.org/

## Compilation

The entire DMM based SAT solver is implemnted in one file, no Makefile required. We typically compile the file dmm.cc:

```g++ dmm.cc -o dmm -std=c++17 -Ofast -lpthread -fpermissive``` (Linux)

```g++ dmm.cc -o dmm -std=c++14 -Ofast -I /opt/homebrew/cellar/boost/1.78.0/include -L /opt/homebrew/cellar/boost/1.78.0/lib``` (MacOS)


After compilation, the file "dmm" is being produced in the directory.

## Running the DMM

The repository contains a few test instances. You can run the DMM based solver with default parameters:

```./dmm -i transformed_barthel_n_100_r_4.300_p0_0.080_instance_001.cnf```

This will load the CNF file and run 8 instances of a DMM to solve it. When a solution is found (the system evolved to an energy level of zero unsat clauses), the variable assignments are being printed and also stored in a .solution.txt file of the instance.

## Parameters

The following command shows all available command line options:

```./dmm -h```

When there's more time, we might compile more detailed information about the available command line features, but most of them should be self-explanatory. The source code is another point to look for further details about the parameters. Some of the most noteable parameters are:

```./dmm -i <CNF-FILE> -o 1``` uses the forward Euler integration scheme to solve the ODE

```./dmm -i <CNF-FILE> -o 3``` uses the Adaptive Runge Kutta Cash Karp 5.4 integration scheme to solve the ODE

```./dmm -i <CNF-FILE> -o 4``` uses the implicit Bulirsch Stoer integration scheme to solve the ODE

```./dmm -i <CNF-FILE> -w 1``` uses one thread for the integration

```./dmm -i <CNF-FILE> -t 1 -u 5``` starts a parameter tuning run to find ideal parameters for the ODE as well as for the integration scheme


## Source code settings

The maximum number of literals in a singe clause is specified in the code with:

```#define         MAX_LITS_SYSTEM 10```

Update this number to the requirements of the CNF formulation you like to solve. Please note that the higher this value is, the more memory is being allocated.

We have implemented higher precision ODE systems by using (i) boost's MultiPrecision functionality and (ii) a modern POSITs library. If you want to enable higher precision or POSITS, the required parts in the source code will have to be uncommmented. When running our tests, we didn't experience major improvements compared to double precision used by default.

<sub>Referenced article:
author = {Fabio Lorenzo Traversa  and Chiara Ramella  and Fabrizio Bonani  and Massimiliano Di Ventra },
title = {Memcomputing <i>NP</i>-complete problems in polynomial time using polynomial resources and collective states},
journal = {Science Advances},
volume = {1},
number = {6},
pages = {e1500031},
year = {2015},
doi = {10.1126/sciadv.1500031},
URL = {https://www.science.org/doi/abs/10.1126/sciadv.1500031},
eprint = {https://www.science.org/doi/pdf/10.1126/sciadv.1500031},
abstract = {Demonstration of computing with memory points to a new route for solving hard problems faster. Memcomputing is a novel non-Turing paradigm of computation that uses interacting memory cells (memprocessors for short) to store and process information on the same physical platform. It was recently proven mathematically that memcomputing machines have the same computational power of nondeterministic Turing machines. Therefore, they can solve NP-complete problems in polynomial time and, using the appropriate architecture, with resources that only grow polynomially with the input size. The reason for this computational power stems from properties inspired by the brain and shared by any universal memcomputing machine, in particular intrinsic parallelism and information overhead, namely, the capability of compressing information in the collective state of the memprocessor network. We show an experimental demonstration of an actual memcomputing architecture that solves the NP-complete version of the subset sum problem in only one step and is composed of a number of memprocessors that scales linearly with the size of the problem. We have fabricated this architecture using standard microelectronic technology so that it can be easily realized in any laboratory setting. Although the particular machine presented here is eventually limited by noise—and will thus require error-correcting codes to scale to an arbitrary number of memprocessors—it represents the first proof of concept of a machine capable of working with the collective state of interacting memory cells, unlike the present-day single-state machines built using the von Neumann architecture.</sub>
