---
layout: lecture
title: Lecture 9&#58; Prufer code	
comments: True
---


In this lecture, we will prove Cayley's formula, using the Prufer code.  The  This is a bijection between labelled trees on $$n$$ vertices and strings of $$n-2$$ numbers, each between $$1$$ and $$n$$.  

Perhaps the starting observation is that to write down a labelled tree is the same as writing down its $$n-1$$ edges; the Prufer code begins by writing down rests on a clever ordering of these edges.  This ordering depends on the notion of a leaf:


Definition
====

A *leaf* in a tree is a vertex of degree 1.

Lemma
===

Every finite tree with at least two vertices has at least two leaves.

Proof
===

Suppose $$T$$ is at tree, pick any edge $$e$$, say, between $$a$$ and $$b$$.  If the vertex $$a$$ is a leaf, we have at least one leaf.  If not, there is another edge, and we can start a path from $$a$$ away from $$e$$ following this edge.  As long as the end vertex of our path is not a leaf we may continue our path, and we will never return to a vertex we have already encountered, since trees have unique paths between vertices.  Since $$T$$ is finite, the path must eventually terminate -- i.e., find a leaf. $$\square$$

Exercise:
===
Give another proof of this Lemma, using the handshaking lemma and the fact that trees with $$n$$ vertices have $$n-1$$ edges. 


The Prufer code
----

We are now ready to introduce the Prufer code.  We begin by writing down the edges of $$T$$.  The two vertices of each edge will be written down in a column, with the parent vertex in the top row and the child vertex on the bottom row.  We record the edges in the following specific order.

 First, find the lowest numbered leaf write this down.  Above it, write down the number of the vertex this leaf is adjacent to, which we call the parent vertex.  Now, delete that lowest numbered leaf and the edge connecting it to the rest of the tree.  Find the lowest leaf on the resulting vertex, and record its number, and the number of its parent, in the next column.  

Iterate this procedure until we have written down all $$n-1$$ edges of our tree, with the leaf numbers in the top row, and the parent numbers in the bottom row.

The list of the first $$n-2$$ parent numbers (i.e., all but the last), is the Prufer code of $$T$$.

Example
===

We illustrate the construction of the Prufer code by finding the code for the following labelled tree:

<a href="http://www.presheaf.com/?d=d1483r4h5s5l12l4i4g1d2w2z292r34"><img src="http://presheaf.com/cache/d1483r4h5s5l12l4i4g1d2w2z292r34.png" title="click to go to presheaf.com for editing"/></a>

The lowest leaf is 3, which is attached to 1, so the first column goes

<table>
<tr>
   <td> Parent node </td>
   <td> 1</td>
   <td> 6</td>
   <td> 6</td>
   <td> 2</td>
   <td> 2</td>
   <td> 7</td>
</tr>
<tr>
  <td> Child node </td>
   <td> 3</td>
   <td> 1</td>
   <td> 4</td>
   <td> 5</td>
   <td> 6</td>
   <td> 2</td>   
</tr>
</table>

Thus, the Prufer code for the above tree is 16622.

Reconstructing a tree from its Prufer Code
===

It is clear from the above definition that the Prufer code is a list of $$n-2$$ numbers between 1 and $$n$$; it is not clear that any such list of numbers is obtained, nor that any two trees give us a different set of numbers.  To see this, we describe an inverse algorithm, that constructs a tree on $$n$$ vertices from a Prufer code.  

More explicitly, the inverse algorithm will take as input a Prufer code, and from that Prufer code it will reconstruct the full ordered table of edges we constructed in the Prufer code algorithm.  It should be clear from the description that the algorithm actually reproduces this table, and not some other table, and hence that the two algorithms are inverse to each other.  This shows that the Prufer code is a bijection, which proves Cayley's formula, as there are $$n^{n-2}$$ valid Prufer codes on $$n$$ vertices.

This algorith proceeds by figuring out the corresponding child nodes one by one.  Recall that any number in our list appeared as a parent node, and so is not a leaf.  At the first step, we deleted the smallest leaf.  So the first child node is the smallest number that does not appear on our list.  

After we recorded that edge, we deleted it; thus, the second child number is the smallest number that we haven't 

1. already used as a leaf number   
2. doesn't appear at or after the current spot as a parent.

Example
===

We first reconstruct the tree we did in the first example, from its code.
Recall, the Prufer code was 1 6 6 2 2.

The lowest unused number is 3, so that is the first child.

To find the next unused number, we move to the second column.  1 only appears in the first column, and so it is now the lowest number that doesn't appear, and so it goes underneath the first 6.  Moving to the third column, we have already used 1 and 3 as child nodes.  The number 2 is still to appear as a parent, and so can't be a leaf yet, and so 4 is the first number that we haven't used yet.  Similar reasoning gives 5 and 7 for the 4th and 5th column.  

Finally, the last remaining edge connects the two nodes we have not used as leaves yet; in this case 2 and 6.  

Introduction to optimisation problems
----

One motivation for introducing trees was as the "cheapest" way of connecting $$n$$ points.  Here, "cheapest" just means the least number of edges.  In real world applications, not all edges are created equal.  For example, consider the case where the vertices of $$\Gamma$$ represent cities, and the edges are roads connecting them.  If we're looking for the shortest path between two cities, we do not just want the least number of edges, as some roads will be longer than others, or be busy and take longer to drive.  These subtleties can be addressed with a *weighted graph*.

Definition
===

A *weighted graph* is a graph $$\Gamma$$, together with a non-negative real number $$w(e)$$ for each edge $$e\in E(\Gamma)$$.

Example
===

Typically, weighted graphs are presented by drawing labelling each edge of the graph with its weight:

![Example of a weighted graph](../Slides/Pictures/weightedgraph.png)

Real world examples of weights
===
Even in the case where the vertices of $$\Gamma$$ are cities and the edges are conenctions between them, there are many possible interpretations of edges weights:
 - The edge weights $$w(e)$$ might represent the cost of building or maintaining the road between the city
 - The edge weights migth represent the distance between the cities
 - The edge weights might represent travel times between the cities
 - the edge weights might represent the cost of a train/plane ticket between the cities

In the next few class lectures, we will discuss the following optimisation problems for weighted graphs:

 - The *minimal spanning tree* -- finding a spanning tree $$T$$ of $$\Gamma$$ where the total cost of the edges in $$T$$ is the cheapest among all spanning trees of $$\Gamma$$.
 - The *shortest path* -- finding a path between two vertices of $$\Gamma$$, where the total weight of all the edges in the path is minimal among all paths between the two vertices.
 - The *traveling salesman problem* -- finding a hamiltonian cycle in a graph $$\Gamma$$ where the total weight of all the edges is minimal.