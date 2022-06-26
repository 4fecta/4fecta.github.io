---
layout: post
title:  "My road to gold at the 2022 CCO"
date:   2022-05-28 12:52:09 -0500
categories: programming
tag:
  - programming
---

Hello again! As promised, the long-awaited sequel to [this post](https://4fecta.ca/how-i-got-cco-silver-without-solving-anything/) is finally here. My experience at CCO this year was vastly different than the last (perhaps due in part to actually being able to solve some problems fully), and I am ecstatic that my practice and hard work over the past year has paid off big-time. With that being said, here's a rundown of my problem solving journey at CCO. Please enjoy :D

## Day 1

### P1. Alternating Heights

This was, in my opinion, the easiest CCO problem in quite a while. A minute or two after reading the problem statement, I had already made the observation of converting the inequalities to a directed graph where an assignment would be possible if and only if the graph is acyclic. Of course, directly precomputing each of the $$\mathcal{O}(N^2)$$ possible ranges would be too slow for full marks. However, I realized that it is possible to binary search on the right endpoint at which the graph becomes cyclic if the left endpoint is fixed, leading to an $$\mathcal{O}(N^2 \log N + Q)$$ solution overall. I had ideas to further improve the solution to $$\mathcal{O}(N^2 + Q)$$ by replacing the binary searches with a two-pointer scan, but was happy enough to see that the simple binary search solution passed under the time limit after submitting. In the end, problem 1 only took around 13 minutes of my contest time, something I was pretty pleased with. Onto the next one.

### P2. Rainy Markets

My initial reaction to this problem was actually to construct a flow network that models the people moving towards the bus shelters. Indeed, if we create $$N - 1$$ vertices $$X_1, X_2, ..., X_{N-1}$$ to represent the markets and $$N$$ vertices $$Y_1, Y_2, ..., Y_N$$ to represent the bus shelters, then we can construct the following edges:

- An edge from the source vertex $$S$$ to each of the market vertices $$X_i$$ with capacity $$P_i$$.
- An edge from each of the shelter vertices $$Y_i$$ to the sink vertex $$T$$ with capacity $$B_i$$.
- An edge from each market vertex $$X_i$$ to the shelter vertices $$Y_i$$ and $$Y_{i+1}$$ with infinite capacity.
- An edge from each of the market vertices $$X_i$$ directly to the sink vertex $$T$$ with a capacity of $$U_i$$ and a cost of $$1$$.

It can now be seen that the answer to the problem is simply the minimum cost maximum flow (MCMF) of the network. The issue, however, is that I have no idea how to implement MCMF! Whenever I needed to use it, it was always during situations where pre-written code was permitted, so I would just copy-paste the MCMF template from [KACTL](https://github.com/kth-competitive-programming/kactl/blob/main/content/graph/MinCostMaxFlow.h). A little annoyed by the fact that I had a theoretically correct solution I couldn't implement, I skipped to problem 3 for a while to calm myself down and see if any free partial marks were waiting for me. Sadly, there weren't, so I returned to problem 2 in hopes of making a little more progress.

Specifically, I realized that my flow model may not have been all for naught. All I had to do was consider efficient ways of sending flow through the network. After all, even the fastest MCMF implementations would stand no reasonable chance in a network with over a million vertices! Slowing down for a moment, I began to consider various greedy approaches to the problem. Firstly, there was the idea of forcing as many people to the left as possible, then to the right, and finally any left-overs to buy umbrellas in the middle, deeming the case impossible if people still remained after that. However, I was quickly able to construct a case where the algorithm dies completely, so I knew I needed to do better. Then, inspired by the key idea of flow algorithms which is to force as much flow down an edge as possible when creating an augmenting path but also leaving yourself the option to "undo" the flow if necessary, I thought of a process that sounded much better than the previous.

Essentially, we still begin by forcing as many people to the left as possible, then to the right, and then down the middle. However, if there are still people left at this point, the case could still be possible as there may be extra umbrellas on the left! Thus, we should send the remaining people down to the left bus shelter while forcing the extras out to the market on the left, essentially undoing what we previously thought was optimal. Now, we have the exact same situation; we need to assign umbrellas to the people at the market until none are left, and the rest must be forced to the bus shelter on the left, continuing all the way until we reach the first market again. This algorithm is comfortably implemented in $$\mathcal{O}(N^2)$$ by doing a DFS backwards whenever necessary, giving me 16 marks. For the remaining 9 marks, I soon realized that saving all the changes for one chain of DFS's going backwards would give me $$\mathcal{O}(N)$$, and a few quick changes to a couple of lines in the code sent me from 16 to 25. All in all, this problem took me a little over an hour, leaving me with over 2 hours to spend on one last problem.

### P3. Double Attendance

It was now time to pick up where I left off during my brief visit to problem 3 from before. I had formulated many possible DP states, but the main issue with all of them was that they could not detect when the same slide was visited multiple times, resulting in severe issues with overcounting the answer. After a little more investigation, however, I realized that storing the last time I visited the other classroom in the state would solve the issue completely, since I just needed to check whether a new slide has been played since then. This observation gave me a solution that solved the first 2 subtasks, leaving me with a comfortable 15/25 marks. 

The next subtask demanded a completely different approach, as my previous DP approach relied on the time values being small. Intuitively, I realized that there aren't that many important times on the slides. In fact, many visits to slides will be done just as the slide starts being presented! After all, there are only two choices to make after seeing a slide: either start walking to the other classroom immediately or stay here and wait for the next slide to be shown. Using an approach similar to Dijkstra's algorithm with a priority queue, I managed to snatch the 6 marks from subtask 3 after a moderate amount of debugging along the way. The constraint $$B_{i, j} - A_{i, j} \le 2K$$ actually ended up helping a lot here, since my algorithm was not capable of detecting whether a slide had already been visited or not. 

Now, only 4 marks away from a perfect score on day 1, I was a little stuck. Actually, scratch that — I was *really* stuck. The last subtask seemed like a huge jump from any of the previous ones. The number of slides was considerably larger so quadratic solutions were no longer permissible, and it wasn't guaranteed that $$B_{i, j} - A_{i, j} \le 2K$$ so a slide could be visited multiple times! I had absolutely no clue where to go from here. Any DP state I could think of was way too large to even declare in my program, and I didn't know how to extend my priority queue solution to make it faster while also adding detection for multiple visits to the same slide. I spent the entire last hour of the contest dumbfounded, sitting around in my chair waiting for the contest timer to hit 0.

### Reflections

Overall, I must say I was highly satisfied with my day 1 performance, especially when considering it in contrast to my performance from last year. The first two problems were smooth and sweet, with barely any debugging required. I also felt like I performed nearly to the best of my ability when it came to problem 3, wasting little time in securing the crucial partial marks. Perhaps I could've spent the last hour of contest time a bit more productively and just tried out random ideas that might have had any chance of working, hoping to stumble upon the genius solution later presented in the solution take up. In any case, as someone who came into day 1 aiming to solve around a problem and a half, I cannot complain about my final score of 71/75. This score also happened to put me in a three-way tie for third place on the unofficial scoreboard, which meant I actually had a shot at the gold medal this time. Exciting!

## Day 1.5

I mentioned last year that I did a mock CCO contest on the off-day in hopes of improving my performance by any possible amount I could last-minute. This year, I decided to try something completely different. I tried my best to take my mind completely off of competitive programming in order to get the most rest I could for day 2. I was confident enough in the practice that I had done throughout the year that I decided it wasn't worth tiring out my brain during the rare opportunity I was given for it to rest. In fact, I even decided to go to prom, which with (fortunate?) unfortunate timing was held the night before day 2 of CCO! After returning home, taking a shower, and shaking all the stress off, it was time to catch a good night's sleep to maximize my performance on day 2.

*P.S. To be fair, I did spend a good portion of my time at prom discussing a Codeforces problem with Allen. However, that was more for personal enjoyment than actual contest preparation!*

## Day 2

### P3. Good Game

The jump in difficulty from day 1 was immediately apparent. I had absolutely no idea where to even start on the first two problems, so the first problem I tackled was the third. It was clearly an ad hoc problem, a category of problems in which I have a good amount of confidence. The first observation I made was that any consecutive sequence of two or more of the same character could be directly removed from the string as necessary (after all, it is not hard to see that any integer no less than 2 can be expressed as the sum of a combination of twos and threes). With this in mind, we can compress the string based on segments sharing the same colour, where each segment is either directly removable (`O`) or not (`X`). 

Then, if the string has odd length, it is easy to see that it's winnable if the middle character is `O`. After all, removing the middle character would merge the two beside it into a middle character that is still `O`. However, what if it's not `O`? In this case, I decided to try to shift the centre of the string until it becomes an `O`. It is possible to shift the centre to the right by 1 if we remove an `O` from the left half, and vice versa as well. Obviously, we only care about the closest `O` on the right, and we also only care about the closest `O` on the left since that gives us the maximum possible shifting distance. It turns out this strategy provides a correct algorithm to determine whether the string is winnable, and it is easily implemented in $$\mathcal{O}(N)$$ by looping from the centre in both directions. However, what if the string has even length?

This case is a little more annoying, since there is no clear centre to work with. To handle this, I tried all possible splitting points so that I am left with two independent strings with odd lengths, and then tried solving the two halves separately. Unfortunately, this means my algorithm is now only $$\mathcal{O}(N^2)$$ for strings with even length. Even so, this was solid progress, and I submitted the solution for 16/25 marks. 

At this point, I had already spent a lot of time and brainpower on the problem and was not really keen on implementing an entirely different algorithm for full marks. So, I decided to instead try something a little more handwavy. Essentially, I realized that if there is an abundance of `O`s in the string, then it is highly likely for there to be two `O`s close to the centre of the second half of the string. So, I precomputed all splitting points where one half of the string would be immediately ready (with an `O` in the centre), and tried splitting the string at the first 200 of these candidate points. If I still hadn't found an answer at this point, I would just evaluate the case to be impossible. This reduced my time complexity back down to $$\mathcal{O}(N)$$ (admittedly with a rather huge constant factor), and to my surprise, actually passed all the cases! Not complaining at all.

### P2. Phone Plans

The next most approachable problem for me, at least in terms of subtasks. The second subtask was a pretty straightforward combination of precomputation and binary search, although the various edge cases made it slightly annoying to debug. For the third subtask, I simply tried all $$\mathcal{O}(AB)$$ possible pairs of plans. The first subtask was the trickiest out of the first three for me, mainly because I wasn't entirely sure how to take advantage of the fact that $$N$$ was small. After a lot of brainstorming though, I thought about doing something similar to what I did for the second subtask, except using bitsets to explicitly calculate the union of the connected pairs! This gave me a solution that worked in $$\mathcal{O}(N^3 \log N / 64)$$, which was actually fast enough in practice to pass the first subtask after some constant optimization. While that submission was judging, I also decided to optimize out the log factor by replacing my binary search with a single monotonic pointer giving me a solution in $$\mathcal{O}(N^3/ 64)$$ just in case, but it ended up being unnecessary as the first solution did just fine on its own.

### P1. Bi-ing Lottery Treekets

Stuck. Really stuck. A lot of things were going on in this problem, and I couldn't think of any ways to simplify it at all. I was even stumped by subtask 2, despite how deceptively easy it looked at first glance. In the end, I gladly walked away from this problem with only the first 3/25 marks by brute forcing literally all the possible scenarios. Probably wasn't getting much more than that regardless.

### Reflections

I was a little nervous after finishing day 2. I wasn't sure exactly how well I performed compared to the other contestants, especially considering how hard I bricked on problem 1. However, as the scores started rolling in on the unofficial scoreboard, I was astonished to see that I had actually settled in 4th place! It turns out that, yes, I did brick extremely hard on problem 1, but what made up for that was the surprising lack of solves on problem 3, which was actually in my opinion the easiest problem of day 2! The surge of emotions I felt at that moment was incredible: an intense mix of joy, excitement, nervousness, and relief. 

## Final Thoughts

Wow... I'm speechless. At the time I am writing this, it has been officially confirmed that I placed in the gold medalist range this year, and have been selected to represent Team Canada at IOI 2022 in Indonesia. One major takeaway from my performance this year was that I was capable of solving extremely challenging problems under contest conditions when I am completely focused and dedicated to the task. However, the contest also helped me identify some weaknesses and areas I can still improve on, for example the cleverly modelled DP solution to day 2 problem 1 outlined during the solution discussions. Regardless, I'm happy I was able to end my high school career on such a high note. In the near future, I hope to also record my experience at IOI in a similar post to this one. Until then!