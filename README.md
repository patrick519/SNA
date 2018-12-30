# SNA
Jupyter notebooks related to Social Network Analysis based on twitter data or flow networks. An Algorithm design for measuring embeddedness in weighted flow networks is introduced.

# Embeddedness ALGORITHM (JongP_X)
We will start with some observations about communities and how they influence the embeddedness score. Then we describe out actual used algorithm called CalcEmbeddedness. Next we discuss the algorithm’s complexity and some optimization suggestions.
## Observations
In the context of network, communities refer to the occurrence of nodes that are more densely connected. If we calculate the embeddedness score (e) for some node v in a community, we know that the outcome lies between zero and one, and the distribution of scores will be right skewed to- wards one. This is certainly because there should only be a few nodes in a (strong) community that are connected to nodes outside the community. We can therefore assume that if a node has no links from outside the community, the embeddedness score equals one. In addition, we work in di- rected networks with the algorithm. The degree of nodes in the directed network can be divided between in and out degree, and if a node v does not have an out-degree then the embeddedness score will also be zero.
## Algorithm
The derived observations as mentioned above can be used to design a very small algorithm, by reducing the number of checks. If a link does not point to a node within the community, it should logically fall within the external (k) group. And if the out-degree is equal to 0, the calculation of the embeddedness score will have no meaning. The Pseudocode for our algorithm is given in Algorithm 2.
After setting some initial value (line 2), in the main two for loops, our algorithm repeatedly selects a node v from the communities dict, which initially contains all the found clusters derived from Louvain Modularity or the Infomap algorithm. The algorithm then counts the number of neighbors inside the community of node v (lines 6-8), and if the node has outgoing edges, the embeddedness score is calculated and added to the scoring list (lines 9-11). Note that we assume that no exceptions are possible because the communities network should be generated without errors. Finally, the algorithm stops when all nodes in all communities have been examined, and so the list of scores is returned (line 12).
### Algorithm CalcEmbeddedness
<b>Input:</b> dict communities, networkX Graph </br>
<b>Output:</b> list embeddednessScores

>function CalcEmbed(Graph G, dict Communities)</br>
>embeddednessScores ← []</br>
>for cluster in communities do</br>
>for node in communities[cluster] do</br>
>kInt ← 0</br>
>for neighbor in G.successor(node) do</br>
>if neighbor in communities[cluster] then</br>
>kInt ++</br>
>if G.outdegree(node) != 0 then</br>
>embeddedness ← kInt / Outdegree</br>
>embeddednessScores ← embeddedness</br>
>return embeddednessScores</br>
 
