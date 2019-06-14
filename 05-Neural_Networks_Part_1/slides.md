---
theme: boxes
theme: Madrid
mainfont: TeX Gyre Pagella
monofont: DejaVuSansMono
header-includes: \usepackage{tikz}
---

\title{Lecture 5}
\author{Cezar Ionescu \\ 15/06/2019}
\titlegraphic{\includegraphics[scale=0.3]{oudce.jpg}}
\maketitle

Administrative
==============

- Homework from 08/06/2019 due now!

- Please complete and hand in the declarations of authorship.

Questions?
==========

Solution to homework from lecture 2
=======================

Apply Bayes' theorem to answer the following question (Elmer Mode 1966, page 53):

 > A class in advanced mathematics contains 10 juniors, 30 seniors, and 10 graduate students.  Three of the juniors, 10 of the seniors, and 5 of the graduate students received an A in the course.  If a student is chosen at random from this class and is found to have earned an A, what is the probability that he is a graduate student?

Solution to homework from lecture 2
=======================

> Ω = {student₁, ..., student₅₀}, Event = ℙ(Ω)
> p(X) = card(X)/card(Ω)

Events of interest:

> G  =  {x | x ∈ Ω, x is a graduate student}
> A  =  {x | x ∈ Ω, x has earned an A}

Solution to homework from lecture 2
=======================

We want ```p(G | A)```{.haskell}.  By Bayes' theorem

> p(G | A) = p(A | G) * p(G) / p(A)

> p(A) = card(A) / card(Ω) = 18/50
> p(G) = card(G) / card(Ω) = 10/50
> p(A | G) = p(A ∩ G) / p(G) = card(A ∩ G) / card(G) = 5/10

Therefore

> p(G | A) = 5/18 = 0.278

Solution to homework from lecture 3
========

(based on Nilsson, 19.4, page 340) Consider the following Bayesian network:

![](burglary.png){ height=55% }

Compute ```p(¬J, ¬M, A, B, E)```{.haskell}.  This is the probability that there is both an earthquake and a burglary, the alarm rings, but neither John nor Mary call.

Solution to homework from lecture 3
========

>    p(¬J, ¬M, A, B, E) 
> = 
>    p(¬J | ¬M ∩ A ∩ B ∩ E) * p(¬M | A ∩ B ∩ E) * 
>    p(A | B ∩ E) * p(B | E) * p(E)
> =
>    p(¬J | A) * p(¬M | A) * p(A | B ∩ E) * p(B) * p(E)
> =
>    0.1 * 0.3 * 0.95 * 0.001 * 0.002
> =  5.7 * 10^(-8)

McCulloch & Pitts
=================

- McCulloch & Pitts 1943 *A Logical Calculus of the Ideas Immanent in Nervous Activity*:

 > "neural events and the relations among them can be treated by means of propositional logic"

- The MC & P neuron had a number of boolean inputs, some positive and others negative.  The neuron was activated if the number of active positive inputs was greater than the number of active negative inputs plus a "threshold":

> mc_p_neuron : ℕ -> ({0, 1}ⁿ, {0, 1}ᵐ) -> {0, 1}
> mc_p_neuron θ (pos, neg) = if sum neg > 0 then 0
>                            else if sum pos ≥ θ then 1
>                                    else 0

Examples
========

![McCulloch & Pitts neuronal networks](mcp_neurons.png)

Logic with McCulloch & Pitts neurons
====================================

Logical functions:

> not : {0, 1} -> {0, 1}
> not x = mc_p_neuron 0 ([], [x])

> and : ({0, 1}, {0, 1}) -> {0, 1}
> and (x, y) = mc_p_neuron 2 ([x, y],[]) 

"Exclusive or" with McCulloch & Pitts neurons
====================================

> xor : ({0, 1}, {0, 1}) -> {0, 1}
> xor (x, y) = mc_p_neuron 2 ([case₁, case₁, case₂, case₂], [])
>  where
>  case₁ = mc_p_neuron 2 ([x, x], [y])
>  case₂ = mc_p_neuron 2 ([y, y], [x])

"Exclusive or" with McCulloch & Pitts neurons
====================================

![Logical "xor" implemented with McCulloch & Pitts neurons](mcp_xor.jpg)

Perceptrons
===========

- Frank Rosenblatt 1957

- A perceptron had real-valued inputs and binary outputs.  The output was a step function of the weighted sum of these inputs

> perceptron : (ℝⁿ, ℝ) -> ℝⁿ -> {0, 1}
> perceptron ([w₁,..., wₙ], θ) [x₁, ..., xₙ] = if s ≥ θ then 1 
>                                                      else -1
>  where s = w₁*x₁ + ... + wₙ * xₙ

