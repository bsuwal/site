---
layout: post
title:  "Understanding the Poisson Distribution Formula"
categories: probability
---
The Poisson probability has a very unintuitive formula:
\begin{equation}
P(X = k) = \frac{\lambda^{k}}{k!}e^{-\lambda}
\end{equation}
where $\lambda$ is the expected number of successes. Let's unpack this.

Say you own a shoe shop. You have noticed that on average 10 customers come to your shop in an hour. But sometimes more than 10 customers show up within an hour, and sometimes less than 10. How could you find the probability that exactly 7 customers show up in any chosen hour, given that you know that the customers show up at random?

You could choose to do something like this. \\
You decide to split the hour into 60 minutes and calculate the probability of getting a customer at each minute. You want the probability of getting $\lambda$ customers in 60 minutes so you have $\lambda$ success events and 60-$\lambda$ failure events. Accounting for all the different combinations these events can take place, you get the probability of getting exactly $\lambda$ customers in your store.

![Poisson-as-binomial]({{ "/assets/poisson-binomial.png"}}){:class="post-img"}

                    Aka you use the Binomial Distribution.

Which is all good and nice, but there is a flaw here. Notice that $\lambda/60$ is the probability of a minute being successful. A minute is successful if a customer enters the shop. *Notice that we are not keeping track of the number of customers that entered the shop, but actually the number of successful minutes we have had.* If two or more customers enter the shop in the same minute, it still counts as one successful minute. 

A simple solution is to get more granular, and measure the probability that a customer comes in a given second instead of a minute. That's 3600 partitions of an hour.

\begin{equation}
P(X = k) = {3600\choose k} \cdot \Big(\frac{\lambda}{3600}\Big)^k \cdot \Big(1- \frac{\lambda}{3600}\Big)^{3600-k}
\end{equation}

But then we encounter the same problem. What if two or more customers enter in the same second? So we get as granular as we can get and split an hour into as many intervals as we can i.e. we take the number of intervals to the limit. Which is to say that instead of 60 or 3600 partitions, we make $n$ partitions and take the limit of $n$. At that level of granularity, the probability that two customers enter at the same time is basically 0, so counting the number of successful time intervals works.

\begin{equation}
\begin{split}
P(X = k) & = lim_{n\rightarrow\infty} {n\choose k} \cdot \Big(\frac{\lambda}{n}\Big)^k \cdot \Big(1- \frac{\lambda}{n}\Big)^{n-k}
	\newline
    & = lim_{n\rightarrow\infty} \frac{n!}{(n-k)!k!} \cdot \Big(\frac{\lambda}{n}\Big)^k \cdot \Big(1- \frac{\lambda}{n}\Big)^{n-k} 
    \newline
    & = lim_{n\rightarrow\infty} \frac{n!}{(n-k)!k!} \cdot \Big(\frac{\lambda}{n}\Big)^k \cdot \Big(1- \frac{\lambda}{n}\Big)^{n-k}
    \newline
    & = lim_{n\rightarrow\infty} {\color{red} \frac{n \cdot (n-1) \cdot (n-2) ... (n-k+1)}{k!} } \cdot \frac{\lambda^k}{n^k} \cdot \Big(1- \frac{\lambda}{n}\Big)^{n-k}
    \newline
    & = lim_{n\rightarrow\infty} {\color{red} \frac{n^k + ...}{n^k}} \cdot \frac{\lambda^k}{k!} \cdot \Big(1- \frac{\lambda}{n}\Big)^{n} \cdot \Big(1- \frac{\lambda}{n}\Big)^{-k}
    \newline
    & = lim_{n\rightarrow\infty} 1 \cdot \frac{\lambda^k}{k!} \cdot {\color{blue} lim_{n\rightarrow\infty} \Big(1- \frac{\lambda}{n}\Big)^{n} }\cdot lim_{n\rightarrow\infty} \Big(1- \frac{\lambda}{n}\Big)^{-k}
	\newline
	& = \frac{\lambda^k}{k!} \cdot {\color {blue} e^{-\lambda}} \cdot 1 
	\newline
	& = \frac{\lambda^k}{k!} \cdot e^{-\lambda}
\end{split}
\end{equation}

Which is how we get the Poisson probability! Woohoo! 

**Notes:**\\
${\color{red} !!!}$ Notice that the term $n$ on the upper line appears k times, so when the expression is multiplied out we get $n^k$. This term dominates as $n$ increases so for large $n$ we can ignore the other terms. \\
${\color{blue} !!!}$ We get this from the definition of $e$. Remember, $e = lim_{n\rightarrow\infty} \Big(1 + \frac{1}{n}\Big)^{n}$ and $e^{\lambda} = lim_{n\rightarrow\infty} \Big(1 + \frac{\lambda}{n}\Big)^{n}$  


**Recommended references:**
Khan Academy has a great video on this.
\\
TODO: Add link to Khan Academy \\
Cool applications of the Poisson distribution in my daily life \\
Requirements of a Poisson Dist \\

