---
title: "Deleuze for Developers: will smooth space/open source suffice to save us?"
pubDate: 2013-03-14
blog: words
---


If you truly want to understand technology today, then you should at least be familiar with the philosophy of Gilles Deleuze. Unfortunately for technologists, Deleuze is rooted firmly in a philosophical tradition and a writing style that they probably find opaque. In this blog series, I plan on explaining Deleuze’s philosophy in terms that programmers can understand. This is the third in the series. You can find the first [here](/deleuze-for-developers-assemblages). Enjoy.

---

Deleuze and Guattari employ the notion of ‘smooth space’ quite a bit in *A Thousand Plateaus*. I saw this quote about it the other day:

> never believe that a smooth space will suffice to save us
> 

I’ve recently been giving a lot of thought to [‘cultural hegemony’](http://en.wikipedia.org/wiki/Cultural_hegemony) and its applicability to the web, specifically along the lines of Google. Basically, cultural hegemony is the idea that you don’t just have dominance through laws or economics, but through culture. So, for example, Google submitted SPDY to become HTTP 2.0: it’s going through the standards process, it’s ‘open,’ but it also gives Google a lot of cultural leverage.

So there’s this connection there between smooth space and open source, and the battle between the Nomads and the State Apparatus. We’ll cover that second bit in a while, but for now: what is ‘smooth space’?

## The spatial metaphor

The first thing to understand about these concepts is that Deleuze and Guattari is (I think of them as a singular entity…) a big fan of spatial metaphors. Essentially, we can project our problem domain onto a 2 or 3 (or more) dimensional space, and then use tools to analyze that space. I was first introduced to this technique by Manuel de Landa, who has a great lecture called “Deleuze and the use of the genetic algorithm in archetecture” ([video](http://www.youtube.com/watch?v=50-d_J0hKz0))([text](http://www.cddc.vt.edu/host/delanda/pages/algorithm.htm)). A quick rundown:

So, let’s talk about the [N queens problem](http://en.wikipedia.org/wiki/Eight_queens_puzzle). We have a chess board with one space, and we place one queen on it:

(warning, lots of ASCII (+ unicode) art to follow)

```
.-.
|♕|
.-.
```

This board is legal. It’s a solution to the ‘1 queen problem’: one queen, a 1x1 board. What about 2 queens? We have no solutions. (2 and 3 don’t. :/ All other natural numbers do.) First we place a queen:

```
.---.
|♕| |
-----
| | |
.---.
```

… but there’s no legal place for the second one. Bummer. We can determine that there’s no solution by using a simple brute force with backtracking: place the queen in the upper right, then try to place the second below it. That’s not legal, so we backtrack and place it on the right. That’s not legal, so we backtrack and put it in the bottom corner. Oh no! That didn’t work, so let’s move our first queen: now it’s in the bottom right, and we try to place the second in the upper right: fail! So we backtrack….

As you can see, this is a lot of steps for a small board. Once you get up to a ‘real’ chess board, there are 4,426,165,368 possible placements, but only 92 solutions. A needle in a haystack! And we’re using a lot of steps to determine if there’s even one possible placement. We need something better.

One answer is genetic algorithms. So we can take our board’s current state and assign it a score. When we place a new queen, if it makes our score better, we keep it, and if it doesn’t, we lose it. Now we’ve taken our 2x2 board and projected it onto a 2-axis linear space. Imagine a plot where queens are on the x axis and score is on the y axis:

```
                      .dAHAd.
                    .adAHHHAbn.
      .dAHAd.     .adAHHHHHHAbn.
    .adAHHHAbn.  .adAHHHHHHHHAbn.
   dHHHHHHHHHHHHHHHHHHHHHHHHHHHHHb
  dHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHb
```

So we want to keep going with our solution as long as it slopes up: we’ve found that our answers are at the maximum of each curve. Yay calculus! But here’s the thing: this curve only has two possible maxima: we will have more. And it might not be in 2-d space, it might be in 5-d space. And we can only ‘use math’ to find the maximum if we generate the *entire* curve, which seems computationally out of our reach. So how do we find the maximum without generating the whole curve? Genetic algorithms!

One such is ‘[simulated annealing](http://en.wikipedia.org/wiki/Simulated_annealing).’ Without getting too into it, let’s just say that there’s a ‘cooling factor’ that controls how tolerant we are of going back down a slope. So at first, we wildly go all over the search space, but then, as we progress, we tighten up our cooling factor, and we stop being so wild. Eventually, we’ll arrive at a solution that’s very likely to be the true global maxima/minima. Neat!

## Discrete vs. Continuous

> If we know that the enemy is open to attack, and also know that our men are in a condition to attack, but are unaware that the nature of the ground makes fighting impracticable, we have still gone only halfway towards victory.Sun Tzu, “the Art of War”
> 

Another interesting feature of this particular projection is that it transforms our problem from a discrete problem into a continuous one. One great tactic for when you’re losing: change the battlefield. Our genetic algorithm tool needs a continuous space to operate, but our problem is that our chess board is discrete. What do I mean by this?

“[discrete space](http://en.wikipedia.org/wiki/Discrete_space)” is one in which the points are separated from one another in some way. As an example, ‘integers’ form a discrete topology: there’s a ‘gap’ between 1, 2, 3, and 4. You can see this by drawing a [number line](http://en.wikipedia.org/wiki/Number_line):

```
<--(-2)--(-1)--(0)--(1)--(2)-->
```

The real numbers form a [continuous topology](http://en.wikipedia.org/wiki/Continuity_%28topology%29) rather than a discrete one, there is no space between them. A ‘[real line](http://en.wikipedia.org/wiki/Real_line)’:

```
<------------------------->
```

Our ‘scoring’ mechanism allows us to change the battlefield, it forms a function (isomorphism) to convert our discrete topology into a continuous one. We can now bring our continuous tooling to bear on a problem that was previously inaccessible.

## Striated vs Smooth Space

> Military tactics are like unto water; for water in its natural course runs away from high places and hastens downwards. So in war, the way is to avoid what is strong and to strike at what is weak. Like water, taking the line of least resistance. Water shapes its course according to the nature of the ground over which it flows; the soldier works out his victory in relation to the foe whom he is facing. Therefore, just as water retains no constant shape, so in warfare there are no constant conditions.Sun Tzu, “the Art of War”
> 

Okay. NOW we’re ready to talk about smooth and striated space. They have a number of names for this concept, and the one that’s most direct from where we currently are is ‘Riemann space / Euclidean space’. Smooth -> Riemann, Euclidean -> Striated. Another isomorphism. ;)

[Euclidean geometry](http://en.wikipedia.org/wiki/Euclidean_geometry) is pretty much the foundation of a metric ton of our math. It’s what’s now known as algebra and geometry: lots of integers, discrete spaces. This cube starts at (1,2,4) and has a side length of 5. Cartesian coordinates.

Along comes [Gauss](http://en.wikipedia.org/wiki/Carl_Friedrich_Gauss), who was incredibly intelligent. He had this student named [Riemann](http://en.wikipedia.org/wiki/Bernhard_Riemann), who ran with the ideas Gauss had and started [Riemannian Geometry](http://en.wikipedia.org/wiki/Riemannian_geometry). Riemann’s geometry is [non-Euclidean](http://en.wikipedia.org/wiki/Non-Euclidean_geometry): if Euclid described lines, Riemann described curves. Einstein would later base the theory of relativity on Riemannian geometry.

So, who cares? Well, think about it this way: You’re in a car, driving down a straight road. It goes for miles, with no turns or other streets. You’re operating in a smooth space: you can easily go from one place to another. Now, someone comes along and puts up stop signs every block. You must (for now) stop at these signs. Now you’re operating in a striated space: your movement is restricted. Now you’re moving from point to point on a line. It’s a very different experience, and you also didn’t have anything to do with the stop signs….

According to D&G, ‘the state machine’ (one instance of which is the State) attempts to take space that is smooth and striate it. This is often a means towards imperialism, as the striation happens through quantification. Take, for example, the sea: a smooth space, you can impose latitude and longitude on top of it, simultaneously quantifying and striating it. This allows you to navigate the waters, and conquer the sea.

This is also the position that many web startups take: ‘friendship’ was a smooth space before social networks came along. They quantified and striated that space, turning “I have many friends” into “I have 200 friends and you have 100 friends.” This quantification allows for commodification: now Facebook can sell ads. It’s also why startups don’t often have a ‘business model’ at first: they first need to conquer and striate their space before they find the commodity. Sometimes you just hit dirt and go bust, sometimes you find a more precious commodity, then refine and sell it.

‘nomads’ are entities which navigate and live in smooth spaces. Maybe my twitter bio makes a bit more sense now. ;)

## How do you create a smooth space?

You might remember Riemann from the ‘[Riemann sum](http://en.wikipedia.org/wiki/Riemann_sum)’ in Calc I. The brilliance of the Riemann sum is that it first striates, then re-smooths the space. You start off first with a curve, and then approximate the area under it by dividing it into a number of evenly-sized bits. This first bit was necessary in a world without calculus: we didn’t have the tooling or concepts to actually tackle the real area, so we mapped that problem to one we did know how to solve. As the number of bits goes up, and their width goes down, more and more of the space under the curve is captured by our algorithm. Finally, once we’re able to take that first step away from (striated) algebra and move into (smooth) calculus, we’re able to move about on the curve for reals. We’re back to smooth space again.

This interplay between smooth and striated spaces often happens, and there’s really no part of our world today that’s completely smooth or entirely striated. D&G posit that the left should be attempting to create as much smooth space as possible, and that capitalism is constantly attempting to striate space. That said, smooth space is necessary, but not sufficient: capitalism, in its hunger and lust for acquisition and totalitization, has managed to navigate some smooth spaces. I’ll just quote straight from ‘a thousand plateaus’, 492. It’s a little hard, but this is getting long enough as is. ;)

> Not only does the user as such tend to be an employee, but capitalism operates less on a quantity of labor than by a complex qualitative process bringing into play modes of transportation, urban models, the media, the entertainment industries, ways of perceiving and feeling – every semiotic system. It is though, at the outcome of the striation that capitalism was able to carry to an unequaled point of perfection, circulating capital necessarily recreated, reconstituted, a sort of smooths pace in which the destiny of human beings is recast. … at the level of world capitalism, a new smooth space is produced in which capital reaches its ‘absolute’ speed, based on machinic components rather than the human component of labor. The multinationals fabricate a kind of deterritorialized smooth space in which points of occupation as well as poles of exchange become quite independent of the classical paths to striation. .. the essential thing is instead the distinction between striated capital and smooth capital, and the way in which the former gives rise to the latter through complexes that cut across territories and States, and even the different types of States.
> 

The first part about ‘users’ is very much drawing a parallel to “If you’re not paying for the product, you are the product.” When they talk about the ‘speed’ of capital, think of HFT. Multinational corporations have managed to overcome the striation that a State has imposed. McDonalds is only partially affected by US laws. They operate ‘outside’ of states.

In this way, simply being smooth will not suffice to save us.

---

If you liked this blog post, you may enjoy ["Philosophy in a time of software](https://groups.google.com/forum/#!forum/philosophy-in-a-time-of-software), a moderated Google Group to discuss the intersection of philosophy and technology. This post started out as an email to that list.
