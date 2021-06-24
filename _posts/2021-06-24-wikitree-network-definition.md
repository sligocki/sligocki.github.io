---
layout: post
title: The WikiTree Network Definition
tags: wikitree_network
---

In my [last post]({% post_url 2021-06-23-wikitree-and-network-theory %}), I discussed the motivation for applying Network Theory towards analyzing the WikiTree genealogy database. In this post, I will discus the nitty-gritty details about how to define such a network. In fact, I have come up with 3 different ways for defining such a network each of which has it's own advantages and disadvantages.

Perhaps this does not sound very interesting? "I was promised exciting discoveries not nitty-gritty network definitions!" you might say. Well, I promise, the discoveries are coming! Stick with me through this adventure because I've discovered that these details make a big difference on the analysis!

## Example Family Tree

![Example Family Tree](/assets/images/example_tree.png "Example Family Tree")
In order to visualize these different representations, let me use this simple family tree. It contains 15 people (3 generations of descendants from J & H and the spouses of those descendants). Keep this tree in mind when reading about the various network layouts I propose below. Note that I consistently use the same colors to denote each family unit and the same letters to denote individual people.

## The Person Network

![Example Person Network](/assets/images/example_person_network.png "Example Person Network")
This is the most natural way to represent the WikiTree database as a network. Each node represents a person and two nodes are connected if they represent a parent-child, sibling or spouse relationship.

Based on the 2021-06-20 data dump: This network has 23.5M nodes and 118.6M edges!

Pro:
* This is a very natural way to represent the WikiTree dataset. It is the same way that the WikiTree Connection Finder represents it. It is easy to explain to people what it means.

Con:
* This network has a **lot** of edges! Even for this small example, the blue family (2 parents and 3 children) has 10 edges. And as you scale this up to large families the number of edges grow very quickly (quadratically). For example, for a family with 2 parents and 8 children, it will have 45 edges connecting every single member of the family to each other. Mathematicians call these completely interconnected sections ["cliques"](https://en.wikipedia.org/wiki/Clique_(graph_theory)) and in addition to making visualization harder (so many crossing edges), cliques also presents a problem for many network analysis algorithms and statistics.

## The Bipartite Network

![Example Bipartite Network](/assets/images/example_bipartite_network.png "Example Bipartite Network")
In order to avoid all the cliques present in the Person Network, I thought maybe I would aim to make networks that look more like standard family tree visualizations. In the Example Family Tree above, we don't draw edges between every person in a family to every other, instead we connect them all through a common central splitting line. To mimic that, we could add a new node type to represent every "nuclear family unit". So now we have two types of nodes: Person nodes and Family nodes. In this network I will never connect any Person node directly to another Person node (and never Family to Family either). Instead I will connect every Person to all of the Families they are a part of (the one they were born into and one for every marriage/co-parentage they have). Mathematicians call this sort of network a [Bipartite network](https://en.wikipedia.org/wiki/Bipartite_graph).

This network has 30.4M nodes and 30.8M edges.

Pro:
* To me this network looks closest to how I think about a family network. If you discover a new sibling and add it to the network, you are adding one node and one edge (in contrast to the Person Network where you are adding tons of new edges for every sibling added). This feels closer to what is really happening, you have added one new person and one new connection.

Cons:
* Bipartite networks are more complicated to analyze. Many conventional algorithms do not really work on them out of the box, instead you have to search for bipartite-specific versions.
* Although we reduced the edge count 4x, we've actually increased the node count (by adding Family nodes). When I tried loading this network on my laptop it still ended up using all 30GB of RAM on my machine, woof.

## The Family Network

![Example Family Network](/assets/images/example_family_network.png "Example Family Network")

The Person nodes don't seem to be adding much information in the Bipartite Network image above. They mostly just seem to either connect to two Family nodes (if we know their parents and they were married or had children) or to a single Family node (if they never wed/had children or we don't know their parents). It seems like the Family nodes are really representing most of the interesting connectivity in this tree. So, what if we built a network with only Family nodes? Since we no longer have Person nodes, I have labeled each Family node with the people it represents (2 parents/spouses at top, children at bottom). Now, how do we draw edges on this network? For every person, we connect their childhood Family node to their adult Family node (if they ever marry/have children).

This network has 7.6M nodes and 8.1M edges.

Pro:
* This network is **dramatically** smaller and simpler. It has 1/3 as many nodes and 1/15 as many edges as the original Person Network! I almost feel embarrassed about drawing such a small network, it feels like I should have tried to represent a larger family. So this visualization allows us to represent a lot more content in a smaller space!

Con:
* Of all the networks I've discussed here, I think this one might be the least intuitive to interpret and explain. Without the Person nodes, you sort of have to re-orient your mind to thinking about family units as objects (nodes) and people as the connections (edges).

## Closing thoughts

Thanks for sticking with me through the details! In the next post, I'll share the first exciting discovery I've made which wouldn't have been possible without having these networks in mind.

---------

(Comments and discussion at [WikiTree G2G](https://www.wikitree.com/g2g/1258747/wikitree-network-defined))

---------

{% include wikitree-network-toc.html %}
