---
layout: post
title: WikiTree and Network Theory
tags: wikitree_network
---

About 3 years ago, I became interested in genealogy research and soon after discovered [WikiTree](https://www.wikitree.com/). WikiTree is a wonderful community of genealogists who collaborate to build and improve a single, shared family tree for all of humanity. At it's core is a user-built database of 27 million profiles, each representing a person (users and ancestors) and containing a biography; vital record (birth, death and marriage) details; and connections to family members (parents, children, spouses and siblings).

As a Mathematician, I quickly became very interested in thinking about this dataset as a [mathematical graph](https://en.wikipedia.org/wiki/Graph_(discrete_mathematics)) (or what is more commonly known these days as a [network](https://en.wikipedia.org/wiki/Network_theory)). For example, we could consider each profile to be a node and connect nodes together with an edge if they are direct family members (parent-child, spouse or sibling relationships, for example).

You can use this network to answer many questions about connectivity in the dataset. For example, the [WikiTree connection finder](https://www.wikitree.com/wiki/Special:Connection) is the equivalent of the [shortest path](https://en.wikipedia.org/wiki/Shortest_path_problem) in this network between any two profiles. And whether or not a profile is [connected](https://www.wikitree.com/wiki/Help:Unconnected) to the main tree is equivalent to the question: is this profile's node in the largest [connected component](https://en.wikipedia.org/wiki/Component_(graph_theory)) of the network?

But there are more complex questions you could answer with a network as well. [For example](https://www.wikitree.com/g2g/612063/mathematical-graph-structure-of-global-tree):
* What is the average distance between two randomly chosen profiles?
* What is the average distance from a specific profile to all others?
* Who is most "central" to the network (smallest average distance to all others)?
* How well connected is the network? Say how many nodes/edges would have to be removed in order to disconnect two people?
* How important are individual people or connections? How does removing a person or a connection degrade the network (disconnect people, increase average distance, etc.)?

These are all questions that can be answered by analyzing the network directly and there is a whole field of Network Theory built around answering these types of questions. Of course, before you start, you need access to the full database in order to build such a network. Luckily, WikiTree is run on a very open data model and so you can easily apply for access to weekly [Data Dumps](https://www.wikitree.com/wiki/Help:Database_Dumps). Over the last several years, I've spent evenings and weekends analyzing these data dumps, producing networks and learning Network Theory in order to try and figure out what interesting things I can say about the WikiTree Network. In future posts, I will share some of the discoveries I've made and difficulties I've run into along the way.

---------

(Comments and discussion at [WikiTree G2G](https://www.wikitree.com/g2g/1258039/wikitree-and-network-theory))

---------

{% include wikitree-network-toc.html %}
