#  Probability Theory 101


Before we show how to use randomness in algorithms, let us do a quick review of some basic notions in probability theory.
This is not meant to replace a course on probability theory, and if you have not seen this material before, I highly recommend you look at additional resources to get up to speed.^[Harvard's [STAT 110](http://projects.iq.harvard.edu/stat110/home) class (whose lectures are available on [youtube](http://projects.iq.harvard.edu/stat110/youtube) ) is a highly recommended introduction to probability. See also these [lecture notes](http://www.boazbarak.org/cs121/LLM_probability.pdf) from MIT's "Mathematics for Computer Science" course.]
Fortunately, we will not need many of the advanced notions of probability theory, but, as we will see, even the so called "simple" setting of tossing $n$ coins can lead to very subtle and interesting issues.




## Random coins


The nature of randomness and probability is a topic of great philosophical, scientific and mathematical depth.
Is there actual randomness in the world, or does it proceed in a deterministic clockwork fashion from some  initial conditions set at the beginning of time?
Does probability refer to our uncertainty of beliefs, or to the frequency of occurrences in repeated experiments?
How can we define probability over infinite sets?

These are all important questions that have been studied and debated by  scientists, mathematicians, statisticians and philosophers.
Fortunately, we will not  need to deal directly with these questions here.
We will be mostly interested in the setting of tossing $n$ random, unbiased and independent coins.
Below we define the basic probabilistic objects of _events_ and  _random variables_ when restricted to this setting.
These can be defined for much more general probabilistic experiments or _sample spaces_, and later on we will briefly discuss how this can be done, though the $n$-coin case is sufficient for almost everything we'll need in this course.

If instead of "heads" and "tails" we encode the sides of each coin by "zero" and "one", we can encode the result of tossing $n$ coins as a string in $\{0,1\}^n$.
Each particular outcome $x\in \{0,1\}^n$ is obtained with probability $2^{-n}$.
For example, if we toss three coins, then we obtain each of the 8 outcomes $000,001,010,011,100,101,110,111$ with probability $2^{-3}=1/8$.
We can also describe this experiment as choosing $x$ uniformly at random from $\{0,1\}^n$, and hence we'll use the shorthand $x\sim \{0,1\}^n$ for it.

An _event_ is simply a subset $A$ of $\{0,1\}^n$.
The _probability of $A$_, denoted by $\Pr_{x\sim \{0,1\}^n}[A]$ (or $\Pr[A]$ for short, when the sample space is understood from the context), is  the probability that a random $x$ chosen uniformly at random will be  contained in $A$.
Note that this is the same as $|A|/2^n$.
For example, the probability that $x$ has an even number of ones is $\Pr[A]$ where $A=\{ x : \sum_{i} x_i = 0 (\mod 2) \}$.
Let us calculate this probability:

> # {.lemma #evenprob}
$$\Pr_{x\sim \{0,1\}^n}[ \text{$\sum x_i$ is even }] = 1/2$$

> # {.proof data-ref="evenprob"}
Let $A = \{ x \in \{0,1\}^n :  \sum_i x_i = 0 (\mod 2) \}$.
Since every $x$ is obtained with probability $2^{-n}$, to show this we need to show that $|A|=2^n/2=2^{n-1}$.
For every $x_0,\ldots,x_{n-2}$, if $\sum_{i=0}^{n-2} x_i$ is even then $(x_0,\ldots,x_{n-1},0)\in A$ and $(x_0,\ldots,x_{n-1},1) \not\in A$.
Similarly, if $\sum_{i=0}^{n-2} x_i$ is odd then $(x_0,\ldots,x_{n-1},1) \in A$ and  $(x_0,\ldots,x_{n-1},0)\not\in A$.
Hence, for every one of the $2^{n-1}$ prefixes $(x_0,\ldots,x_{n-2})$, there is exactly a single continuation of $(x_0,\ldots,x_{n-2})$ that places it in $A$.

