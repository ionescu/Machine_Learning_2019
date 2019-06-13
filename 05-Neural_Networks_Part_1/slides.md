---
theme: boxes
theme: Madrid
mainfont: TeX Gyre Pagella
monofont: DejaVuSansMono
header-includes: \usepackage{tikz}
---

\title{Lecture 5}
\author{Cezar Ionescu}
\date{15/06/2019}
\titlegraphic{\includegraphics[scale=0.4]{oudce.jpg}}
\maketitle

Administrative
==============

- Homework from 08/06/2019 due now!

- Please complete and hand in the declarations of authorship.

Questions?
==========

McCulloch & Pitts
=================

- McCulloch & Pitts 1943 *A Logical Calculus of the Ideas Immanent in Nervous Activity*

  > "neural events and the relations among them can be treated by means of propositional logic"

- The MC & P neuron had a number of boolean inputs, some positive and others negative.  The neuron was activated if the number of active positive inputs was greater than the number of active negative inputs plus a "threshold":

> mc_p_neuron : ℝ -> ({0, 1}ⁿ, {0, 1}ᵐ) -> {0, 1}
> mc_p_neuron θ (pos, neg) = if sum(pos) - sum(neg) ≥ θ 
>                            then 1 else 0

McCulloch & Pitts
=================

Logical functions:

> not : {0, 1} -> {0, 1}
> not x = mc_p_neuron (-0.5) ([], [x])

> and : ({0, 1}, {0, 1}) -> {0, 1}
> and (x, y) = mc_p_neuron (-1.5) ([x, y],[]) 


Perceptrons
===========

- Frank Rosenblatt 1957

- An FR neuron had real-valued inputs and binary outputs.  The output was a step function of the weighted sum of these inputs

> fr_neuron : (ℝⁿ, ℝ) -> {0, 1}ⁿ -> {0, 1}
> fr_neuron ([w₁,..., wₙ], θ) [x₁, ..., xₙ] = if s ≥ θ then 1 
>                                                      else -1
>  where s = w₁*x₁ + ... + wₙ * xₙ

Perceptrons
===========

Logical functions:

Perceptrons
===========

The perceptron training rule:

> wᵢ <- wᵢ + η * (t - o) * xᵢ

Perceptrons
===========

The case of ```xor```{.haskell}:

Perceptrons
===========

Linear separability

Perceptrons
===========

Implementing ```xor```{.haskell}

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


