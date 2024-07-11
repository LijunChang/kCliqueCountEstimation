# k-Clique Count Estimation

## Overview

This guide provides instructions for compiling and running our k-Clique count estimation algorithm SR-kCCE. It estimates the number of k-cliques in a given graph with a given relative error epsilon; the failure probability (i.e., produce an estimation with relative error larger than epsilon) is 1%.

This program also includes the implementation of other algorithms, Pivoter and DPColorPath.

## Compilation

To compile the code, run the following commands:

```sh
$ make clean
$ make
```
It will generate an executable file named `kCliqueC`.

## Execution

To run the kCliqueC program, use the following command:

```sh
$ ./kCliqueC -g {path_to_graph} -k {k_value} -a {algorithm} -e {epsilon}
```

### Parameters:

-   `-g {path_to_graph}`: Specifies the path to the input graph file.
-   `-k {k_value}`: Defines the clique size (k).
-   `-a {algorithm}`: Chooses the algorithm from `SR-kCCE`, `DPColorPath`, or `Pivoter`. 
-   `-e {epsilon}`: Sets the relative error parameter (epsilon).
-   `-t {number_of_samples}`: Manually sets the number of samples. This is optional and only used for DPColorPath. Once it is specified, then a fixed number of samples is taken and the given epsilon is ignored; thus, the estimation accuracy is not guaranteed.
-   `-r {number_of_refinement}`: Manually sets the number of refinement used in our algorithm SR-kCCE.
-   `-b`: This is optional. Once set, the graph will be loaded from the two binary files (see below for more information on the data format.

### Examples for reproducing the paper experimental results:

#### 1. Get the exact 12-clique count in com-youtube
```sh
$ ./kCliqueC -b -g datasets/com-youtube -k 12 -a Pivoter
```

The running time and memory usage (reported in Figures 11--13) can be obtained by using the /usr/bin/time command.

#### 2. Get the estimated 12-clique count in com-youtube by our algorithm SR-kCCE with a relative error 0.001

```sh
$ ./kCliqueC -b -g datasets/com-youtube -k 12 -a SR-kCCE -e 0.001
```
By comparing the estimated count with the exact count, we can get the actual relative error achieved by SR-kCCE as reported in Figures 7 and 8

The k-clique density mu (reported in Figure 10) can be obtained as (total\_estimate\_cnt - exact\_cnt)/UB.

The running time and memory usage (reported in Figures 11--13) can be obtained by using the /usr/bin/time command.

#### 3. Get the estimated 12-clique count in com-youtube by the existing algorithm DPColorPath with a relative error 0.001

```sh
$ ./kCliqueC -b -g datasets/com-youtube -k 12 -a DPColorPath -e 0.001
```
By comparing the estimated count with the exact count, we can get the actual relative error achieved by DPColorPath as reported in Figure 8

The k-clique density mu (reported in Figure 10) can be obtained as (total\_estimate\_cnt - exact\_cnt)/UB.

The running time and memory usage (reported in Figures 11--13) can be obtained by using the /usr/bin/time command.

#### 4. Get the estimated 12-clique count in com-youtube by the existing algorithm DPColorPath with a fixed number (i.e., 50,000,000) samples

```sh
$ ./kCliqueC -b -g datasets/com-youtube -k 12 -a DPColorPath -t 50000000
```
By comparing the estimated count with the exact count, we can get the actual relative error achieved by DPColorPath5e7 as reported in Figure 8

#### 5. Run our algorithm SR-kCCE with a fixed number (e.g., 1000) of refinement and relative error 0.001

```sh
$ ./kCliqueC -b -g datasets/com-youtube -k 12 -a SR-kCCE -e 0.001 -r 1000
```

From the output, we can get the results reported in Figure 14

## Data Format

The SR-kCCE program supports two data formats for input graphs: the default text format and a more time-efficient binary format.

### Default Text Format

-   The input file `edges.txt` contains a list of undirected edges represented as vertex pairs.
-   The first line of the file should contain two integers, `n` and `m`, representing the number of vertices and the number of undirected edges, respectively.
-   Vertex IDs must be within the range `[0, n-1]`.

### Example of `edges.txt`

```
5 7
0 1
0 2
1 2
1 3
1 4
2 3
3 4
```

### Binary Format

For faster graph loading, the binary format is recommended. Each graph is represented by two binary files: `b_adj.bin` and `b_degree.bin`.

-   `b_adj.bin`: Contains the graph's adjacency lists.
-   `b_degree.bin`: Contains the degree of the vertices.

To read the input graph from the binary format, add the `-b` flag when running the kCliqueC program:

```
$ ./kCliqueC -g datasets/CA-GrQc -k 9 -a SR-kCCE -e 0.001 -b 
```

### More Information

For more details on the data format, refer to the [Cohesive Subgraph Book datasets page](https://lijunchang.github.io/Cohesive_subgraph_book/datasets).
