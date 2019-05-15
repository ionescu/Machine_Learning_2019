---
title: Lecture 2
monofont: DejaVuSansMono
author: Cezar Ionescu
geometry: "left=2.5cm,right=2.5cm,top=2.5cm,bottom=2.5cm"
mainfont: TeX Gyre Pagella
fontsize: 12pt
monofont: DejaVuSansMono
...

Probability theory
------------------

- The first question of probability theory is "probability of what?".
  - Richard von Mises (1883-1953): "The term probability is meaningful for us only with regard to a clearly defined collective[^1] (or population." (*Mathematical Theory of Probability and Statistics*, page 17).
  - Sir Harold Jeffreys (1891-1989): "there is a valid primitive idea expressing the degree of confidence that we may reasonably have in a proposition" (*Theory of Probability, 3rd Ed.*, page 15).
  
[^1]: Intuitively, a *collective* is an infinite sequence of results of a repeated experiment.

- The currently accepted mathematical model was developed by Kolmogorov in 1933 and covers both interpretations.
  - Ingredients:
      - Ω, the set of all possible outcomes
      - ```Event```{.haskell}, the set of all events ```Event ⊆ P(Ω)```{.haskell}[^2]
      - ```p : Event → [0, 1]```{.haskell}, the probability measure

[^2]: ```P(X)```{.haskell} denotes the set of all subsets of ```X```{.haskell}, including ```∅```{.haskell} and ```X```{.haskell} itself.

- **Examples**:
  1. Drawing a card from a standard deck.  Any of the 52 cards is a possible result, so ```Ω = {2♣, 2♦, 2♥, 2♠, ...}```{.haskell}.  An event is what we want to assign a probability to, for example "the card drawn is a spade", or "a red card between 7 and 10".  Any event can be represented by a subset of ```Ω```{.haskell}; when ```Ω```{.haskell} is finite, we take *all* subsets, so that ```Event = P(Ω)```{.haskell}.
  2. Choosing a point of the unit disc.  This can be seen as an idealised model for a game of darts.  ```Ω = {(x, y) | x, y ∈ ℝ, x² + y² ≤ 1}```{.haskell}.  Events are of the form "the point will be chosen from this or that subset of the unit disc", so in principle we would like again to have ```Event = P(Ω)```{.haskell}.  It is an annoying but inescapable mathematical fact that we cannot, since that would make it impossible to define a good probability measure, so we have to limit the number of subsets that we can assign probabilities to.  The mathematical choice is that of a *Borel σ-algebra*, which is "almost as big" as ```P(Ω)```{.haskell}.  We can be even less ambitious and choose ```Event```{.haskell} to be the set of all *computable* subsets of ```Ω```{.haskell} (i.e., all those whose characteristic function ```X : Ω → {0, 1}```{.haskell} can be implemented as a program).
  
  
