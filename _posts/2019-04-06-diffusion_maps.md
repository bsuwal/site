---
layout: post
title:  "Diffusion Maps I: The Diffusion"
categories: dimensionality
comments: true
---
This is Part 1 of a two-part series explaining diffusion maps. Diffusion maps is a dimension reduction technique that I think is so, so cool. In this part we will only be talking about the diffusion part of diffusion maps, where we model how diffusion occurs in graphs. 

### Diffusion helps us answer the question: If I were to start at point A, what is the probability that I end up at point Z after t timesteps?

The roots of the terminology ‘diffusion maps’ comes from physics, I believe. If you release a gas at point A, how can I model how the gas is going to diffuse over time? How do I know where the gas has reached 3 seconds after releasing it? 10 seconds after releasing it? 100 seconds? 

### Why do I care about diffusion?
If modeling gas diffusion doesn't sound like the most exciting thing to you, fear not. The idea of how modeling how gases diffuse in the air is generalizable to how any information is disseminated in a graph, and thus is applicable to any graph-based problem. Some non-gas use cases that I can think of right now are:

* The famous PageRank algorithm that Google originally used to rank search results used a diffusion-like crawl over the internet to rank web-pages!
* You could model how news spreads in a social network. If I plant some fake news that goes viral on my feed this second, I could model how many people saw it in 3 days. 
* You could use this as a clustering technique, in the sense that the you will diffuse to points close to you than to points farther. This is the foundation of a popular method called spectral clustering.
* A weird (and very unrealistic) one but bear with me. Say a friend of yours is backpacking across Asia for a year without a phone and there is no way for you to get in touch with them, but you want to throw them a surprise birthday party. How would you know which city your friend is going to be in? Using diffusion, you could actually make an educated guess on which city your friend will be on their birthday! [^stalker]

### The framing of the graph problem
If readers are familiar with the term _random walk_, then that is essentially how we model diffusion. Let us start with visualizing diffusion on the following graph:
![Example-graph]({{ "/assets/diffusion_maps/graph.png"}}){:class="post-img"}

The little swirls around the nodes pointing to themselves means that you have the option of staying at the same node in the next time step.

The adjacency matrix for this graph is:
\begin{bmatrix}
    1 & 1 & 1 & 0 & 0 & 0 \\\
    1 & 1 & 1 & 0 & 0 & 0 \\\
    1 & 1 & 1 & 1 & 0 & 0 \\\
    0 & 0 & 1 & 1 & 1 & 1 \\\
    0 & 0 & 0 & 1 & 1 & 1 \\\
    0 & 0 & 0 & 1 & 1 & 1 \\\
\end{bmatrix}

The adjacency matrix essentially tells you whether you are connected to any other node in the graph. The first row corresponds to A, the second row corresponds to B, and so on. Same for columns: the first column is for A, the second column is for B, and so on. For example, The 1 on the top left means that A is connected to A, which that little swirl around A denotes. The next two 1s on the first row denote A's connections to B and C.

### The transition matrix
Next we are going to convert this adjacency matrix into a transition matrix, where instead of each slot denoting the existence of a connection, we instead put in the probability of going to another node at the next time step. The transition matrix $P$ tells you the probability of going to a node in the next time step[^transition_matrix]: 

\begin{equation}
P = 
\begin{bmatrix}
    1/3 & 1/3 & 1/3 & 0 & 0 & 0 \\\
    1/3 & 1/3 & 1/3 & 0 & 0 & 0 \\\
    1/4 & 1/4 & 1/4 & 1/4 & 0 & 0 \\\
    0 & 0 & 1/4 & 1/4 & 1/4 & 1/4 \\\
    0 & 0 & 0 & 1/3 & 1/3 & 1/3 \\\
    0 & 0 & 0 & 1/3 & 1/3 & 1/3 \\\
\end{bmatrix}
\end{equation}

Say we start at node A. When we start, we know we are at node A with a probability of 1. Our position vector would be:

\begin{equation}
 x_{0} = 
\begin{bmatrix}
    1\\\
    0\\\
    0\\\
    0\\\
    0\\\
    0\\\
