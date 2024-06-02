# k-Clique Count Estimation

## Compile the code

```sh
$ make clean
$ make
```
It generates an executable "kCliqueC"

## Run the code

```sh
$ ./kCliqueC -g {path_to_graph} -k {k_value}
```

An example of estimating the number of 9-cliques in com-youtube with epsilon=0.001 is as follows
```sh
$ ./kCliqueC -g datasets/com-youtube -k 9 -a SR-kCCE -e 0.001
```

## Data format
Two data formats are supported. The default data format is "edges.txt", which contains a list of undirected edges represented as vertex pairs. The first line contains two numbers n and m, representing the number of vertices and the number of undirected edges, respectively. Note that, the vertex ids must be between 0 and n-1.

The more time-efficient format is the binary format; to read the input graph from this format, please add "-b" when running the code. Each graph is represented by two binary files, b_adj.bin and b_degree.bin (e.g. datasets/CA-GrQc/b_adj.bin and datasets/CA-GrQc/b_degree.bin). More details of the data format can be found in [https://lijunchang.github.io/Cohesive_subgraph_book/datasets](https://lijunchang.github.io/Cohesive_subgraph_book/datasets)

