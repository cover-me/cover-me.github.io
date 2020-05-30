---
layout: post
title: The Majorana phase boundary in nanowires
---

According to Ref [1], 

$$\mathcal{H}=(p^2/2m-\mu(y))\tau_z + u(y) p \sigma_z \tau_z + B(y) \sigma_x + \Delta(y) \tau_x$$

$\sigma$ and $\tau$  operate in spin and particle-hole space, respectively.

Let $\xi = p^2/2m-\mu(y)$, with the help of Mathematica, we get,

$$E = \pm \sqrt{\xi^2 + (u p)^2 + B^2 +\Delta^2 \pm 2\sqrt{\xi^2 u^2 p^2 + \xi^2 B^2 + B^2 \Delta^2}}$$

Note that $up$ is $u*p$ instead of the word "up". There are both $u$ and $\mu$ in the Hamiltonian, dont' mix them up.

Ref [1] said the spectrum is "conveniently obtained by squaring the Hamiltonian twice", so let's do it. 

Square the Hamiltonian, use the formular $(A \otimes B)(C \otimes D)=(AC)\otimes(BD)$

$$\mathcal{H}^2 = \xi^2 + (u p)^2 + B^2 +\Delta^2 + 2\xi u p\sigma_z\tau_0 + 2\xi B \sigma_x\tau_z + 2 B \Delta \sigma_x\tau_x$$

Let $a = \xi^2 + (u p)^2 + B^2 +\Delta^2$, $b = 2\xi u p$, $c = 2\xi B$, $d = 2 B \Delta$, square the equation again.

$$
\mathcal{H}^4 
= (a + b\sigma_z\tau_0 + c\sigma_x\tau_z + d\sigma_x\tau_x)^2\\
= a^2+b^2+c^2+d^2+2a(b\sigma_z\tau_0 + c\sigma_x\tau_z + d\sigma_x\tau_x)\\
$$

Let it be $x+y\sigma_z \tau_0+\sigma_x Z$, to find eigenvalues

$$
0 = 
\left|
\begin{matrix} 
x+y-\lambda & Z\\
Z & x-y-\lambda\\
\end{matrix}
\right|
=\\
\left|(x-\lambda+y)(x-\lambda-y)-Z^2\right|\\
=((x-\lambda)^2-y^2-2a^2(c^2+d^2))^2
$$

$$
\Rightarrow \lambda = x\pm\sqrt{y^2+2a^2(c^2+d^2)}\\
= (a^2+b^2+c^2+d^2)\pm \sqrt{(2ab)^2+2a(c^2+d^2)}\\
= a^2+(b^2+c^2+d^2) \pm 2a \sqrt{b^2+c^2+d^2}\\
= (a+\sqrt{b^2+c^2+d^2})^2
$$

The formula of determinants of block matrices is used [2]. (Oops! Maybe I don't have to square $\mathcal{H}$ at all to apply this formula :P)

Let's now look at the spectrum $E$ to see when the energy gap is zero. That is

$$
0 = E = \pm \sqrt{\xi^2 + (u p)^2 + B^2 +\Delta^2 \pm 2\sqrt{\xi^2 u^2 p^2 + \xi^2 B^2 + B^2 \Delta^2}}\\
\Rightarrow
0 = \xi^2 + (u p)^2 + B^2 +\Delta^2 \pm 2\sqrt{\xi^2 u^2 p^2 + \xi^2 B^2 + B^2 \Delta^2}\\
\Rightarrow
0 = a + b + c +d \pm 2\sqrt{a b + a c + c d}\\
\\
\Rightarrow
0 = a + b + c +d - 2\sqrt{a b + a c + c d}\\
\Rightarrow
a + b + c +d = 2\sqrt{a b + a c + c d}\\
\Rightarrow
(a + b + c +d)^2 = 4(a b + a c + c d)\\
\Rightarrow
(a + b + c +d)^2 - 4(a b + a c + c d) = 0
$$

Where $a=\xi^2$, $b=(u p)^2$, $c = B^2$, $d=\Delta^2$. $a,b,c,d \geq 0$.

If $a \geq c$, 

$$(a + b + c +d)^2 - 4(a b + a c + c d)\\
=(-a + b + c +d)^2 + 4 (a - c) d \geq 0\\
$$

else,

$$
(a + b + c +d)^2 - 4(a b + a c + c d)\\
=(a + b - c +d)^2 + 4 (c - a) b \geq 0
$$

So the solution for $(a + b + c +d)^2 - 4(a b + a c + c d) = 0$ is 

$$
a-c=b+d=0\Rightarrow \left|B\right|=\left|\xi\right|, up=0, \Delta=0\\
or\\
-a+b+c=d=0\Rightarrow \Delta=0, \left|B\right|=\sqrt{\xi^2-(u p)^2}\\
or\\
a-c+d=b=0 \Rightarrow u p = 0, \left|B\right|=\sqrt{\xi^2+\Delta^2}
$$

If $u \neq 0$, $\Delta \neq 0$,the only solution for a zero gap is $p = 0$ and $| B |=\sqrt{\mu^2+\Delta^2}$, which happens to be the topological phase boudary for Majorana in a nanowire.

```
[1] PhysRevLett.105.177002
[2] http://djalil.chafai.net/blog/2012/10/14/determinant-of-block-matrices/
```