\end{bmatrix}
\end{equation}

I want to know what the probabilities of where I will be at the next time step. To transition into the next time step, I slap my current position vector with the _transpose_ of the transition matrix, $P_{T}$. The reason why we use the transpose will be apparent in a bit.

Then, the position vector of the next time step $x_{1}$ would be $P^{T}x_{0} = x_{1}$

\begin{equation}
P^{T}x_{0} = 
\begin{bmatrix}
    1/3 & 1/3 & 1/4 & 0 & 0 & 0 \\\
    1/3 & 1/3 & 1/4 & 0 & 0 & 0 \\\
    1/3 & 1/3 & 1/4 & 1/4 & 0 & 0 \\\
    0 & 0 & 1/4 & 1/4 & 1/3 & 1/3 \\\
    0 & 0 & 0 & 1/4 & 1/3 & 1/3 \\\
    0 & 0 & 0 & 1/4 & 1/3 & 1/3 \\\
\end{bmatrix}
\begin{bmatrix}
    1\\\
    0\\\
    0\\\
    0\\\
    0\\\
    0\\\
\end{bmatrix}
= 
\begin{bmatrix}
    1/3\\\
    1/3\\\
    1/3\\\
    0\\\
    0\\\
    0\\\
\end{bmatrix}
= x_{1}
\end{equation}

The reason we multiplied it with $P^{T}$ and not $P$ was to make the numbers make sense - we essentially wanted to pluck out the first row of $P$, and multiplying $x_{0}$ by $P^{T}$ gives us just that. 

So far, we know that if we were to start at A, then in one time step we arrive at either A, B or C. Nothing too interesting, _yet_.


**What if we want to know where we could be in two time steps?**\\
That is easy. We know where we are at $x_{1}$. To get $x_{2}$, so we could just slap $x_{1}$ with $P^{T}$ again[^no_graph_change], like we did earlier:

\begin{equation}
P^{T}x_{1} = 
\begin{bmatrix}
    1/3 & 1/3 & 1/4 & 0 & 0 & 0 \\\
    1/3 & 1/3 & 1/4 & 0 & 0 & 0 \\\
    1/3 & 1/3 & 1/4 & 1/4 & 0 & 0 \\\
    0 & 0 & 1/4 & 1/4 & 1/3 & 1/3 \\\
    0 & 0 & 0 & 1/4 & 1/3 & 1/3 \\\
    0 & 0 & 0 & 1/4 & 1/3 & 1/3 \\\
\end{bmatrix}
\begin{bmatrix}
    1/3\\\
    1/3\\\
    1/3\\\
    0\\\
    0\\\
    0\\\
\end{bmatrix}
= 
\begin{bmatrix}
    0.3056\\\
    0.3056\\\
    0.3056\\\
    0.0833\\\
    0\\\
    0\\\
\end{bmatrix}
= x_{2}
\end{equation}

Let's pause and think if the numbers in $x_{2}$ make sense for a bit. Representing each 'hop' or time-step as a '->', let's see all the different nodes we can arrive to in two time-steps.

**Ways we can arrive at A:**
* A -> A -> A 
* A -> B -> A
* A -> C -> A

Sum of probabilities: $1/3 \times 1/3 + 1/3 \times 1/3 + 1/3 \times 1/4 = 0.3056$

**Ways we can arrive at B:**
* A -> A -> B
* A -> B -> B
* A -> C -> B

Sum of probabilities: $1/3 \times 1/3 + 1/3 \times 1/3 + 1/3 \times 1/4 = 0.3056$

**Ways we can arrive at C:**
* A -> A -> C
* A -> B -> C
* A -> C -> C

Sum of probabilities: $1/3 \times 1/3 + 1/3 \times 1/3 + 1/3 \times 1/4 = 0.3056$

**Ways we can arrive at D:**
* A -> C -> C

Sum of probabilities: $1/3 \times 1/4 = 0.0833$

This makes sense intuitively. A,B and C are more "densely" connected, so if I start in that cluster I should probably end up somewhere in the cluster.

Notice what we did though. 

