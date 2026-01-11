---
title: 'How Did Feynman Calculate Cube Root So Faster?'
date: 2026-01-10
permalink: /posts/2026/01/feynman-cube-root-calculation/
tags:
  - Mathematics
  - Richart Feynman
  - Calculus
---
An episode from _Surely You’re Joking, Mr. Feynman!_ has stuck with me for years, and I wanted to analyze the math
behind it. In the story, Feynman describes an interaction with a Japanese man at a restaurant in Brazil.

The man brings an abacus and boasts to the waitresses that he can solve any arithmetic problem faster than they can. 
They then point him toward Feynman, and the situation turns into an impromptu math duel. It starts with simple addition
and gradually moves to harder problems. The man with the abacus outpaces Feynman early on, but as the calculations 
become more complex, Feynman closes the gap. In the end, Feynman wins on a cube-root problem, and the man leaves the 
restaurant visibly upset.

The problem is to find the cube root of $1729.03$. While the man with the abacus struggles to compute it on the spot, 
Feynman uses a quick calculus approximation to get the answer correct to three decimal places. 
Here is the idea behind his method.

Feynman knew a useful benchmark: $1$ cubic foot equals $1728$ cubic inches, and $1728 = 12^3$. So $\sqrt[3]{1729.03}$ 
must be very close to $12$. The only remaining task is to estimate the small _offset_ from $12$.

Let’s analyze that mathematically.

If $l$ denotes the side length of a cube and $V$ denotes its volume, then

$$
V = l^3.
$$

Differentiate both sides:

$$
dV = 3l^2\,dl.
$$

Divide by $V=l^3$:

$$
\frac{dV}{V} = \frac{3l^2\,dl}{l^3} = \frac{3\,dl}{l},
$$

so

$$
\frac{dl}{l} = \frac{1}{3}\frac{dV}{V}.
$$

In words: the fractional (percentage) change in side length is about one third of the fractional change in volume.

Here the volume changes from $1728$ to $1729.03$, so $\Delta V = 1.03$. Because this change is tiny compared with $1728$, we can treat differentials as good approximations:

$$
\frac{\Delta l}{l} \approx \frac{1}{3}\frac{\Delta V}{V}.
$$

Plug in $l = 12$ and $V = 1728$:

$$
\Delta l \approx 12 \cdot \frac{1}{3}\cdot \frac{1.03}{1728}
= \frac{1.03}{432}.
$$

Now do the mental arithmetic: $1/432 \approx 0.002...$, so

$$
\frac{1.03}{432} \approx 1.03\times 0.002 \approx 0.002.
$$

Therefore,

$$
\sqrt[3]{1729.03} \approx 12 + 0.002 = 12.002 \quad \text{(to three decimals)}.
$$

So, the brilliance is not a mysterious trick: it’s a clean combination of
(1) a memorized fact $12^3=1728$,
(2) a first-order calculus approximation, and
(3) quick mental estimation. A neat reminder that a little calculus goes a long way in everyday problem-solving.