Perceptrons
===========

![Rosenblatt's perceptron (1961)](rosenblatt_1961.png){ height=70% }

Logic with perceptrons
======================

Logical functions:

> not x      = perceptron ([-1], 0) [x]

> and (x₁, x₂) = perceptron ([0.5, 0.5], 1) [x₁, x₂]

Logic with perceptrons
======================

![Logical "and" implemented with a perceptron](perc_and.jpg)

"Exclusive or" with perceptrons
===============================

> xor (x₁, x₂) = perceptron ([1, 1], 0) [case₁, case₂]
>  where
>  case₁ = perceptron ([0.5, -0.5], 0) [x₁, x₂]
>  case₂ = perceptron ([-0.5, 0.5], 0) [x₁, x₂]

"Exclusive or" with perceptrons
===============================

![Logical "xor" implemented with perceptrons](perc_xor.jpg)


Homework
========

Implement the "two out of three" function defined by:

> two_of_three : ({0, 1}, {0, 1}, {0, 1}) -> {0, 1}
> two_of_three (x, y, z)
>  | (1, 1, 0)  =  True
>  | (1, 0, 1)  =  True
>  | (0, 1, 1)  =  True
>  | otherwise  =  False

using 

a) McCulloch & Pitts neurons

b) perceptrons

Perceptrons
===========

The perceptron training rule:

> wᵢ <- wᵢ + η * (t - o) * xᵢ

The threshold ```θ```{.haskell} is treated as the weight of an always active input.

Gradient descent
================

Why does the perceptron training rule work?

"Naive" gradient descent:

> x <- x - η * D f (x)

What is ```f```{.haskell} in our case?

Error functions
===============

> t - o, |t - o|, (t - o)², (t - o)⁴, ...

Bayesian learning and error minimisation
========================================

> data: (x₁, t₁), ..., (xₙ, tₙ), xᵢ ∈ ℝᵐ, tᵢ ∈ ℝ
> hypothesis: w ∈ ℝᵐ

> hₘₐₚ = argmax p(h | d)
>      = argmax p(d | h) * p(h) / p(d)
>      = argmax p(d | h) * p(h)
>      -- assume p(h) = const
>      = argmax p(d | h)
>      = hₘₗ

Bayesian learning and error minimisation
========================================

> p(d | h) = p((x₁, t₁), ..., (xₙ, tₙ) | w)
>   = p(x₁, t₁ | w)* ...*p(xₙ, tₙ | w)

> hₘₗ = argmax p(d | h)
>     = argmax ln p(d | h)
>     = argmax ln p(x₁, t₁ | w) + ...+ ln p(xₙ, tₙ | w)

Bayesian learning and error minimisation
========================================

Assume the ```f(xᵢ, w)```{.haskell} are normally distributed around the ```tᵢ```{.haskell}, with the **same** ```σ```{.haskell}:

> p(xᵢ, tᵢ | w) = 1/√(2 * π * σ²) exp (-(tᵢ - f(xᵢ, w))²/(2 * σ^2))

Therefore

> ln p(xᵢ, tᵢ | w) = ln 1/√(2 * π * σ²)-(tᵢ - f(xᵢ, w))²/(2 * σ^2)
>                  = k - (tᵢ - f(xᵢ, w))²/(2 * σ^2)

Bayesian learning and error minimisation
========================================

> hₘₗ = argmax ln p(x₁, t₁ | w) + ...+ ln p(xₙ, tₙ | w)
>     = argmax n * k - Σ (tᵢ - f(xᵢ, w))²/(2 * σ^2)
>     = argmax - Σ (tᵢ - f(xᵢ, w))²
>     = argmin Σ (tᵢ - f(xᵢ, w))²

Bayesian learning and error minimisation
========================================

Therefore, the correct error to choose is the sum of squared errors...

...at least if: 

- all hypothesis are equally likely a-priori,

- the errors are independent, and

- the errors have identical normal distributions.

Perceptrons and learning
========================

- Using the learning rule, we can iteratively find the weights and threshold for any perceptron (such as the one implementing ```and```{.haskell}).

- However, the learning rule does not apply to multi-layer perceptrons, such as the one needed for ```xor```{.haskell}.

Linear separability
===================

- Perceptrons can only classify linearly separable sets, but ```xor```{.haskell} is not linearly separable.

- The 1969 book *Perceptrons* by Minsky and Pappert pointed out these facts, killing funding for neural networks for a decade or more.