We can also use the  _intersection_ ($\cap$) and _union_ ($\cup$) operators to talk about the probability of both event $A$ _and_ event $B$ happening, or the probability of event $A$ _or_ event $B$ happening.
For example, the probability $p$ that $x$ is has an _even_ number of ones _and_ $x_0=1$ is the same as
$\Pr[A\cap B]$ where $A=\{ x\in \{0,1\}^n : \sum_i x_i =0 (\mod 2) \}$ and $B=\{ x\in \{0,1\}^n : x_0 = 1 \}$.
This probability is equal to $1/4$: can you see why?
Because intersection corresponds to considering  the logical AND of the conditions that two events happen, while union corresponds to considering the logical OR, we will sometimes use the $\wedge$ and $\vee$ operators instead of $\cap$ and $\cup$, and so write this probability $p$ above also as
$$
\Pr_{x\sim \{0,1\}^n} \left[ \sum_i x_i =0 (\mod 2) \; \wedge \; x_0 = 1 \right] \;.
$$

If $A \subseteq \{0,1\}^n$ is an event, then $\overline{A} = \{0,1\}^n \setminus A$ corresponds to the event that $A$ does _not_ happen.
Note that $\Pr[ \overline{A}] = 1 -\Pr[A]$: can you see why?

### Random variables

A _random variable_ is a function $X:\{0,1\}^n \rightarrow \R$ that maps every outcome $x\in \{0,1\}^n$ to a real number $X(x)$.
For example, the sum of the $x_i$'s is a random variable.
The _expectation_ of a random variable $X$, denoted by $\E[X]$, is the average value that it will receive.
That is,
$$
\E[X] = \sum_{x\in \{0,1\}^b} 2^{-n}X(x) \;.
$$

If $X$ and $Y$ are random variables, then we can define $X+Y$ as simply the random variable maps $x$ to $X(x)+Y(x)$.
One of the basic and useful properties of the expectation is that is is _linear_:

> # {.lemma title="Linearity of expectation" #linearityexp}
$$ \E[ X+Y ] = \E[X] + \E[Y] $$

> # {.proof data-ref="linearityexp"}
$$
\begin{split}
\E [X+Y] = \sum_{x\in \{0,1\}^n}2^{-n}\left(X(x)+Y(x)\right) =  \\
\sum_{x\in \{0,1\}^b} 2^{-n}X(x) + \sum_{x\in \{0,1\}^b} 2^{-n}Y(x) = \\
\E[X] + \E[Y]
\end{split}
$$

Similarly, $\E[kX] = k\E[X]$ for every $k \in \R$.
For example, using the linearity of expectation, it is very easy to show that the expectation of the sum of the $x_i$'s for $x \sim \{0,1\}^n$ is equal to $n/2$.
Indeed, if we write $X= \sum_{i=0}^{n-1} x_i$ then $X= X_0 + \cdots + X_{n-1}$ where $X_i$ is the random variable $x_i$. Since for every $i$, $\Pr[X_i=0] = 1/2$ and $\Pr[X_i=1]=1/2$, we get that $\E[X_i] = (1/2)\cdot 0 + (1/2)\cdot 1 = 1/2$ and hence $\E[X] = \sum_{i=0}^{n-1}\E[X_i] = n\cdot(1/2) = n/2$.
(If you have not seen discrete probability before, please go over this argument again until you are sure you follow it, it is a prototypical simple example of the type of reasoning we will employ again and again in this course.)

If $A$ is an event, then $1_A$ is the random variable such that $1_A(x)$ equals $1$ if $x\in A$ and $1_A(x)=0$ otherwise.
Note that $\Pr[A] = \E[1_A]$ (can you see why?).
Using this and the linearity of expectation, we can show one of the most useful bounds in probability theory:

> # {.lemma title="Union bound" #unionbound}
For every two events $A,B$, $\Pr[ A \cup B] \leq \Pr[A]+\Pr[B]$

> # {.proof data-ref="unionbound"}
For every $x$, the variable $1_{A\cup B}(x) \leq 1_A(x)+1_B(x)$.
Hence, $\Pr[A\cup B] = \E[ 1_{A \cup B} ] \leq \E[1_A+1_B] = \E[1_A]+\E[1_B] = \Pr[A]+\Pr[B]$.

