---
theme: boxes
theme: Madrid
mainfont: TeX Gyre Pagella
monofont: DejaVuSansMono
header-includes: \usepackage{tikz}
---

\title{Lecture 2}
\author{Cezar Ionescu}
\date{18/05/2019}
\titlegraphic{\includegraphics[scale=0.4]{oudce.jpg}}
\maketitle

Administrative
==============

**No lecture on the 1st of June!**

Homework from 2019-05-11 due **now**!

Questions?
==========


Probability theory
==================
:::::: {.columns}
::: {.column width="49%"}
Richard von Mises (1883-1953): *The term probability is meaningful for us only with regard to a clearly defined collective (or population).*
:::
::: {.column width="49%"}
![](von_mises.jpg){ height=40% }
:::
::::::

\ 

:::::: {.columns}
::: {.column width="49%"}
Sir Harold Jeffreys (1891-1989): *[T]here is a valid primitive idea expressing the degree of confidence that we may reasonably have in a proposition.*
:::
::: {.column width="49%"}
![](jeffreys.jpg){ height=40% }
:::
::::::

Mathematical model
==================

:::::: {.columns}
::: {.column width="60%"}

A. N. Kolmogorov (1903-1987):

- ```Ω```{.haskell}, the set of all possible outcomes

- ```Event```{.haskell}, the set of all events ```Event ⊆ ℙ(Ω)```{.haskell}

- ```p : Event → [0, 1]```{.haskell}, the probability measure, satisfying

  1. ```p(∅) = 0, p(Ω) = 1```{.haskell}
  
  2. for all events ```X₁, ..., Xₙ, ...```{.haskell} pairwise disjoint, ```Σ p(Xᵢ) = p(⋃ Xᵢ)```{.haskell}

:::
::: {.column width="40%"}
![](kolmogorov.jpg){ width=100% }
:::
::::::

Examples
========

1. Drawing a card from a standard deck.

   - ```Ω = {2♣, 2♦, 2♥, 2♠, ...}```{.haskell}
   
   - ```Event = ℙ(Ω)```{.haskell}

2. Choosing a point of the unit disc.

  - ```Ω = {(x, y) | x, y ∈ ℝ, x² + y² ≤ 1}```{.haskell}  
  - We would like to have ```Event = ℙ(Ω)```{.haskell} ... but we can't
    
  - We can take ```Event = {X : Ω → {0, 1} | X computable}```{.haskell}
  
The classical model of probability
==================================

```Ω```{.haskell} finite, ```Event = ℙ(Ω)```{.haskell}, and ```p```{.haskell} defined by

> p(X) = card(X) / card(Ω)

Interpretations:

- Frequentist: every elementary outcome ```ω ∈ Ω```{.haskell} turns up with approximately the same frequency as any other.

- Bayesian: we have no reason to prefer one elementary outcome over any other.

Example
=======

Rolling a die:

- ```Ω = {1, 2, 3, 4, 5, 6}, card(Ω) = 6```{.haskell}

- rolling an even number: ```even = {2, 4, 6}, card(even) = 3```{.haskell}

- probability of rolling an even number: ```p(even) = card(even)/card(Ω) = 3/6 = 0.5```{.haskell}

Operations with events
======================

:::::: {.columns}
::: {.column width=60%}

- union: ```A ∪ B```{.haskell}
- intersection: ```A ∩ B```{.haskell}
- complement: ```¬ A = Ω - A```{.haskell}

----------------------

- result is even **and** ```≤ 3```{.haskell}
- result is even **or** ```≤ 3```{.haskell}
- result is **not** ```≤ 3```{.haskell}

:::

::: {.column width=40%}

\begin{tikzpicture}[scale=0.7]
  \draw (2.5, 2.5) circle [radius=2.5];
  \draw[fill=blue, opacity=0.5] (2.0, 3.0) circle [radius=1.6];
  \draw[fill=pink, opacity=0.5] (3.0, 2.0) circle [radius=1.6];
  \node at (2.1, 4.2) {1};
  \node at (2.5, 2.6) {2};
  \node at (1.0, 3.0) {3};
  \node at (4.0, 1.8) {4};
  \node at (3.0, 1.0) {6};
  \node at (1.2, 1.1) {5};
