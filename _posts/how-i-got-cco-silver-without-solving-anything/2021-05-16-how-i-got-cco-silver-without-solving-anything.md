---
layout: post
title:  "How I got CCO silver without solving anything"
date:   2021-05-16 20:40:20 -0500
categories: programming
tag:
  - programming
---
I would like to preface this by saying that you should not voluntarily attempt what I did at CCO if you are serious about your results. My strategy was mainly damage control from day 1, and it was a miracle I even got silver. However, you may find my first CCO experience interesting or maybe even hilarious, which is why I am making this post. Anyways, please enjoy. 🙂

## Day 1

### P1. Swap Swap Sort

The first fault in my approach at CCO. I had some initial ideas early on, scoring the first 2 subtasks at around 16 minutes. Over the next hour, I developed a solution which runs in square root log time, believing it would pass with ease due to a fatal misreading of the constraints. The solution worked well for the first three subtasks, but for some reason I couldn't figure out kept receiving a `WA` verdict on the last batch. For around another hour, I read over my code countless times and ran multiple fast-slow scripts, but simply couldn't find out where the error lied. After a gruesome 2 hour and 30 minutes on problem 1, I decided to take a peek at the rest of the problemset before it was too late (although it was already way too much time wasted).

### P3. Through Another Maze Darkly

I had a quick glance over problem 2, but decided that the subtasks from problem 3 were a lot more approachable. Really, my goal here was to snatch whatever I can and rush back to problem 1 where I had the most potential points yet to be earned. A quick program that printed the path given by a random line graph revealed the pattern of $$ 1-x-1-y-1-...-1-N-1-N-... $$, for which the first idea that came to mind was binary search. This was the smoothest subtask of day 1 for me, taking only around 20 minutes of time in total.

### P2. Weird Numeral System

This problem was intimidating at first, with no clear idea on how to proceed. Again, I was simply controlling the damage of my poor problem 1 performance here, so I was only aiming for the subtask. The subtask provided the constraint that the absolute value of any allowed digit is less than the base, which intuitively meant that any change caused by a higher base couldn't be reverted by lower bases, no matter what values we assign them. This inspired a brute force recursion in descending order of power, ensuring that the number is in the range $$ (-b^K, b^K) $$ when we are done with the $$K$$-th power (of course, $$b$$ denotes the given base here). The solution ended up running surprisingly fast (0.1 seconds), but kept getting `WA` on case 7. After around 10 minutes of debugging, I decided it was all or nothing at this point and simply slapped `__int128` into my code, which surprisingly fixed the bug and gave me the first subtask. Overall, this was around 30 minutes spent on 8 marks, which I was relatively pleased with (considering that was more than half of the points I had earned so far).

### The struggle with P1 continues

It was only here that I decided it may be a good idea to reread the statement, and discovered that contrary to my belief that $$Q = 100\;000$$, the constraints actually had $$Q = 1\;000\;000$$! A quick 1 line fix to my `MQ` constant resolved the `WA` verdict, but replaced it with the ever so agonizing `TLE` instead. As there were only 30 minutes left at this point, I decided it would be futile to search for new solutions and settled with constant optimizing my square root log. I spammed around 20 different versions of the same code with different block sizes and with/without `pragma` optimizations, but none of them made it through the last batch. In the last 10 minutes I tried to cheese the time limit by handling numbers with small frequencies separately but that was to no avail either, and the timer hit 0 with only 11 points on problem 1, a problem I dedicated over 3 hours of contest time to.

### Reflections

Clearly, my strategy of tunnel visioning on problem 1 did not work out in my favour at all. Spending over 3 out of 4 hours of such an important contest for 11 points is something that would pain anyone to see, and I was quite sad over my mediocre day 1 performance. If there is one lesson to be learned out of my CCO experience this year, it would be to *read the constraints, and read them carefully*. Also, repeatedly submitting `WA` submissions or trying to fast-slow for over 30 minutes was just a waste of time, time that could have well been spent going for more partials on problem 3 or even a full solve on problem 2. Finally, it may have been wiser to try and optimize out the log factor from my code instead of spamming flimsy `pragma`s and cheeses, something that seems more than possible with 30 minutes in hindsight. Regardless, I could not redo what had already been done, and it was time to get well rested and prepare for day 2.

## Day 2

### Preparations

My mindset going into day 2 was to mainly control the damage that had been done on day 1. My lousy day 1 performance had already eliminated any chances of going for gold, so it was time to work on securing that silver. I did a mock CCO contest the day before, just to practice waking up, getting into contest mindset, and not choking or getting stuck on any particular problem for too long.

### P3. Loop Town

This goes first since it was the problem I eliminated first. After reading all the problem statements right off the bat, problem 3 already seemed concerningly difficult. The best I could come up with was some 2-SAT approach based on clockwise or counterclockwise travel, but the dependencies and conditions simply did not work out. After fiddling with it for a bit longer, I decided that this was probably the killer problem of CCO 2021, and dropped it completely (of course, the first subtask being worth 12 points helped with that realization as well). Back to the other two.

### P1. Travelling Merchant

I invested a fair amount of time into this problem. My first impressions were "oh hey, I finally found the template free easy Tarjan's problem" that previous CCOs all had (at least, some variation of a template problem). However, the problem quickly managed to shove my words back into my mouth as I pondered the details for around 30 minutes (hint, it wasn't Tarjan's at all). Keeping an open mind, I changed to an approach relying on Dijkstra's algorithm which managed to almost pass the first batch after some debugging. As it turned out, replacing the Dijkstra with a simple BFS allowed my solution to pass subtask 1 in the exact 1 second of allotted time, of course with some flimsy break statements attached as well. I couldn't find any easy optimizations with multisource BFS that would lead to a full solution, so I decided to move on to the next problem. I was quite shocked to learn after the contest that simply switching the BFS to a DFS and applying memoization was enough for full marks, but I guess that's just how it is sometimes :)

### P2. Bread First Search

For this problem, the observation of partitioning the nodes into some blocks of equal distances was immediately apparent, and a naive $$\mathcal{O}(N^3)$$ dynamic programming solution soon followed. Here, perhaps slightly foolishly due to the mindset of redeeming my day 1 performance, I decided to search for a $$\mathcal{O}(N)$$ greedy algorithm instead of attempting to optimize my DP to $$\mathcal{O}(N^2)$$. The reasoning behind this was that I had done quite a number of difficult problems which ended up converting a partial DP solution into full marks with a greedy approach, and I figured that this must be one of those as well. To my dismay, the problem was not actually a greedy problem, and I spent the rest of day 2 searching for something that wasn't even there in the first place. 

### Reflections

After day 2, I was quite sure my tragic performance (or so I thought) on both days would place me in the bronze medal range. The mistake of going for a solution that simply did not exist as compensation for a poor performance from before was unwise and panic-induced. However, the one thing I did manage to do well at CCO was ensuring that I didn't miss out on trivial partials, and giving all the problems at least a slight jab before moving on to the next. To my surprise, the median score on day 2 was only 2 points, which ended up placing me in the silver medalist range.

## Final Thoughts

This marks the conclusion of my first experience at CCO, and how I managed to earn a silver medal without even scoring full points on a single problem. It turns out that only partial farming (unintentionally) for both days can be sufficient to cross that silver cutoff, and I am glad I was able to leave the contest with a lot more experience and ideas than before. I can't say that I'm completely happy with how I did, but I am thankful that things didn't go as bad as I thought. I guess this also means I have a lot more to learn and prepare before the next wave of computing competitions. Anyways, that's it for my first blog.

Ciao 👋
