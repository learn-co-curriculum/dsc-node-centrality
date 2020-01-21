
# Node Centrality 

## Introduction

Dijkstra's algorithm and other path related searches are essential for establishing a common distance metric within network graphs. With that, one can investigate relationships between nodes. For example, in the context of social networks, one might wonder who is most influential to various social circles. Alternatively, if you were studying the spread of disease, it would be important to identify key stakeholders for quarantining the disease off from subpopulations. The umbrella term for these metrics is known as centrality. In this lesson, you'll take a look at four measures of centrality and how they can collectively uncover important relationships within networks.

## Objectives

You will be able to:

- Describe the use case for the centrality measures 
- Explain network centrality and its importance in graph analysis 
- Compare and calculate degree, closeness, betweenness, and eigenvector centrality measures 

## Centrality

An important concept in graphs is centrality. Take the social network below, created from a subset of twitter data. Some of the nodes are enmeshed in the network, with many connections amongst other nodes. Others, such as node 95, are tied to this central hub, but only weakly through a single connection to the main hub. Two nodes, 86 and 87, aren't even directly tied to the main cluster, and are floating on an island on their own. By quantifying the relationships between nodes, one can better understand the underlying structure of networks.


```python
import networkx as nx
import matplotlib.pyplot as plt
%matplotlib inline
```


```python
# Load the Network from File
G = nx.read_edgelist('twitter.edges')
# Simplify the Node Labels
G = nx.relabel_nodes(G, dict(zip(G.nodes, range(len(G.nodes)))))
# Create a matplotlib figure
fig = plt.figure(figsize=(15,10))
# Draw the network!
nx.draw(G, pos=nx.spring_layout(G), with_labels=True,
        alpha=.8, node_color='#1cf0c7', node_size=700)
```

    //anaconda3/lib/python3.7/site-packages/networkx/drawing/nx_pylab.py:579: MatplotlibDeprecationWarning: 
    The iterable function was deprecated in Matplotlib 3.1 and will be removed in 3.3. Use np.iterable instead.
      if not cb.iterable(width):



![png](index_files/index_2_1.png)


## Degree Centrality 

The most primitive measure of centrality is degree centrality. This is simply the number of edges attached to a node. In directed graphs, one can measure both the in-degree and out-degree of nodes. Implementing this in NetworkX is incredibly straightforward:


```python
nx.degree(G)
```




    DegreeView({0: 3, 1: 24, 2: 13, 3: 10, 4: 5, 5: 2, 6: 9, 7: 1, 8: 4, 9: 14, 10: 7, 11: 14, 12: 7, 13: 5, 14: 20, 15: 7, 16: 8, 17: 10, 18: 21, 19: 10, 20: 26, 21: 22, 22: 10, 23: 15, 24: 13, 25: 20, 26: 8, 27: 3, 28: 10, 29: 14, 30: 7, 31: 9, 32: 4, 33: 3, 34: 14, 35: 14, 36: 10, 37: 3, 38: 12, 39: 7, 40: 11, 41: 15, 42: 5, 43: 6, 44: 7, 45: 2, 46: 6, 47: 6, 48: 1, 49: 8, 50: 8, 51: 1, 52: 5, 53: 6, 54: 4, 55: 9, 56: 5, 57: 5, 58: 10, 59: 3, 60: 16, 61: 4, 62: 3, 63: 4, 64: 1, 65: 7, 66: 5, 67: 4, 68: 3, 69: 4, 70: 12, 71: 8, 72: 5, 73: 6, 74: 15, 75: 5, 76: 4, 77: 2, 78: 5, 79: 7, 80: 6, 81: 1, 82: 5, 83: 3, 84: 4, 85: 7, 86: 1, 87: 1, 88: 5, 89: 3, 90: 3, 91: 2, 92: 1, 93: 1, 94: 1, 95: 1, 96: 1, 97: 2, 98: 1})



## Closeness Centrality

Next, let's discuss closeness. The closeness of a node is roughly the average distance from that node to any other node in the network. As such, it relies upon Dijkstra's algorithm in defining the distance between two nodes. 



```python
# The Closeness for a central node
print(nx.closeness_centrality(G, 4))
# The Closeness Metric for an ostracized node
print(nx.closeness_centrality(G, 86))
```

    0.32540074853470796
    0.01020408163265306


## Betweenness Centrality

Rather then simply looking at the distance between nodes, betweenness centrality investigates whether a node is a key stepping stone in moving between nodes. More specifically, betweenness investigates the number of shortest paths that a node lies on. To calculate betweenness, you must first calculate the shortest path between all node pairs using Dijkstra's algorithm. From there, one counts the number of paths from this output that the node in question lies on. Finally, this number is then normalized to be on a scale from 0 to 1. In other words, in order to compute betweenness for a single node, one must count the number of shortest paths each node lies on. The normalization from 0 to 1 involves subtracting the minimum number of paths any node in the network lies on and dividing by the maximum number of paths any node lies on.


```python
# The Betweeness Metric for a central node
print(nx.betweenness_centrality(G)[4])
# The Betweeness Metric for an ostracized node
print(nx.betweenness_centrality(G)[86])
```

    0.016522004108264238
    0.0


## Eigenvector Centrality

Eigenvector centrality is an iterative algorithm that attempts to measure a node's relative influence in the network. The underlying motivation is that degree centrality can be refined to incorporate the relative importance of neighboring nodes. In other words, a connection to a central node is more important then a connection to an isolated one. As with the previous measures of centrality, NetworkX makes calculating the eigenvector centrality quite easy.


```python
# The eigenvector Metric for a central node
print(nx.eigenvector_centrality(G)[4])
# The eigenvector Metric for an ostracized node
print(nx.eigenvector_centrality(G)[86])
```

    0.028875860204350325
    2.505996464597042e-27