\end{tikzpicture}

:::
::::::

Exercises
=========

:::::: {.columns}
::: {.column width=60%}

1. Derive the formula for ```p(¬A)```{.haskell}.
2. Derive the formula for ```p(A ∪ B)```{.haskell}
3. Assume that ```B₁, ..., Bₙ```{.haskell} are pairwise disjoint.  Show that

> p(A) = p(A ∩ B₁) + ... + p(A ∩ Bₙ)

:::

::: {.column width=60%}

\begin{tikzpicture}
  \draw (2.5, 2.5) circle [radius=2.5];
  \node at (1.5, 0.6) {¬B};
  \node at (0.9, 1.3) {B};
  \draw (2.5-1.76, 2.5-1.76) -- (5, 2.5);
  \draw[fill=blue, opacity=0.5] (2.5, 2.5) circle [radius=1.6];
  \node at (2.5, 2.6) {A};
\end{tikzpicture}

:::
::::::

Conditional probability
=======================

**Definition**: Let ```(Ω, Event, p)```{.haskell} as above, ```X ∈ Event```{.haskell} s.t ```p(X) ≠ 0```{.haskell}.  Let ```Y ∈ Event```{.haskell}.  The **conditional probability** of ```Y```{.haskell} given ```X```{.haskell} is defined by

> p(Y | X) = p(Y ∩ X) / p(X)

Notational problems
===================

- is ```|```{.haskell} a set-theoretical operation?

- is ```|```{.haskell} commutative?

- Kolmogorov's original notation: ```p```{.haskell}$_{X}$```(Y)```{.haskell}

The law of total probability
============================

```B₁, ..., Bₙ```{.haskell} pairwise disjoint, ```p(Bᵢ) ≠ 0```{.haskell} for all ```i```{.haskell}.  Show that

> p(A) = p(A | B₁)*p(B₁) + ... + p(A | Bₙ)*p(Bₙ)

Bayesian learning
=================

- Set of hypotheses, ```H```{.haskell}

- Data, ```d ∈ D```{.haskell}  

- Find the "best" hypothesis that fits the data

  - e.g., ```Find-S```{.haskell}, "best" = the most specific hypothesis consistent with the data.  
  
- Bayesian learning:  "best" = *the most probable* hypothesis, given the data.

Maximum a posteriori hypothesis
=====================================

The aim of Bayesian learning is to determine the **maximum a posteriori (MAP) hypothesis**:

> hₘₐₚ = argmaxₕ p(h | d)

What does ```p(h | d)```{.haskell} mean?

Bayes' theorem
==============

```X, Y ∈ Event```{.haskell}, ```p(X) * p(Y) ≠ 0```{.haskell}.

> p(X | Y) = p (X ∩ Y) / p (Y) 
>     therefore p(X ∩ Y) = p(X | Y) * p(Y)
> p(Y | X) = p (Y ∩ X) / p (X)
>     therefore p(Y ∩ X) = p(Y | X) * p(X)

```X ∩ Y = Y ∩ X ⇒ p(X ∩ Y) = p(Y ∩ X)```{.haskell}, therefore:

> p(X | Y) = p(Y | X) * p(X) / p(Y)

Example
=======

Mitchell, pages 157-158:

- ```H =  {healthy, ill}```{.haskell}

- test results: ```R = {⊖, ⊕}```{.haskell}

- ```0.008```{.haskell} of the population have the disease.

- patient ill ```⇒```{.haskell} test positive in ```98%```{.haskell} of the cases  

- patient healthy ```⇒```{.haskell} test positive in ```3%```{.haskell} of the cases  

- A patient's test has come back positive.  

What is the MAP hypothesis?

Translation to probability-speak
================================

- ```Ω = ```{.haskell} the set of people in the population

- ```Event = ℙ(Ω)```{.haskell}

- classical model for ```p```{.haskell}: ```p(X) = card(X)/card(Ω)```{.haskell}

- the events of interest: ```Healthy```{.haskell}, ```Ill```{.haskell}, ```Pos```{.haskell}, ```Neg```{.haskell}

