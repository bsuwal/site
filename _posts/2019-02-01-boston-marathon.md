---
layout: post
title:  "People want to run sub 4-hour marathons"
categories: exploratory
comments: true
---
**If you wish to, [support me] on my Boston Marathon run this year!**
<hr style="width: 100px;" />

I ran 18 miles for the first time ever last weekend and was absolutely gassed. I struggle a lot with pacing - what pace should I aim for at different stages of the run? I don't want to run too fast in the beginning and run out of fuel at the end [^pyakhov], but what's a good pace for a semi-fit 23 year old male? My masculinity is just toxic enough that I don't want to be at the tail end of the race in the midst of 80 year olds.  

So I got hold of [a Kaggle dataset] of the Boston Marathon finishers of 2016 and 2017, and went down a little rabbit hole of interesting findings.

I first found this little gem:

![Histogram of Boston Marathon Finishers]({{ "/assets/marathon_analysis/histogram.jpg"}}){:class="post-img"}

That drop from right before the 4 hour mark to right after is noticeably sharp. The decline in numbers is so steep at a very conveniently round number. Suspicious?

My theory is that people make it a personal goal to finish within 4 hours. I can definitely see that people would infintely prefer a time of 03:58:00 than a time of 4:02:00, and care nowhere near the same for a time of 04:14:00 vs a 04:18:00. The former is a badge of honor, the latter is a unglamorous statistic. [Update: I have learned that [4 hour marathons are a thing.]]

The awesome thing about that dataset was that it gives us the times at which a runner reaches the 5K mark, the 10K mark, the 15K mark ... all the way upto the 40K mark and the finish time at 42.2K. I used these times to calculate the average pace of each runner over the last 5K at each mark, except for the finishing pace where I only calculated the pace for the last 2.2K (1.36 miles). I also ignored the pace at 5K because the start of the marathon is messy - not all runners start at 0:00:00 because the runners start according to tiers that they are assigned to based on their qualifying times. Next, I divided the runners into groups by their finishing times at intervals of five minutes. For example, if a runner finished at 3:26:29, then they are in a group of runners who finished between 3:25:00 and 3:30:00. [Code]

And that gave me this:
![Pace lineplots of all finishers]({{ "/assets/marathon_analysis/all_runners_lineplot.jpg"}}){:class="post-img"}

#### Some observations:

- Runners seem to slow down over the course of the marathon (unsurprising). 
We get this by noticing a general upward bend in the lines.
- Runners who finish later seem to slow down earlier than those who finish later (again, unsurprising). Observe the lines at the very bottom, which seem to stay flat upto about 25K and compare them to lines arouund the middle, which start bending upwards much earlier.
- It seems that at some point after the 35K mark, the runners start running faster until they hit the finish line. I originally chalked this down to the motivation that comes with the end being so close, but upon further digging I realized that there are four hills at Newton starting right after the 25K mark, ending with the infamous Heartbreak Hill at around 33K. The slow down is because of the hills, and the faster pace after 35K is because it is all downhill from Heartbreak Hill into Cleveland Circle.
- The last 2.2K (1.3 miles) are even faster even though those miles are at the same elevation. There is no other explanation than the motivation of finishing and the encouragement of the crowd at the finish line!

![Marathon Map]({{ "/assets/marathon_analysis/bos_mar_map.jpg"}}){:class="post-img"}

                            Image via https://www.baa.org/

But we were talking about people hustling to make the 4-hour mark, so lets zoom into that lineplot a bit:
![Pace lineplots of finishers around the 4 hour mark]({{ "/assets/marathon_analysis/4_hr_mark.jpg"}}){:class="post-img"}

The drop in the last 2.2K for the runners that finished between 3:55:00 and 4:00:00 (the green line) is distinctly noticeable, especially compared to the other lines that seem to be parallel at that part of the graph.

The zoom-in into the 3-hour and 5-hour marks offer a similar story:

![Pace lineplots of finishers around the 3 hour mark]({{ "/assets/marathon_analysis/3_hr_mark.jpg"}}){:class="post-side-img"}
![Pace lineplots of finshers around the 3 hour mark]({{ "/assets/marathon_analysis/5_hr_mark.jpg"}}){:class="post-side-img"}

The runners getting in right before the 3 hour mark are the only ones actually speeding up in that last 2.2K. And that is some clear hustle shown by that green line to finish below 5 hours.

#### I have learnt the secret to winning the Boston Marathon 2019
I'm going to round this off with this interesting nugget. Go back to the figure with the lineplots of all the marathon finishers and look at the bottom-most one. That orange line represents the time of only two runners over the marathons who finished in the 2:05:00-2:10:00 range, both in 2017. Notice that these two absolute beasts _actually sped up_ at Heartbreak Hill (30K-35K) while the rest struggled. Crazy.

<br>
<hr style="width: 100px;" />
<!-- Footnotes -->
[^pyakhov]: Our Nepali textbook at some point in school had this Russian story translated in Nepali about this guy named Pyakhov who entered a race where all the land one could cover in a day would be theirs. Pyakhov was greedy and ran wayyy too much and collapsed at the end of the day and died. I think about Pyakhov a lot these days.

[4 hour marathons are a thing.]: https://www.realbuzz.com/articles-interests/running/article/tips-on-how-to-run-a-sub-4-hour-marathon/
[support me]: https://www.crowdrise.com/o/en/campaign/tuftsboston2019/bhushansuwal
[Code]: https://github.com/bsuwal/Boston_Marathon_Analysis
[a Kaggle dataset]: https://www.kaggle.com/rojour/boston-results