## Putting it All Together

With that, let's investigate some of these measures for the twitter network displayed above. To do this, we'll take a look at a few different types of nodes in the network that we will categorize based on their position relative to all the other points in the network. How would you expect the centrality metrics to differ between these various groups?

<img src="images/node_examples.png">


```python
import pandas as pd

degrees = nx.degree_centrality(G)
closeness = nx.closeness_centrality(G)
betweeness = nx.betweenness_centrality(G)
eigs = nx.eigenvector_centrality(G)
df = pd.DataFrame([degrees, closeness, betweeness, eigs]).transpose()
df.columns = ['degrees', 'closeness', 'betweeness', 'eigs']
# Some Nodes to Investigate
islanders = [86, 87]
penisulas = [51, 92, 98, 95]
bridges = [52, 96]
periphial = [57, 97]
centers = [74, 3, 20]
temp = {'islanders': islanders,
       'penisulas': penisulas,
       'bridges': bridges,
       'periphial': periphial,
       'centers': centers}
node_label_dict = {}
for label in temp.keys():
    nodes = temp[label]
    for node in nodes:
        node_label_dict[node]=label
ex_nodes = islanders + penisulas + bridges + periphial + centers
df['group'] = df.index.map(node_label_dict)
df.iloc[ex_nodes]
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>degrees</th>
      <th>closeness</th>
      <th>betweeness</th>
      <th>eigs</th>
      <th>group</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>86</th>
      <td>0.010204</td>
      <td>0.010204</td>
      <td>0.000000</td>
      <td>2.505996e-27</td>
      <td>islanders</td>
    </tr>
    <tr>
      <th>87</th>
      <td>0.010204</td>
      <td>0.010204</td>
      <td>0.000000</td>
      <td>2.505996e-27</td>
      <td>islanders</td>
    </tr>
    <tr>
      <th>51</th>
      <td>0.010204</td>
      <td>0.271794</td>
      <td>0.000000</td>
      <td>2.311366e-03</td>
      <td>penisulas</td>
    </tr>
    <tr>
      <th>92</th>
      <td>0.010204</td>
      <td>0.271794</td>
      <td>0.000000</td>
      <td>2.311366e-03</td>
      <td>penisulas</td>
    </tr>
    <tr>
      <th>98</th>
      <td>0.010204</td>
      <td>0.271794</td>
      <td>0.000000</td>
      <td>2.311366e-03</td>
      <td>penisulas</td>
    </tr>
    <tr>
      <th>95</th>
      <td>0.010204</td>
      <td>0.245537</td>
      <td>0.000000</td>
      <td>1.181819e-03</td>
      <td>penisulas</td>
    </tr>
    <tr>
      <th>52</th>
      <td>0.051020</td>
      <td>0.374665</td>
      <td>0.059331</td>
      <td>2.715085e-02</td>
      <td>bridges</td>
    </tr>
    <tr>
      <th>96</th>
      <td>0.010204</td>
      <td>0.304339</td>
      <td>0.000000</td>
      <td>8.021756e-03</td>
      <td>bridges</td>
    </tr>
    <tr>
      <th>57</th>
      <td>0.051020</td>
      <td>0.318782</td>
      <td>0.001811</td>
      <td>7.223395e-02</td>
      <td>periphial</td>
    </tr>
    <tr>
      <th>97</th>
      <td>0.020408</td>
      <td>0.320958</td>
      <td>0.000326</td>
      <td>2.937446e-02</td>
      <td>periphial</td>
    </tr>
    <tr>
      <th>74</th>
      <td>0.153061</td>
      <td>0.452119</td>
      <td>0.054725</td>
      <td>1.593317e-01</td>
      <td>centers</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.102041</td>
      <td>0.400174</td>
      <td>0.013053</td>
      <td>1.024298e-01</td>
      <td>centers</td>
    </tr>
    <tr>
      <th>20</th>
      <td>0.265306</td>
      <td>0.508329</td>
      <td>0.146951</td>
      <td>2.782426e-01</td>
      <td>centers</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.groupby('group').mean()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>degrees</th>
      <th>closeness</th>
      <th>betweeness</th>
      <th>eigs</th>
    </tr>
    <tr>
      <th>group</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>bridges</th>
      <td>0.030612</td>
      <td>0.339502</td>
      <td>0.029665</td>
      <td>1.758630e-02</td>
    </tr>
    <tr>
      <th>centers</th>
      <td>0.173469</td>
      <td>0.453541</td>
      <td>0.071576</td>
      <td>1.800014e-01</td>
    </tr>
    <tr>
      <th>islanders</th>
      <td>0.010204</td>
      <td>0.010204</td>
      <td>0.000000</td>
      <td>2.505996e-27</td>
    </tr>
    <tr>
      <th>penisulas</th>
      <td>0.010204</td>
      <td>0.265230</td>
      <td>0.000000</td>
      <td>2.028979e-03</td>
    </tr>
    <tr>
      <th>periphial</th>
      <td>0.035714</td>
      <td>0.319870</td>
      <td>0.001069</td>
      <td>5.080421e-02</td>
    </tr>
  </tbody>
</table>
</div>



As you can see, the central nodes have the highest measures of centrality across the board. Interestingly, the "bridge" nodes, which connect some outside nodes to the center. have a fairly high level of betweenness due to their importance in maintaining this intermediate relationship. Additionally, of all the center nodes, node 20 appears to be particularly influential given its exceedingly large betweenness centrality.

## Summary

In this lesson you investigated the concept of centrality in networks. Specifically, you took a look at four measures of centrality: degree, closeness, betweenness, and eigenvector centrality. From here, you'll further investigate these concepts and their interpretation in the context of a real world social network.