The way we often use this in theoretical computer science is to argue that, for example, if there is a list of 1000 bad events that can happen, and each one of them happens with probability at most $1/10000$, then with probability at least $1-1000/10000 \geq 0.9$, no bad event happens.

### More general sample spaces.

While in this lecture we assume that the underlying probabilistic experiment   corresponds to tossing $n$ independent coins, everything we say easily generalizes to sampling $x$ from a more general finite set $S$ (and not-so-easily generalizes to infinite sets $S$ as well).
A _probability distribution_  over a finite set $S$ is simply a function $\mu : S \rightarrow [0,1]$ such that
$\sum_{x\in S}\mu(s)=1$.
We think of this as the experiment where we obtain every $x\in S$ with probability $\mu(s)$, and sometimes denote this as $x\sim \mu$.
An _event_ $A$ is a subset of $S$, and the probability of $A$, which we denote by $\Pr_\mu[A]$ is $\sum_{x\in A} \mu(x)$.
A _random variable_ is a function $X:S \rightarrow \R$, where the probability that $X=y$ is equal to $\sum_{x\in S \text{s.t.} X(x)=y} \mu(x)$.

^[TODO: add exercise on simulating die tosses and choosing a random number in $[m]$ by coin tosses]

## Correlations and independence

One of the most delicate but important concepts in probability is the notion of _independence_ (and the opposing notion of _correlations_).
Subtle correlations are often behind surprises and errors in probability and statistical analysis, and several mistaken predictions have been blamed on miscalculating the correlations between, say, housing prices in Florida and Arizona, or voter preferences in Ohio and Michigan. See also Joe Blitzstein's aptly named talk ["Conditioning is the Soul of Statistics"](https://youtu.be/dzFf3r1yph8).^[Another thorny issue is of course the difference between _correlation_ and _causation_. Luckily, this is another point we don't need to worry about in our clean setting of tossing $n$ coins.]  

Two events $A$ and $B$ are _independent_ if the fact that $A$ happened does not make $B$ more or less likely to happen.
For example, if we think of the experiment of tossing $3$ random coins $x\in \{0,1\}^3$, and let $A$ be the event that $x_0=1$ and $B$ the event that $x_0 + x_1 + x_2 \geq 2$, then if $A$ happens it is more likely that $B$ happens, and hence these events are _not_ independent.
On the other hand, if we let $C$ be the event that $x_1=1$, then because the second coin toss is not affected by the result of the first one, the events $A$ and $C$ are independent.

Mathematically, we say that events $A$ and $B$ are _independent_ if $\Pr[A \cap B]=\Pr[A]\Pr[B]$.
If $\Pr[A \cap B] > \Pr[A]\Pr[B]$ then we say that $A$ and $B$ are _positively correlated_, while if $\Pr[ A \cap B] < \Pr[A]\Pr[B]$ then we say that $A$ and $B$ are _negatively correlated_.

If we consider the above examples on the experiment of choosing $x\in \{0,1\}^3$ then we can see that

$$
\begin{aligned}
\Pr[x_0=1] &= 1/2 \\
\Pr[x_0+x_1+x_2 \geq 2] = \Pr[\{ 011,101,110,111 \}] &= 4/8 = 1/2
\end{aligned}
$$

but

$$
\Pr[x_0 =1 \; \wedge \; x_0+x_1+x_2 \geq 2 ] = \Pr[ \{101,110,111 \} ] = 3/8 > (1/2)(1/2)
$$

and hence, as we already observed, the events $\{ x_0 = 1 \}$ and $\{ x_0+x_1+x_2 \geq 2 \}$ are  not independent and in fact positively correlated.
On the other hand $\Pr[ x_0 = 1 \wedge x_1 = 1 ] = \Pr[ \{110,111 \}] = 2/8 = (1/2)(1/2)$ and hence the events $\{x_0 = 1 \}$ and $\{ x_1 = 1 \}$ are indeed independent.


__Conditional probability:__ If $A$ and $B$ are events, and $A$ happens with nonzero probability then we define the probability that $B$ happens _conditioned on $A$_ to be $\Pr[B|A] = \Pr[A \cap B]/\Pr[A]$.
This corresponds to calculating the probability that $B$ happened if we already know that $A$ happened.
Note that $A$ and $B$ are independent if and only if $\Pr[B|A]=\Pr[B]$.

