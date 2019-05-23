---
title: Homework from 2019-05-11
monofont: DejaVuSansMono
author: Cezar Ionescu
geometry: "left=2.5cm,right=2.5cm,top=2.5cm,bottom=2.5cm"
mainfont: TeX Gyre Pagella
fontsize: 12pt
monofont: DejaVuSansMono
...


Handwriting recognition
-----------------------

- ```Task = Handwritten → Standard```{.haskell}

- ```Experience = List (Handwritten, Standard)```{.haskell}

- ```perf : (Task, List (Handwritten, Standard)) → ℝ```{.haskell}

> perf (recog, [(h₁, s₁), ..., (hₙ, sₙ)]) = 
>   sum [correct (h₁, s₁), ..., correct (hₙ, sₙ)] * 100 / n 
>   where
>   correct (h, s)  =  if recog(h) = s then 1 else 0

