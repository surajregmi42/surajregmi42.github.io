---
title: 'How Many Samples Are Needed to Detect System Improvement?'
date: 2025-10-05
permalink: /posts/2025/10/num-samples-for-improvement/
tags:
  - Mathematics
  - Statistics
  - Data Science
---
## Background
In the real world, often, we would not be solving some problem for the first time. There would already be some 
existing solution in place and we would like to improve upon the existing solution. The improvement might come in 
different flavors; it can be expansion of the solution or simply improvement of the solution on some performance 
metric.

Let's take a concrete example. Suppose we have a credit card fraud detection system. Its job is to detect fraudulent
transactions. Let's suppose precision is our north star metric for the system. The existing system has precision of $p_1$.

The team decided to build the better system. The team eventually build the newer solution and deployed it to the production.
They could not do A/B testing before deployment for some reason and now, after deployment, they would like to know how 
many predicted positive samples need to be audited/reviewed to detect precision improvement.

## Mathematical Framework
The mathematical framework we would fit for this problem is to create statistical models for the systems and use the
concepts from statistics to find out the number of samples needed to detect the change.

### Statistical Model
For the old system, we assume it follows a Bernoulli statistical model with $p_1$ parameter. $p_1$ here is the
true precision of the old system. Similarly, we also assume the new system to follow the Bernoulli model but with
$p_2$ parameter.

So, the statistical models for those systems are:

$$\text{Old}:~(\{0,1\},\, \operatorname{Ber}(p_1)), \qquad
\text{New}:~(\{0,1\},\, \operatorname{Ber}(p_2)).$$

We also have $n_1$ data points for the old system and $n_2$ data points for the new system. We assume those $n_1$
and $n_2$ data points are identically and independently distributed in the respective statistical models.

The data points we get for the old system are represented as $X_1$, $X_2$, ..., $X_{n_1}$ and the data points we get
for the new system are represented as $X_1'$, $X_2'$, ..., $X_{n_2}'$.

### Hypothesis Formulation
The null hypothesis we would like to test here would be that the new system is not better than the old system. So, the
hypothesis formulation would be:

$H_0: p_1 \ge p_2$\
$H_a: p_1 < p_2$

Let $d$ be improvement in the precision of the new system.
Then, $d = p_2 - p_1$.

Now, the hypothesis can be rewritten as:\
$H_0: d <= 0$\
$H_a: d > 0$

Our estimates for $p_1$ and $p_2$ would be:

$$ \hat p_1 = \frac{1}{n_1}\sum_{i=0}^{n_1} X_{i} $$ and $$ \hat p_2 = \frac{1}{n_2}\sum_{i=0}^{n_2} X_{i}'$$

By using central limit theorem, both $\hat p_1$ and $\hat p_2$ are normally distributed on 
$ \mathcal{N}(p_1,\,\frac{p_1(1 - p_1)}{n_1}) $ and $ \mathcal{N}(p_2,\,\frac{p_2(1 - p_2)}{n_2}) $ respectively.
However, for the central limit theorem to hold, these conditions need to be satisfied:
$ n_1p_1 \ge 10,  n_1(1-p_1) \ge 10, n_2p_2 \ge 10, n_2(1-p_2) \ge 10$.

So, the difference of the normally distributed random variable is also normally distributed.

i.e., $ \hat d \sim \mathcal{N}(p_2 - p_1,\,\frac{p_2(1 - p_2)}{n_2} + \frac{p_1(1 - p_1)}{n_1}) $

We can use Wald's test to test this hypothesis and we can use $\hat d = \hat p_2 - \hat p_1 $ as our test statistic.

If we would want to control for Type (I) error of $\alpha$ for this test, the probability of rejecting when null is true
should be less than or equal to $\alpha$.

Hence, it should satisfy this condition:

$$\cfrac{\hat d}{\sqrt{\frac{p_2(1 - p_2)}{n_2} + \frac{p_1(1 - p_1)}{n_1}}} \le z_{1-\alpha}$$

### Power under a Planning Alternative
For the alternative plan, we need to set minimum detectable effect. Let's define minimum detectable effect as MDE. 
Also, let's assume power of $1-\beta$ for our test. That means when $d$ is at least MDE, we are able to reject
the test $1-\beta$ percentage of times. We just take boundary point for $d$ as that is sufficient.

So, $ P(\text{Rejection of test} \mid d = \mathrm{MDE}) = 1 - \beta $

If the value of $d$ is MDE, then $\cfrac{\hat d - MDE}{\sqrt{\frac{p_2(1 - p_2)}{n_2} + \frac{p_1(1 - p_1)}{n_1}}}$
would be  $\mathcal{N}(0, 1)$.

So, this would correspond:

$$ \cfrac{\hat d - MDE}{\sqrt{\frac{p_2(1 - p_2)}{n_2} + \frac{p_1(1 - p_1)}{n_1}}} = z_\beta $$

Subtracting (3) from (2),

$$ \cfrac{MDE}{\sqrt{\frac{p_2(1 - p_2)}{n_2} + \frac{p_1(1 - p_1)}{n_1}}} = z_{1-\alpha} - z_\beta $$

### Solve for Sample-size
Using (4), we can solve for sample size ($n_2$) and we would end up with:

$$ n_2 = \frac{ p_2(1 - p_2) }{ \left( \frac{\text{MDE}}{z_{1-\alpha} - z_{\beta}} \right)^{\!2} - \frac{p_1(1 - p_1)}{n_1} } $$

### Feasibility Condition
The denominator in (5) should be greater than 0. So, the following condition should hold true for being able to
guarantee $\alpha$ type (I) error and $\beta$ type (II) error.

$$ \left( \frac{\text{MDE}}{z_{1-\alpha} - z_{\beta}} \right)^{\!2} > \frac{p_1(1 - p_1)}{n_1} $$

We can rewrite this by solving for $n_1$.

$$ n_1 > p_1(1 - p_1) \left( \frac{z_{1-\alpha} - z_{\beta}}{\text{MDE}} \right)^{\!2} $$

## Application Example
Now, let's take an example and work through it. The question we would like to address is the following:
If the precision of old system was $5\%$ after reviewing $20000$ samples, how many samples would we want to collect if we
want to detect MDE of $0.5\%$ with $0.05$ level of significance and $80\%$ power.

In this case, $n_1$ would be $20000$, estimate for $p_1$ would be $0.05$, $\alpha$ would be $0.05$, MDE would be $0.005$, 
and $\beta$ would be $0.2$.

The value for $z_{0.95}$ and $z_{0.2}$ would be $1.64$ and $-0.84$ respectively.

At first let's see if the feasibility condition (7) would be met. Putting the values, the condition would be $n_1 > 11686$.
So, it satisfies that condition. Let's also check the CLT conditions. For our data, the expected number of positive samples would suffice. 
The expected number of positive samples is $10000$ and $1100$ for old and new system respectively. So, the CLT condition also holds.

Now, if we put in the values, we would get $n_2$ as $30759$. That's a lot of samples we need to get to make sure we have
those type (I) and type (II) errors.

## Final Thoughts
Personally, this feels awesome to me. This is what gets me excited about statistics, data science, and mathematics. Using the concepts
from these fields to answer real-life questions with rigor is what makes me awake at night. I stop my pen here with awe.
Thank you for reading my article!