__More than two events:__ We can generalize this definition to more than two events.
We say that events $A_1,\ldots,A_k$ are _mutually independent_ if knowing that any set of them occurred or didn't occur does not change the probability that an event outside the set occurrs.
Formally the condition is that for every subset $I \subseteq [k]$,
$$
\Pr[ \wedge_{i\in I} A_i] =\prod_{i\in I} \Pr[A_i]
$$

For example, if $x\sim \{0,1\}^3$, then the events $\{ x_0=1 \}$, $\{ x_1 = 1\}$ and $\{x_2 = 1 \}$ are mutually independent.
On the other hand, the events $\{x_0 = 1 \}$, $\{x_1 = 1\}$ and $\{ x_0 + x_1 = 0 (\mod 2) \}$ are _not_ mutually independent even though every pair of these events is independent (can you see why?).

### Independent random variables

We say that two random variables $X$ and $Y$ are independent if for every $u,v \in \R$, the events $\{ X=u \}$ and $\{ Y=t \}$ are independent.
That is, $\Pr[ X=u \wedge Y=t]=\Pr[X=u]\Pr[Y=t]$.
For example, if two random variables depend on the result of tossing different coins then they are independent:

> # {.lemma  #indcoins}
Suppose that $S=\{ s_1,\ldots, s_k \}$ and $T=\{ t_1 ,\ldots, t_m \}$ are disjoint subsets of $\{0,\ldots,n-1\}$ and let
$X,Y:\{0,1\}^n \rightarrow \R$ be random variables such that $X=F(x_{s_1},\ldots,x_{s_k})$ and $Y=G(x_{t_1},\ldots,x_{t_m})$ for some functions $F: \{0,1\}^k \rightarrow \R$ and $G: \{0,1\}^m \rightarrow \R$.
Then $X$ and $Y$ are independent.

> # {.proof data-ref="indcoins"}
Let $a,b\in \R$, and let $A = \{ x \in \{0,1\}^k : F(x)=a \}$ and $B=\{ x\in \{0,1\}^m : F(x)=b \}$.
Since $S$ and $T$ are disjoint, we can reorder the indices so that $S = \{0,\ldots,k-1\}$ and $T=\{k,\ldots,k+m-1\}$ without affecting any of the probabilities.
Hence we can write $\Pr[X=a \wedge X=b] = |C|/2^n$ where $C= \{ x_0,\ldots,x_{n-1} : (x_0,\ldots,x_{k-1}) \in A \wedge (x_k,\ldots,x_{k+m-1}) \in B \}$.
Another way to write this using string concatenation is that $C = \{ xyz : x\in A, y\in B, z\in \{0,1\}^{n-k-m} \}$, and hence $|C|=|A||B|2^{n-k-m}$, which means that
$$
\tfrac{|C|}{2^n} = \tfrac{|A|}{2^k}\tfrac{|B|}{2^m}\tfrac{2^{n-k-m}}{2^{n-k-m}}=\Pr[X=a]\Pr[Y=b]
$$



Note that if $X$ and $Y$ are independent then

$$
\begin{split}
\E[ XY ] = \sum_{a,b} \Pr[X=a \wedge Y=b]ab = \sum_a \Pr[X=a]\Pr[Y=b]ab = \\
\left(\sum_a \Pr[X=a]a\right)\left(\sum_b \Pr[Y=b]b\right) = \\
\E[X] \E[Y]
\end{split}
$$
(This is not an "if and only if", see [noindnocorex](){.ref}.)

If $X$ and $Y$ are independent random variables then so are $F(X)$ and $G(Y)$ for every functions $F,G:\R \rightarrow R$.
This is intuitively true since learning $F(X)$ can only provide us with less information than learning $X$.
Hence, if learning $X$ does not teach us anything about $Y$ (and so also about $F(Y)$) then neither will learning $F(X)$.
Indeed, to prove this  we can write

