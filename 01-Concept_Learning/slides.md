---
theme: boxes
theme: Madrid
mainfont: TeX Gyre Pagella
monofont: DejaVuSansMono
---

\title{Lecture 1}
\author{Cezar Ionescu}
\date{11/05/2019}
\titlegraphic{\includegraphics[scale=0.4]{oudce.jpg}}
\maketitle

Administrative matters
======================

- course materials [on GitHub](https://github.com/ionescu/Machine_Learning_2019)
  - lectures notes will generally be available after the lecture, though drafts may appear before
  - exercises will also be posted on GitHub

Administrative matters
======================

- register for CATS points
  - assessment: homework to be handed in a the next meeting (or sent to me by email, or to PPWeekly)
  - every piece of homework is pass or fail

Administrative matters
======================

- course takes place Saturdays 10:00-12:30 at Ewert House
  - exceptions: **no class** on the **4th of May** and **1st of June**!

Administrative matters
======================

- main text: *Machine Learning*, Tom Mitchell, 1997
  - there are a couple of copies available in the ContEd library
  - you can buy it used for under 15 GBP on [Amazon](https://www.amazon.co.uk/gp/offer-listing/1259096955/ref=sr_1_3_olp?keywords=mitchell+machine+learning&qid=1555784813&s=gateway&sr=8-3)
  - but you should be able to complete the course using only the lecture notes

Types of learning
============

- types of learning:
  - by rote
  - conditioning
  - from experience
  - any others?

Why "machine learning"?
============

- reasons for *machine learning*:
  - programming is hard, it would be better if computers learnt by themselves
  - to study human learning (and intelligence)
    - perhaps we could then improve our own abilities to learn and to teach

Approaches to ML
============

- two main approaches:
  - modelling how we think and learn, without caring about the underlying physiological mechanisms
  - modelling the underlying physiological mechanism, without caring how they lead to thinking and learning

Definition
============

- **Definition** (Mitchell, p. 2) A computer program is said to **learn**
  from experience *E* with respect to some class of tasks *T* and
  performance measure *P*, if its performance at tasks in *T*, as
  measured by *P*, improves with experience *E*.

Sets and functions
==================

**Notation**:

- ```f : In → Out```{.haskell} 

- ```f(x)```{.haskell} 

- ```x ∈ In, f(x) ∈ Out```{.haskell} 
  
Black boxes versus functions
============================

- is ```Random.choice(seq)```{.Python} a function?

- we use ```⇝```{.haskell} instead of ```→```{.haskell} when we need to distinguish black boxes from "real" functions
  
Mathematical representation
===========================

- ```Task = In → Out```{.haskell}
- ```Experience = List E```{.haskell}
- ```perf : (Task, List Out) → ℝ```{.haskell}
- ```learn : Experience → Task```{.haskell}
- for all ```ex₁, ex₂, ins```{.haskell}, we have
  
>    perf (learn ex₁) ins <= perf (learn (es₁ ++ es₂)) ins

Example: checkers learning
==========================
- Task: ```Play = Board → Move```{.haskell}
- Experience: ```Experience = Play → List Game```{.haskell}
    - ```play : (Play, Play) → Game```{.haskell}
    - the list of games is created by giving the ```play```{.haskell} function the same argument *twice*
- Measure of performance: ```perf : (Play, List Play) → ℝ```{.haskell} 
  
> perf (learner, [adv₁, ..., advₙ]) =
>   percentage [score(play(learner, adv₁)), ..., score(play(learner, advₙ))]

Example: self-driving car
=========================

- Task: ```Drive = Sensor → Steer```{.haskell}
- Experiences: ```Experience = List (Sensor, Steer)```{.haskell}
- Measure of performance: ```perf : (Drive, Itinerary) → Time```{.haskell}
    - ```perf (learner, itinerary)``` how long the learner drives along the given itinerary before making a mistake

Homework
========

- Give a similar interpretation for the handwriting recognition problem (Mitchell, page 3).
  
Concept learning
================

- idea: acquiring general concepts from examples
  - e.g., learn to recognise cats from images of animals

- what is a concept?
  - *nominalistic* view: the set of instances of the concept
  
Mathematical representation of concepts
=======================================

- mathematically, we can identify a concept with a subset
  - e.g., ```X```{.haskell} is the set of all images of animals, ```C ⊆ X``{.haskell} is the subset of images of cats
  
- subsets are in one-to-one correspondence with boolean-valued functions 
  - ```C ⊆ X```{.haskell} can be replaced by ```c : X → Bool```{.haskell} such that
  
```
    ∀ x ∈ X  c(x) = 1  iff x ∈ C
```{.haskell}

Definition
==========

- Mitchell uses the functional view and defines: 
  - **Concept learning:** inferring a boolean-valued function from training examples of input and output
  
- **Exercise:** Give an interpretation of concept learning as a learning task (i.e., identify the task, the experience, and the performance measure).

Notation
========

- the training data ```D = {((x₁, c(x₁)) ..., (xₙ, c(xₙ))}```{.haskell}
- the subset of negative training examples ```D₀ = {(x, 0) | (x, 0) ∈ D}```{.haskell}
- the subset of positive training examples ```D₁ = {(x, 1) | (x, 1) ∈ D}```{.haskell}

Example
=======
- we want to learn the concept ```enjoyable : Day → {0, 1}```{.haskell}
- days are described via *attributes*:
  - ```Day = (Sky, Temp, Humidity, Wind, Water, Forecast)```{.haskell}
    - ```Sky = {Sunny, Cloudy, Rainy}```{.haskell}
    - ```Temp = {Warm, Cold}```{.haskell}
    - ```Humidity = {Normal, High}```{.haskell}
    - ```Wind = {Strong, Weak}```{.haskell}
    - ```Water = {Warm, Cool}```{.haskell}
    - ```Forecast = {Same, Change}```{.haskell}
    
Training data
=============
  
Nr    Sky    Temp    Humidity   Wind     Water   Forecast   Enjoyable
---   ----   ----    --------   ----     -----   --------   ---------
1     Sunny  Warm    Normal     Strong   Warm    Same       Yes
2     Sunny  Warm    High       Strong   Warm    Same       Yes
3     Rainy  Cold    High       Strong   Warm    Change     No
4     Sunny  Warm    High       Strong   Cool    Change     Yes

Two problems
============

- ```Day```{.haskell} contains 96 elements; there are $2^{96}$ concepts.

- We can represent a concept by a lookup table, but not if it's too big!

- The training data does not suffice to determine the concept we are looking for. 

Decisions
=========

- The two problems force us to make two decisions:

  1. How to represent **some** of the concepts (the hypotheses space)
  
  2. Which hypothesis to pick.
  
Inductive bias
==============

- The assumptions under which we manage to learn the correct concept form **the inductive bias**.

```Find-S```
============

- ```Find-S``` solves problem 2 by choosing the *most specific* hypothesis that is consistent with the training data.  

- Our hypothesis correspond to subsets.  We have a natural ordering on subsets: ```⊆```{.haskell}.

```Find-S``` algorithm
================

```{.haskell}

-- input: training data {((x₁, c(x₁)) ..., (xₙ, c(xₙ))}
--        hypothesis set H
h = min H -- set the current hypothesis h to "the" (or "a") smallest element of H
for i in 1:n
  if c(xᵢ) = 0 
    then keep h
    else if xᵢ ∈ h then keep h
                   else h = min {h' ∈ H | h ⊆ h' and xᵢ ∈ h'}
-- output: "the" (or "a" most) specific hypothesis in H consistent with the training data
```

Remarks
=======

- If ```H```{.haskell} contains all possible concepts, then the result of ```Find-S``` is ```D₁```.
- A bad situation for ```Find-S```:
  - ```X = {a, b, c, d}, H = {∅, {a, b}, {a, c}}, D₁ = {a}```{.haskell}
- The choice of ```H```{.haskell} can avoid these problems.

```Find-S``` and the weather example
====================================

- Hypothesis space for the weather example:
  - each hypothesis is described by a tuple of ```(Sky*, Temp*, Humidity*, Wind*, Water*, Forecast*)```{.haskell}, where ```S* = S ∪ {?, ∅}```{.haskell}
  - notation: ```s ~ s* iff s = s* or s* = ?``` (```s``` matches ```s*```)
  
Let ```h``` be described by ```(s*, t*, u*, wi*, wa*, f*)```.  Then

> h (s, t, u, wi, wa, f) = s ~ s* and t ~ t* and u ~ u* and wi ~ wi* and wa ~ wa* and f ~ f*