- ```p(healthy) = 0.992```{.haskell}, ```p(ill) = 0.008```{.haskell}, ```p(⊕ | ill) )  = 0.98```{.haskell}, ```p(⊕ | healthy) = 0.03```{.haskell}

Compute ```p(ill | ⊕)```{.haskell}.

Applying Bayes' theorem
=======================

>     p(ill | ⊕) 
> =   p(⊕ | ill) * p(ill) / p(⊕)
> =   0.98 * 0.008 / p(⊕)
> =   0.00784 / p(⊕)

>     p(healthy | ⊕) 
> =   p(⊕ | healthy) * p(healthy) / p(⊕)
> =   0.03 * 0.992 / p(⊕)
> =   0.02976 / p(⊕)

Therefore, the MAP hypothesis is ```healthy```{.haskell}.

A general argument
==================

> hₘₐₚ = argmaxₕ p(h | d)
>      = argmaxₕ (p(d | h) * p(h) / p(d))
>      = argmaxₕ (p(d | h) * p(h)) -- p(d) is constant w.r.t. h

Likelihood
==========

If all hypotheses are equally likely:

> hₘₐₚ = argmaxₕ p(h | d)
>      = argmaxₕ p(d | h)

```p(d | h)```{.haskell} is called the **likelihood** of ```d```{.haskell} given ```h```{.haskell}.  

A hypothesis that maximises the likelihood is a *maximum likelihood hypothesis (ML)*.  Therefore

> hₘₗ = argmaxₕ p(d | h)

In general, ```hₘₐₚ ≠ hₘₗ```{.haskell}!  

Exercise
========

What is ```hₘₗ```{.haskell} in the example above?

Homework
========

Apply Bayes' theorem to answer the following question (Elmer Mode 1966, page 53):

 > A class in advanced mathematics contains 10 juniors, 30 seniors, and 10 graduate students.  Three of the juniors, 10 of the seniors, and 5 of the graduate students received an A in the course.  If a student is chosen at random from this class and is found to have earned an A, what is the probability that he is a graduate student?

Concept learning revisited
==========================

Concept:

> c : X → {0, 1}

```h ∈ H ⇒ h : X → {0, 1}```{.haskell})

We assume ```c ∈ H```{.haskell}

The data:

> d = {(x₁, c(x₁)), ..., (xₘ, c(xₘ))}

"Brute-force" Bayesian concept learning
=======================================

> given H, p(h)
> for every h ∈ H:
>   compute p(h | d) = p(d | h) * p(h) / p(d)
>   return hₘₐₚ = argmaxₕ p(h | d)

The problem is computing, for every ```h```{.haskell}, ```p(d | h)```{.haskell}.  We also want to compute ```p(d)```{.haskell}.

Computing the likelihood
========================

Assumption: the data is completely correct, and hypotheses are not "noisy" functions.  

> p(d | h) = if for all (xᵢ, bᵢ) ∈ d  h(xᵢ) = bᵢ
>               then 1
>               else 0

Computing ```p(d | h)```{.haskell}
==================================

We can now compute ```p(d)```{.haskell} by applying the law of total probability:

> p(d) = Σₕ p(d | h) * p(h)

In a certain sense, the data and the hypothesis must have the same nature.

Consistent learners
===================

Assume all hypotheses are equally likely.  Then the brute-force Bayesian concept learning algorithm will return an ML hypothesis:

> hₘₐₚ = hₘₗ = argmaxₕ p(d | h)

Since ```p(d | h) = 1```{.haskell} if ```h```{.haskell} is consistent with the data, and ```p(d | h) = 0```{.haskell} otherwise, the algorithm will return any one of the hypotheses in ```H```{.haskell} consistent with ```d```{.haskell}.


Bayesian concept learning and ```Find-S```
==========================================

```Find-S```{.haskell} returns a consistent hypothesis ```⇒```{.haskell} it returns a MAP (and an ML) hypothesis!

Even if ```p```{.haskell} is not constant w.r.t. hypotheses, ```Find-S```{.haskell} can still return a MAP hypothesis, if there is a unique most specific hypothesis consistent with ```d```{.haskell} in ```H```{.haskell} and

> hᵢ ⊆ hⱼ ⇒ p(hᵢ) ≥ p(hⱼ)