$$
\begin{split}
\Pr[ F(X)=a \wedge G(Y)=b ] = \sum_{x \text{ s.t.} F(x)=a, y \text{ s.t. } G(y)=b} \Pr[ X=x \wedge Y=y ] = \\
\sum_{x \text{ s.t.} F(x)=a, y \text{ s.t. } G(y)=b} \Pr[ X=x ] \Pr[  Y=y ]  = \\
\left( \sum_{x \text{ s.t.} F(x)=a } \Pr[X=x ] \right) \cdot \left( \sum_{y \text{ s.t.} G(y)=b } \Pr[Y=y ] \right) = \\
\Pr[ F(X)=a] \Pr[G(Y)=b]
\end{split}
$$

### Collections of independent random variables.

We can extend the notions of independence to more than two random variables.
We say that  the random variables $X_0,\ldots,X_{n-1}$ are _mutually independent_ if for every $a_0,\ldots,a_{n-1}$ then
$$
\Pr[X_0=a_0 \wedge \cdots \wedge X_{n-1}=a_{n-1}]=\Pr[X_0=a_0]\cdots \Pr[X_{n-1}=a_{n-1}]
$$
and similarly we have that

> # {.lemma title="Expectation of product of independent random variables" #expprod}
If $X_0,\ldots,X_{n-1}$ are mutually independent then
$$
\E[ \prod_{i=0}^{n-1} X_i ] = \prod_{i=0}^{n-1} \E[X_i]
$$

> # {.lemma title="Functions preserve independence" #indeplem}
If $X_0,\ldots,X_{n-1}$ are mutually independent, and $Y_0,\ldots,Y_{n-1}$ are defined as $Y_i = F_i(X_i)$ for some functions $F_0,\ldots,F_{n-1}:\R \rightarrow \R$, then $Y_0,\ldots,Y_{n-1}$ are mutually independent as well.

We leave proving [expprod](){.ref} and [indeplem](){.ref} as [expprodex](){.ref} [indeplemex](){.ref}.
It is  good idea for you stop now and do these exercises to make sure you are comfortable with the notion of independence, as we will use it heavily later on in this course.



## Concentration

The name "expectation" is somewhat misleading.
For example, suppose that I place a bet on the outcome of 10 coin tosses, where if they all come out to be $1$'s then  I pay you 100000 dollars and otherwise you pay me 10 dollars.
If we let $X:\{0,1\}^{10} \rightarrow \R$ be the random variable denoting your gain, then we see that

$$
\E[X] = 2^{-10}\cdot 100000 - (1-2^{-10})10 \sim 90
$$

but we don't really "expect" the result of this experiment to be for you to gain 90 dollars.
Rather, 99.9\% of the time you will pay me 10 dollars, and you will hit the jackpot 0.01\% pf the times.

However, if we repeat this experiment again and again (with fresh and hence _independent_ coins), then in the long run we do expect your average earning to be 90 dollars, which is the reason why casinos can make money in a predictable way even though every individual bet is random.
For example, if we toss $n$ coins, then as $n$ grows the number of coins that come up ones will be more and more _concentrated_ around $n/2$ according to  the famous "bell curve" (see [bellfig](){.ref}).

