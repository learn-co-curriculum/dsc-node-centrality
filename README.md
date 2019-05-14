
# Node Centrality 

## Introduction

Dijkstra's algorithm and other path related searches are essential for establishing a common distance metric within network graphs. With that, one can investigate relationships between nodes. For example, in the context of social networks, one might wonder who is the most influential to various social circles. Alternatively, if you were studying the spread of disease, it would be important to identify key stakeholders for quarantining the disease off from subpopulations. The umbrella term for these metrics is known as centrality. In this lesson, you'll take a look at four measures of centrality and how they can collectively uncover important relationships within networks.

## Objectives

You will be able to:

* Understand and explain network centrality and its importance in graph analysis
* Understand and calculate Degree, Closeness, Betweenness and Eigenvector centrality measures
* Describe the use case for several centrality measures

## Centrality

A central concept to graphs is centrality. Take the social network below, created from a subset of twitter data. Some of the nodes are enmeshed in the network, with many connections amongst other nodes. Others, such as node 95, are tied to this central hub, but only weakly through a single connection to the main hub. Two nodes, 86 and 87, aren't even directly tied to the main cluster, and are floating on an island on their own. By quantifying the relationships between nodes, one can better understand the underlying structure of networks.


```python
import networkx as nx
import matplotlib.pyplot as plt
%matplotlib inline
```


```python
#Load the Network from File
G = nx.read_edgelist("twitter.edges")
#Simplify the Node Labels
G = nx.relabel_nodes(G, dict(zip(G.nodes, range(len(G.nodes)))))
#Create a matplotlib figure
fig = plt.figure(figsize=(15,10))
#Draw the network!
nx.draw(G, pos=nx.spring_layout(G, random_state=5), with_labels=True,
        alpha=.8, node_color="#1cf0c7", node_size=700)
```


![png](index_files/index_4_0.png)


## Degree Centrality 

The most primitive measure of centrality is degree centrality. This is simply the number of edges attached to a node. In directed graphs, one can measure both the in-degree and out-degree of nodes. Implementing this in NetworkX is incredibly straightforward:


```python
nx.degree(G)
```




    DegreeView({0: 3, 1: 24, 2: 13, 3: 10, 4: 5, 5: 2, 6: 9, 7: 1, 8: 4, 9: 14, 10: 7, 11: 14, 12: 7, 13: 5, 14: 20, 15: 7, 16: 8, 17: 10, 18: 21, 19: 10, 20: 26, 21: 22, 22: 10, 23: 15, 24: 13, 25: 20, 26: 8, 27: 3, 28: 10, 29: 14, 30: 7, 31: 9, 32: 4, 33: 3, 34: 14, 35: 14, 36: 10, 37: 3, 38: 12, 39: 7, 40: 11, 41: 15, 42: 5, 43: 6, 44: 7, 45: 2, 46: 6, 47: 6, 48: 1, 49: 8, 50: 8, 51: 1, 52: 5, 53: 6, 54: 4, 55: 9, 56: 5, 57: 5, 58: 10, 59: 3, 60: 16, 61: 4, 62: 3, 63: 4, 64: 1, 65: 7, 66: 5, 67: 4, 68: 3, 69: 4, 70: 12, 71: 8, 72: 5, 73: 6, 74: 15, 75: 5, 76: 4, 77: 2, 78: 5, 79: 7, 80: 6, 81: 1, 82: 5, 83: 3, 84: 4, 85: 7, 86: 1, 87: 1, 88: 5, 89: 3, 90: 3, 91: 2, 92: 1, 93: 1, 94: 1, 95: 1, 96: 1, 97: 2, 98: 1})



## Closeness Centrality

Next, let's discuss closeness. The closeness of a node is roughly the average distance from that node to any other node in the network. As such, it relies upon Dijkstra's algorithm in defining the distance between two nodes. 



```python
#The Closeness for a central node
print(nx.closeness_centrality(G, 4))
#The Closeness Metric for an ostracized node
print(nx.closeness_centrality(G, 86))
```

    0.32540074853470796
    0.01020408163265306


## Betweeness Centrality




```python
#The Betweeness Metric for a central node
print(nx.betweenness_centrality(G)[4])
#The Betweeness Metric for an ostracized node
print(nx.betweenness_centrality(G)[86])
```

    0.016522004108264238
    0.0


## Eigenvector Centrality



## Page Rank



```python
## Summary

In this lesson you investigated concepts of centrality in networks. Specifically, you took a look at four 
```