$x_{2} = P^{T}x_{1} = P^{T}P^{T}x_{0} = (P^{T})^{2}x_{0} = (P^{2})^{T}x_{0}$

This means that if we want to know where we will be in $k$ time steps, we can do that with the computation $(P^{k})^{T}x_{0}$. At each time step we diffuse a little bit more. This is how we model diffusion. Very cool!!!

### Tying it back to our earlier examples
Let us see how this diffusion process represents our earlier examples.

* The PageRank algorithm was trying to solve the problem of ranking web pages by their relative importance. This is roughly what they were thinking: The more important web-pages are the ones that have the more important pages linking to them. At each step of the diffusion, you traverse a hyperlink on a web-page to follow where it leads to.
* In a social network, the edges could be whether a person is connected to another person. If you have spicy gossip and you introduce it to the network, the transition probabilities are the chances that a person has heard the gossip.
* You can see why this is useful for clustering. In one time-step, you probably travel to your nearest neighbors. The distance information is just disguised as probabilities.
* Each time step in diffusion lets you update the probabilities which city your friend probably is in. 


### Extra credit 
We have discussed how diffusion occurs in a graph, but let us now talk about the somewhat interesting case we let the diffusion occur for a long time. Over many time steps, you would expect 
* gas to be completely diffused in the air
* information/news to be fully disseminated in the social graph

You would expect that over a long enough time period, your diffusion would hit all nodes even though they had low probability edges initially. Then, you would also expect that we would reach an equilibrium in our graph where $P^{k}$ stops changing for additional $k$, for example after a viral piece of news is completely disseminated, we would expect the information dissemination in 1000 days to be the same as in 1001 days. 

I run the following snippet of Matlab code for our example graph above:

```Matlab
% this is our adjacency matrix for our example above
A = [1 1 1 0 0 0; 1 1 1 0 0 0; 1 1 1 1 0 0; 0 0 1 1 1 1; 0 0 0 1 1 1; 0 0 0 1 1 1]';
D = diag(sum(A,2));

% this is the transition matrix
P = inv(D) * A;

% transition a 100 times
num_time_steps = 100;
P^num_time_steps
```

and this gives me the following output:

\begin{equation}
P^{100} = 
\begin{bmatrix}
    0.15 & 0.15 & 0.20 & 0.20 & 0.15 & 0.15 \\\
    0.15 & 0.15 & 0.20 & 0.20 & 0.15 & 0.15 \\\
    0.15 & 0.15 & 0.20 & 0.20 & 0.15 & 0.15 \\\
    0.15 & 0.15 & 0.20 & 0.20 & 0.15 & 0.15 \\\
    0.15 & 0.15 & 0.20 & 0.20 & 0.15 & 0.15 \\\
    0.15 & 0.15 & 0.20 & 0.20 & 0.15 & 0.15 \\\
\end{bmatrix}
\end{equation}

Notice that this is independent of what $x_{0}$ is. That means that no matter which node you start from, you have the same probability distribution for your future position! This should make sense, because over a long enough time we would expect to diffuse over the entire graph no matter where we start.

Another interesting tidbit: notice that nodes A, B, E and F have 0.15 as the probability whereas nodes C and D have a slightly higher probability 0.2. It happens that this probability is dependent only on the number of edges of each node, and since there are more ways to get to C and D, you have a higher chance of ending up there in the limit. Out of the 20 edges in the graph, C and D have 4 each, so the chances of ending up there are 4/20 = 0.20. Likewise, for A, B, E and F it is 3/20 = 0.15. Neat stuff!

<br>
<hr style="width: 100px;" />
<!-- Footnotes -->
[^stalker]: This is why we don’t want stalkers to also be mathematicians.
[^transition_matrix]: It is a slight pet peeve of mine that the transition matrix seems to referred by $P$ and not $T$ in most literature. The $P$ of course stems from the fact that the transitions are just probabilities.
[^no_graph_change]: Note that in assuming we can keep applying the same $P$ matrix again and again means that we are implying that our transition matrix $P$ doesn't change, which means that we assume that our graph doesn't change at any point. 