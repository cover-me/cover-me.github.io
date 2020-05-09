---
layout: post
title: The phase daigram of a single dot coupled to a superconductor
---
For a single dot:

$$
H = \epsilon (n_\uparrow + n_\downarrow) + U n_\uparrow n_\downarrow + \Delta (c^\dagger_\uparrow c^\dagger_\downarrow + c_\downarrow c_\uparrow)
$$

Let $\left|n_\uparrow n_\downarrow\right> = \left|00\right>,\left|01\right>,\left|10\right>,\left|11\right>$ be the basis, we have

$$
c_\uparrow = \begin{bmatrix}
0 & 0&1 &0\\
0 &0& 0&1\\
0&0 & 0&0\\
0& 0&0 &0\\
\end{bmatrix}
$$

$$
c_\downarrow = \begin{bmatrix}
0 & 1&0 &0\\
0 &0& 0&0\\
0&0 & 0&-1\\
0& 0&0 &0\\
\end{bmatrix}
$$

So

$$
H = \left[
\begin{array}{cccc}
 0 & 0 & 0 & \Delta  \\
 0 & \epsilon  & 0 & 0 \\
 0 & 0 & \epsilon  & 0 \\
 \Delta  & 0 & 0 & U+2 \epsilon  \\
\end{array}
\right]
$$

Eigensystem (by Mathematica):

$$
\left(
\begin{array}{cccc}
 \epsilon  & \epsilon  & -\sqrt{\Delta ^2+\frac{U^2}{4}+U \epsilon +\epsilon ^2}+\frac{U}{2}+\epsilon  & \sqrt{\Delta ^2+\frac{U^2}{4}+U \epsilon +\epsilon ^2}+\frac{U}{2}+\epsilon  \\
 \{0,0,1,0\} & \{0,1,0,0\} & \left\{-\frac{\sqrt{\Delta ^2+\frac{U^2}{4}+U \epsilon +\epsilon ^2}+\frac{U}{2}+\epsilon }{\Delta },0,0,1\right\} & \left\{-\frac{-\sqrt{\Delta ^2+\frac{U^2}{4}+U \epsilon +\epsilon ^2}+\frac{U}{2}+\epsilon }{\Delta },0,0,1\right\} \\
\end{array}
\right)
$$

The ground state energy can be either $\epsilon$ or $-\sqrt{\Delta ^2+\frac{U^2}{4} + U \epsilon + \epsilon ^2} + \frac{U}{2}+\epsilon$, the transition happens when this two energies are euqal, that is

$$
\frac{U^2}{4} = \Delta^2 + (\frac{U}{2}+\epsilon)^2
$$

or (can U be negative?)

**P.S.**

If $U = 0$, the spectrum is $\{E_1,E_2,E_3,E_4\} = \{\epsilon,\epsilon,-\sqrt{\Delta^2+\epsilon^2}+\epsilon,\sqrt{\Delta^2+\epsilon^2}+\epsilon\}$. The ground state engergy is $E_3$, while the exciting energy is $\sqrt{\Delta^2 + \epsilon^2}$ or $2\sqrt{\Delta^2 + \epsilon^2}$, just like there are 0, 1, or 2 quasiparticles whoes energy is $\sqrt{\Delta^2 + \epsilon^2}$.


We then try the BdG Hamiltonian mentioned in [this](2019/05/11/something-about-particle-hole-symmetry.html) post as a test.

$$
\begin{align}
H &= \epsilon (n_\uparrow + n_\downarrow) + \Delta (c^\dagger_\uparrow c^\dagger_\downarrow + c_\downarrow c_\uparrow)\\
&= \epsilon (c_\uparrow^\dagger c_\uparrow + c_\downarrow^\dagger c_\downarrow) + \Delta(...)\\
&= \frac{\epsilon}{2}(c_\uparrow^\dagger c_\uparrow + c_\uparrow^\dagger c_\uparrow + c_\downarrow^\dagger c_\downarrow + c_\downarrow^\dagger c_\downarrow) + \Delta(...)\\
&=\frac{\epsilon}{2}(c_\uparrow^\dagger c_\uparrow + 1-c_\uparrow c_\uparrow^\dagger + c_\downarrow^\dagger c_\downarrow + 1-c_\downarrow c_\downarrow^\dagger) + \Delta(...)\\
&=\epsilon + \frac{\epsilon}{2}(c_\uparrow^\dagger c_\uparrow -c_\uparrow c_\uparrow^\dagger + c_\downarrow^\dagger c_\downarrow -c_\downarrow c_\downarrow^\dagger) + \Delta(c^\dagger_\uparrow c^\dagger_\downarrow + c_\downarrow c_\uparrow)
\end{align} 
$$

Let $c_\uparrow,c_\downarrow,c_\uparrow^\dagger,c_\downarrow^\dagger$ be $(1,0,0,0),(0,1,0,0),(0,0,1,0),(0,0,0,1)$. we get,

$$
c_\uparrow^\dagger c_\uparrow = (1,0,0,0)^\dagger (1,0,0,0)\\
= \begin{bmatrix}
1 & 0&0 &0\\
0 &0& 0&0\\
0&0 & 0&0\\
0& 0&0 &0\\
\end{bmatrix}
$$

After a few similar calculations, H (without the first term $\epsilon$) becomes

$$
H = \begin{bmatrix}
\frac{\epsilon}{2} & & &\Delta\\
 &\frac{\epsilon}{2}& &\\
& &-\frac{\epsilon}{2}&\\
\Delta& & &-\frac{\epsilon}{2}\\
\end{bmatrix}
$$

Sovle the eigensystem with Mathematica ($t = \epsilon/2$):

$$
\left(
\begin{array}{cccc}
 -t & t & -\sqrt{\Delta ^2+t^2} & \sqrt{\Delta ^2+t^2} \\
 \{0,0,1,0\} & \{0,1,0,0\} & \left\{-\frac{\sqrt{\Delta ^2+t^2}-t}{\Delta },0,0,1\right\} & \left\{-\frac{-\sqrt{\Delta ^2+t^2}-t}{\Delta },0,0,1\right\} \\
\end{array}
\right)
$$

It can be interpreted as if there are two kinds of quasiparticles, one with energy $t$ and the other one with energy $\sqrt{\Delta^2+t^2}$. Why there are two kinds instead of one?