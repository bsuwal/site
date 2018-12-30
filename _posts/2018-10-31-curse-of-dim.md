---
layout: post
title:  "A simple explanation of the Curse of Dimensionality"
categories: dimensionality
---
Keeping with the Halloween vibes of this week, I am going to write today about the Curse of Dimensionality, which essentially is a dramatic way[^bellman] of saying that higher dimension data suffer from being too far away from each other.

I am going to show you how the volume of higher dimensional data is concentrated towards the edges. 

Let's start with a simple two-dimensional square and see how much of its substance is concentrated around the edges. We are going to build one square of side $l = 10$ and another inside it of $l = 9$.

![Two squares of sides equal to 9 and 10]({{ "/assets/curse_of_dim/squares.png"}}){:class="post-img"}

For two dimensions, the shaded area will be $10^2 - 9^2 = 100 - 81 = 19$. The ratio of the area of the shaded region to that of the bigger square is 19/100 = 0.19.

Let's extend that to three dimensions. The squares become cubes and the areas become volumes, but the computation is still similar: $10^3 - 9^3 = 1000 - 729 = 271$. Ratio of the volume of the shaded part to the bigger cube: 271/1000 = 0.271.

Keep this up and we see that the ratio gets really big.

![Ratio of shaded region increasing with the number of dimensions]({{ "/assets/curse_of_dim/curse_of_dim_ratios.png"}}){:class="post-img"}

### What does this tell us?
This means that most of the "substance" of a dimension space is increasingly concentrated in the edges as the number of dimensions increase. For computational purposes I used the example of $l = 10$ here to define the upper bound of my dimension, but obviously a dimension does not stop at 10 and goes on to infinity. The most important thing to realize here is that as the number of dimensions increase, the data points are going to get farther and farther apart from each other.

### But what does this mean for me, a high-dimensional data ninja?
It means that any method that has the notion of distance performs poorly on high dimensional data. Take the k-nearest neighbors method for example. To find a point's k-nearest neigbors, I have to calculate the distance of each point in the dataset from my point. If this is very high dimensional data, then there is so much distance between the data points that distance stops being an important differentiator for classification. When the distances are so high, the choice of a nearest neighbor is effectively random. 

### How do I deal with this?
If high dimensional data seems to be a problem, then it seems plausible that a good thing to do reduce the number of dimensions! There seem to be a number of ways[^solution_post] to deal with this, but watch out for my next post on PCA, a very popular dimensionality reduction technique!

As an aside, it is very interesting how neural networks have somehow overcome the curse of dimensionality. They operate on very high number of features and yet produce excellent classifiers. I believe that why this happens is not understood very well. 

[This]: https://towardsdatascience.com/why-and-how-to-get-rid-of-the-curse-of-dimensionality-right-with-breast-cancer-dataset-7d528fb5f6c0
[Richard Bellman]: https://en.wikipedia.org/wiki/Richard_E._Bellman

<br>
<br>
<hr style="width: 100px;" />
<!-- Footnotes -->
[^bellman]: Kudos to [Richard Bellman] for this excellent spooky nomenclature.
[^solution_post]: [This] is a great blog post that discusses some of these solutions. 


