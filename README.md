
# GED4py a graph edit distance library for python. It is based on GMatch4Py.

GED4py is a library to compute graph edit distance (GED) much faster than NetworkX. The Graph structures are stored in NetworkX graph objects.
GED4py algorithms were implemented with Cython to enhance performance.

## Requirements

 * Python 3
 * Numpy and Cython installed (if not : `(sudo) pip(3) install numpy cython`)
 
## Installation

To install `GED4py`, run the following commands:

```bash
git clone https://github.com/chilligerchief/GED4py.py
cd GED4py
(sudo) pip(3) install .
```

## Get Started
### Graph input format

In `GED4py`, algorithms manipulate `networkx.Graph`, a complete graph model that 
comes with a large spectrum of parser to load your graph from various inputs : `*.graphml,*.gexf,..` (check [here](https://networkx.github.io/documentation/stable/reference/readwrite/index.html) to see all the format accepted)

### Use GED4py
To use the *graph edit distances*, here is an example:

```python
# GED4py use networkx graph 
import networkx as nx 
import ged4py
```

In this example, we use generated graphs using `networkx` helpers:
```python
g1=nx.complete_bipartite_graph(5,4) 
g2=nx.complete_bipartite_graph(6,4)
```

All graph matching algorithms in `GED4py` work this way:
 * Each algorithm is associated with an object, each object having its specific parameters. In this case, the parameters are the edit costs (delete a vertex, add a vertex, ...)
 * Each object is associated with a `compare()` function with two parameters. First parameter is **a list of the graphs** you want to **compare**, i.e. measure the distance/similarity (depends on the algorithm). Then, you can specify a sample of graphs to be compared to all the other graphs. To this end, the second parameter should be **a list containing the indices** of these graphs (based on the first parameter list). If you rather compute the distance/similarity **between all graphs**, just use the `None` value.

```python
ged=ged4py.GraphEditDistance(1,1,1,1) # all edit costs are equal to 1
result=ged.compare([g1,g2],None) 
print(result)
```

The output is a similarity/distance matrix :
```python
array([[0., 14.],
       [10., 0.]])
```
This output result is "raw", if you wish to have normalized results in terms of distance (or similarity) you can use :

```python
ged.similarity(result)
# or 
ged.distance(result)
```

## Exploit nodes and edges attributes

In this latest version, we add the possibility to exploit graph attributes ! To do so, the `base.Base` is extended with the `set_attr_graph_used(node_attr,edge_attr)` method.

```python
import networkx as nx 
import GED4py as gm
ged = gm.GraphEditDistance(1,1,1,1)
ged.set_attr_graph_used("theme","color") # Edge colors and node themes attributes will be used.
```

## List of algorithms


 * Graph Edit Distance [5]
    * Approximated Graph Edit Distance 
    * Hausdorff Graph Edit Distance 
    * Bipartite Graph Edit Distance 
    * Greedy Edit Distance

If you want to use one of the following algorithms, please refer to the GMatch4py library

 * Graph Embedding
    * Graph2Vec [1]
 * Node Embedding
    * DeepWalk [7]
    * Node2vec [8]
 * Graph kernels
    * Random Walk Kernel (*debug needed*) [3]
        * Geometrical 
        * K-Step 
    * Shortest Path Kernel [3]
    * Weisfeiler-Lehman Kernel [4]
        * Subtree Kernel     
 * Vertex Ranking [2]
 * Vertex Edge Overlap [2]
 * Bag of Nodes (a bag of words model using nodes as vocabulary)
 * Bag of Cliques (a bag of words model using cliques as vocabulary)
 * MCS [6]
    

## Publications associated

  * [1] Papadimitriou, P., Dasdan, A., & Garcia-Molina, H. (2010). Web graph similarity for anomaly detection. Journal of Internet Services and Applications, 1(1), 19-30.
  * [2] Shervashidze, N., Schweitzer, P., Leeuwen, E. J. V., Mehlhorn, K., & Borgwardt, K. M. (2011). Weisfeiler-lehman graph kernels. Journal of Machine Learning Research, 12(Sep), 2539-2561.
  * [3] Fischer, A., Riesen, K., & Bunke, H. (2017). Improved quadratic time approximation of graph edit distance by combining Hausdorff matching and greedy assignment. Pattern Recognition Letters, 87, 55-62.
  * [4] A graph distance metric based on the maximal common subgraph, H. Bunke and K. Shearer, Pattern Recognition Letters, 1998  

## Author(s)

Adrian Hofmann

The implementations were forked from

Jacques Fize, *jacques[dot]fize[at]cirad[dot]fr*

Some algorithms from other projects were integrated to GED4py. **Be assured that
each code is associated with a reference to the original.**


## CHANGELOG
### 10.11.2022

 * Removed all functionalities not used for graph edit distances