![The probabilities we obtain a particular sum when we toss $n=10,20,100,1000$ coins converge quickly to the Gaussian/normal distribution.](../figure/binomial.png){#bellfig .class width=300px height=300px}

Much of probability theory is concerned with so called _concentration_ or _tail_ bounds, which are upper bounds on the probability that a random variable $X$ deviates too much from its expectation.
The first and simplest one of them is Markov's inequality:

> # {.theorem title="Markov's inequality" #markovthm}
If $X$ is a non-negative random variable then $\Pr[ X \geq k \E[X] ] \leq 1/k$.

> # {.proof data-ref="markovthm"}
Let $\mu = \E[X]$ and define $Y=1_{X \geq k \mu}$. That is, $Y(x)=1$ if $X(x) \geq k \mu$ and $Y(x)=0$ otherwise.
Note that by definition, for every $x$, $Y(x) \leq X/(k\mu)$.
We need to show $\E[Y] \leq 1/k$.
But this follows since  $\E[Y] \leq \E[X/k(\mu)] = \E[X]/(k\mu) = \mu/(k\mu)=1/k$.

Markov's inequality says that a (non negative) random variable $X$ can't go too crazy and be, say, a million times its expectation, with significant probability.
But ideally we would like to say that with high probability $X$ should be very close to its expectation, e.g., in the range $[0.99 \mu, 1.01 \mu]$ where $\mu = \E[X]$.
This is not generally true, but does turn out to hold when $X$ is obtained by combining (e.g., adding)  many independent random variables.
This phenomena, variants of which are known as  "law of large numbers", "central limit theorem", "invariance principles" and "Chernoff bounds", is one of the most fundamental in probability and statistics, and one that we heavily use in computer science as well.

## Chebyshev's Inequality

A standard way to  measure the deviation of a random variable from its expectation is using its _standard deviation_.
For a random variable $X$, we define the _variance_ of $X$ as  $Var[X] = \E[X-\mu]^2$ where $\mu = \E[X]$, i.e., the variance is the average square distance of $X$ from its expectation.
The _standard deviation_ of $X$ is defined as $\sigma[X] = \sqrt{Var[X]}$.

Using Markov's inequality we can control the probability that a random variable is too many standard deviations away from its expectation.

> # {.theorem title="Chebyshev's inequality" #chebychevthm}
Suppose that $\mu=\E[X]=\mu$ and $\sigma^2 = Var[X]$.
Then for every $k>0$, $\Pr[ |X-\mu | \geq k \sigma ] \leq 1/k^2$.

> # {.proof data-ref="chebychevthm"}
The proof follows from Markov's inequality.
We define the random variable $Y = (X-\mu)^2$.
Then $\E[Y] = Var[X] = \sigma^2$, and hence by Markov the probability that $Y > k^2\sigma^2$ is at most $1/k^2$.
But clearly $(X-\mu)^2 \geq k^2\sigma^2$ if and only if $|X-\mu| \geq k\sigma$.

One example of how to use Chebyshev's inequality is the setting when $X = X_1 + \cdots + X_n$ where $X_i$'s are independent and identically distributed (i.i.d for short) variables with values in $[0,1]$ where each has  expectation $1/2$.
Since $\E[X] = \sum_i \E[X_i] = n/2$, we would like to say that $X$ is very likely to be in, say, the interval  $[0.499n,0.501n]$.
Using Markov's inequality directly will not help us, since it will only tell us that $X$ is very likely to be at most $100n$ (which we already knew, since it always lies between $0$ and $n$).
However,  since $X_1,\ldots,X_n$ are independent,
$$
Var[X_1+\cdots +X_n] = Var[X_1]+\cdots + Var[X_n]  \label{varianceeq}\;.
$$
(We leave showing this to the reader as  [varianceex](){.ref}.)

For every random variable $X_i$ in $[0,1]$, $Var[X_i] \leq 1$ (if the variable is always in $[0,1]$, it can't be more than $1$ away from its expectation), and hence [varianceeq](){.eqref} implies that $Var[X]\leq n$ and hence $\sigma[X] \leq \sqrt{n}$.
For large $n$, $\sqrt{n} \ll 0.01n$, and in particular if $\sqrt{n} \leq 0.01n/k$,  we can use Chebyshev's inequality  to bound the probability that $X$ is not in $[0.499n,0.501n]$ by $1/k^2$.


## The Chernoff bound

Chebyshev's inequality already shows a connection between independence and concentration, but in many cases we can hope for a quantitatively much stronger result.
If, as in the example above, $X= X_1+\ldots+X_n$ where the $X_i$'s are  bounded i.i.d random variables of mean $1/2$, then as $n$ grows, the distribution of $X$ would be roughly the _normal_ or _Gaussian_ distribution, that is distributed according to  the  _bell curve_.
This distribution has the property of being _very_ concentrated in the sense that the probability of deviating $k$ standard deviation is not merely $1/k^2$ as is guaranteed by Chebyshev, but rather it is roughly $e^{-k^2}$.
That is we have an _exponential decay_ of the probability of deviation.
This is stated by the following theorem, that is known under many names in different communities, though it is mostly called the "Chernoff bound" in the computer science literature:


> # {.theorem title="Chernoff bound" #chernoffthm}
If $X_1,\ldots,X_n$ are i.i.d random variables such that $X_i \in [0,1]$ and $\E[X_i]=p$ for every $i$,
then for every $\epsilon >0$
$$
\Pr[ \left| \sum_{i=0}^{n-1} X_i - pn \right| > \epsilon n ] \leq 2\exp(-\epsilon^2 n/2)
$$

We omit the proof, which appears in many texts, and uses Markov's inequality on i.i.d random variables $Y_0,\ldots,Y_n$ that are of the form $Y_i = e^{\lambda X_i}$ for some carefully chosen parameter $\lambda$.
See [chernoffstirlingex](){.ref}  for a proof of the    simple (but highly useful and representative) case where each $X_i$ is $\{0,1\}$ valued and $p=1/2$.
(See also [poorchernoff](){.ref} for a generalization.)

^[TODO: maybe add an example application of Chernoff. Perhaps a probabilistic method proof using Chernoff+Union bound.]

## Lecture summary

* A basic probabilistic experiment corresponds to tossing $n$ coins or choosing $x$ uniformly at random from $\{0,1\}^n$.
* _Random variables_ assign a real number to every result of a coin toss. The _expectation_ of a random variable $X$, is its average value, and there are several _concentration_ results showing that under certain conditions  random variables deviate significantly from their expectation only with small probability.

## Exercises

> # {.exercise }
Give an example of random variables $X,Y: \{0,1\}^3 \rightarrow \R$ such that
$\E[XY] \neq \E[X]\E[Y]$.


> # {.exercise #noindnocorex }
Give an example of random variables $X,Y: \{0,1\}^3 \rightarrow \R$ such that $X$ and $Y$ are _not_ independent but $\E[XY] =\E[X]\E[Y]$.



> # {.exercise title="Product of expectations" #expprodex}
Prove [expprod](){.ref}

> # {.exercise title="Transformations preserve independence" #indeplemex}
Prove [indeplem](){.ref}


> # {.exercise title="Variance of independent random variances" #varianceex}
Prove that if $X_0,\ldots,X_{n-1}$ are independent random variables then $Var[X_0+\cdots+X_{n-1}]=\sum_{i=0}^{n-1} Var[X_i]$.


> # {.exercise title="Entropy (challenge)" #entropyex}
Recall the definition of a distribution $\mu$ over some finite set $S$.
Shannon defined the _entropy_ of a distribution $\mu$, denoted by $H(\mu)$, to be $\sum_{x\in S} \mu(x)\log(1/\mu(x))$.
The idea is that if $\mu$ is a distribution of entropy $k$, then encoding members of $\mu$ will require $k$ bits, in an amortized sense.
In this exercise we justify this definition. Let  $\mu$ be such that $H(\mu)=k$. \
1. Prove that for every one to one function $F:S \rightarrow \{0,1\}^*$, $\E_{x \sim \mu} |F(x)| \geq k$. \
2. Prove that  for every $\epsilon$, there is some $n$ and a one-to-one function $F:S^n \rightarrow \{0,1\}^*$, such that $\E_{x\sim \mu^n} |F(x)| \leq n(k+\epsilon)$,
where $x \sim \mu$ denotes the experiments of choosing $x_0,\ldots,x_{n-1}$ each independently from $S$ using the distribution $\mu$.

> # {.exercise title="Entropy approximation to binomial" #entropybinomex}
Let $H(p) = p \log(1/p)+(1-p)\log(1/(1-p))$.^[While you don't need this to solve this exercise,this is the function that maps $p$ to the entropy (as defined in [entropyex](){.ref}) of the $p$-biased coin distribution over $\{0,1\}$, which is the function $\mu:\{0,1\}\rightarrow [0,1]$ s.y. $\mu(0)=1-p$ and $\mu(1)=p$.]
Prove that for every $p \in (0,1)$ and $\epsilon>0$, if $n$ is large enough then^[__Hint:__ Use Stirling's formula for approximating the factorial function.]
$$
2^{(H(p)-\epsilon)n }\binom{n}{pn} \leq 2^{(H(p)+\epsilon)n}
$$
where $\binom{n}{k}$ is the binomial coefficient $\tfrac{n!}{k!(n-k)!}$ which is equal to the number of $k$-size subsets of $\{0,\ldots,n-1\}$.


> # {.exercise title="Chernoff using Stirling" #chernoffstirlingex}
1. Prove that $\Pr_{x\sim \{0,1\}^n}[ \sum x_i = k ] = \binom{n}{k}2^{-n}$.\
2. Use this and [entropybinomex](){.ref} to prove the Chernoff bound for the case that $X_0,\ldots,X_n$ are i.i.d. random variables over $\{0,1\}$ each equaling $0$ and $1$ with probability $1/2$.

> # {.exercise title="Poor man's Chernoff" #poorchernoff}
Let $X_0,\ldots,X_n$ be i.i.d random variables with $\E X_i = p$ and $\Pr [ 0 \leq X_i \leq 1 ]=1$.
Define $Y_i = X_i - p$.  \
1. Prove that for every $j_1,\ldots,j_n \in \N$, if there exists one $i$ such that $j_i$ is odd then $\E [\prod_{i=0}^{n-1} Y_i^{j_i}] = 0$. \
2. Prove that for every $k$, $\E[ (\sum_{i=0}^{n-1} Y_i)^k ] \leq (10kn)^{k/2}$.^[__Hint:__ Bound the number of tuples $j_0,\ldots,j_{n-1}$ such that every $j_i$ is even and $\sum j_i = k$.] \
3. Prove that for every $\epsilon>0$, $\Pr[ |\sum_i Y_i| \geq \epsilon n ] \geq 2^{-\epsilon^2 n / (10000\log 1/\epsilon)}$.^[__Hint:__ Set $k=2\lceil \epsilon^2 n /1000 \rceil$ and then show that if the event $|\sum Y_i | \geq \epsilon n$ happens then the random variable $(\sum Y_i)^k$ is a factor of $\epsilon^{-k}$ larger than its expectation.]



> # {.exercise title="Simulating distributions using coins" #coindistex}
Our model for probability involves tossing $n$ coins, but sometimes algorithm require sampling from other distributions, such as selecting a uniform number in $\{0,\ldots,M-1\}$ for some $M$.
Fortunately,  we can simulate this with an exponentially small probability of error: prove that for every $M$, if $n>k\lceil \log M \rceil$, then there is a function $F:\{0,1\}^n \rightarrow \{0,\ldots,M-1\} \cup \{ \bot \}$ such that __(1)__ The probability that $F(x)=\bot$ is at most $2^{-k}$ and __(2)__ the  distribution of $F(x)$ conditioned on $F(x) \neq \bot$ is equal to the uniform distribution over $\{0,\ldots,M-1\}$.^[__Hint:__ Think of $x\in \{0,1\}^n$ as choosing $k$ numbers $y_1,\ldots,y_k \in \{0,\ldots, 2^{\lceil \log M \rceil}-1 \}$. Output the first such number that is in $\{0,\ldots,M-1\}$. ]

> # {.exercise title="Sampling" #samplingex}
Suppose that a country has 300,000,000 citizens, 52 percent of them prefer the color "green" and 48 percent of which prefer the color "orange". Suppose we sample $n$ random citizens and ask them their favorite color (assume they will answer truthfully). What is the smallest value $n$ among the following  choices so that the probability that the majority of the sample answers "green" is at most $0.05$? \\
a. 1,000 \\
b. 10,000 \\
c. 100,000 \\
d. 1,000,000 \\

> # {.exercise  #exid}
Would the answer to [samplingex](){.ref}  change if the country had 300,000,000,000 citizens?

> # {.exercise title="Sampling (2)" #exid}
Under the same assumptions as [samplingex](){.ref}, what   is the smallest value $n$ among the following  choices so that the probability that the majority of the sample answers "green" is at most $2^{-100}$?
a. 1,000 \\
b. 10,000 \\
c. 100,000 \\
d. 1,000,000 \\
e. It is impossible to get such low probability since there are fewer than $2^{100}$ citizens.

^[TODO: add some exercise about the probabilistic method]

## Bibliographical notes


## Further explorations

Some topics related to this lecture that might be accessible to advanced students include: (to be completed)



## Acknowledgements
