# Synopsis

**gtltl2ba** tool enhances **ltl2ba** with the following features:

- `-g`: display Buchi Automaton on screen
- `-G file_name`: save Buchi Automaton as `file_name.pdf`
- `-t`: print Buchi Automaton in DOT format
- `-T file_name`: save Buchi Automaton in DOT format as `file_name`.

All the rest remains equal with the **ltl2ba** tool.

# Motivation

The website of [ltl2ba](http://www.lsv.fr/~gastin/ltl2ba/index.php) already provides several options for generating and displaying Buchi Automatons starting from an ltl formula. However, this functionality is not available when offline.

This wrapper for **ltl2ba** was conceived as an alternative mean for achieving the same goal when an internet connection is not available.

# Installation

1. Install `graphviz` module for `python3`:

       ~$ pip3 install graphviz

2. download **ltl2ba** from [here](http://www.lsv.fr/~gastin/ltl2ba/download.php), and
   install it:
   
       ~$ tar -xf ltl2ba-1.2b1.tar.gz
       ~$ cd ltl2ba-1.2b1
       ~$ make
       ~$ export PATH=$PATH:$(pwd)

3. clone or download **gltl2ba** repository, and add it to your `PATH`:

       ~$ unzip gltl2ba-master.zip
       ~$ cd gltl2ba-master
       ~$ export PATH=$PATH:$(pwd)
    
To make your installation permanent, modify `.bashrc` so as to update the `PATH` variable
to contain the paths for **ltl2ba** and **gltl2ba**.

# Usage

    usage: gltl2ba [-h] (-f FORMULA | -F FILE) [-d] [-s] [-l] [-p] [-o] [-c] [-a]
                   [-g] [-G OUTPUT_GRAPH] [-t] [-T OUTPUT_DOT]
    
    optional arguments:
      -h, --help            show this help message and exit
      -f FORMULA, --formula FORMULA
                            translate LTL into never claim
      -F FILE, --file FILE  like -f, but with the LTL formula stored in a 1-line
                            file
      -d                    display automata (D)escription at each step
      -s                    computing time and automata sizes (S)tatistics
      -l                    disable (L)ogic formula simplification
      -p                    disable a-(P)osteriori simplification
      -o                    disable (O)n-the-fly simplification
      -c                    disable strongly (C)onnected components simplification
      -a                    disable trick in (A)ccepting conditions
      -g, --graph           display buchi automaton graph
      -G OUTPUT_GRAPH, --output-graph OUTPUT_GRAPH
                            save buchi automaton graph in pdf file
      -t, --dot             print buchi automaton graph in DOT notation
      -T OUTPUT_DOT, --output-dot OUTPUT_DOT
                            save buchi automaton graph in DOT file

# Example

To generate the Buchi Automaton of `([] p0) || (<> p1)`, type:

    ~$ gltl2ba.py -f "([] p0) || (<> p1)" -t -g

to get the *Promela* code of the Buchi Automaton and the DOT version:

    never { /* ([] p0) || (<> p1) */
    T0_init:
    	if
    	:: (1) -> goto T0_S1
    	:: (p1) -> goto accept_all
    	:: (p0) -> goto accept_S2
    	fi;
    T0_S1:
    	if
    	:: (1) -> goto T0_S1
    	:: (p1) -> goto accept_all
    	fi;
    accept_S2:
    	if
    	:: (p0) -> goto accept_S2
    	fi;
    accept_all:
    	skip
    }
    
    digraph {
    	T0_init [label=init peripheries=1 shape=circle]
    	T0_init -> T0_S1 [label="(1)"]
    	T0_init -> accept_all [label="(p1)"]
    	T0_init -> accept_S2 [label="(p0)"]
    	T0_S1 [label=S1 peripheries=1 shape=circle]
    	T0_S1 -> T0_S1 [label="(1)"]
    	T0_S1 -> accept_all [label="(p1)"]
    	accept_S2 [label=S2 peripheries=2 shape=circle]
    	accept_S2 -> accept_S2 [label="(p0)"]
    	accept_all [label=all peripheries=2 shape=circle]
    	accept_all -> accept_all [label="(1)"]
    }

the following graph is also generated:

![alt text](https://github.com/PatrickTrentin88/gltl2ba/blob/master/examples/example.png)

# Contributors

This project is authored by [Patrick Trentin](http://www.patricktrentin.com) (<patrick.trentin.88@gmail.com>).

# License

GNU General Public License v3.0
