---
layout: post
---

[This](https://topocondmat.org/w1_topointro/0d.html) page gives a very helpful introduction to the relationship between topology and symmetry. Both topology and symmetry can be used for classification. A ball is symmetrically different from a cube and they can be classified into two different groups. In fact, a ball has more symmetry operations than a cube--one can rotate a ball by any degrees and the ball would still look the same, which is not the case for a cube. In the real world, a different symmetry means a different phase ([water and ice](http://www.lassp.cornell.edu/sethna/OrderParameters/BrokenSymmetry.html)). However, does a different phase always mean a different symmetry? [Here](https://physics.stackexchange.com/questions/105166/symmetry-breaking-and-phase-transition) is a discussion about it. I think the word 'symmetry' is not well defined. If two systems are different, one can always find an operation that keeps one unchanged and the other one changed (I can't prove this now...). By calling that 'operation' a symmetry operation the two systems are symmetrically different. In this scenario, every difference is a symmetry difference, it would be meaningless using symmetry for classification!

Particle-hole symmetry is discussed in the notebook mentioned above. Spectrums are calculated and they are symmetric with respect to $E = 0$. A puzzle comes up when I read that the parity of the system changes once two energy levels cross each other at zero. As you can image, for an "ordinary" spectrum the crossing does nothing to the particle number, let alone the parity. Why for this particular case the parity changes? The key is that the spectrum is calculated from the BdG Hamiltonian, which is an auxiliary Hamiltonian.

First, let's assume that there is no hopping or superconducting coupling terms. Consider the Hamiltonian $H = \sum_{i} h_{i} c_{i}^{\dagger} c_{i}$, which is equal to 

$$\frac{1}{2} \sum_i h_i (2c_i^\dagger c_i) = \frac{1}{2} \sum_i h_i (c_i^\dagger c_i - c_i c_i^\dagger + 1) = \frac{1}{2} (\sum_i h_i c_i^\dagger c_i + \sum_i (-h_i) c_i c_i^\dagger + \sum_i h_i)$$

Let $H_{BdG} = \sum_i h_i c_i^\dagger c_i + \sum_i (-h_i) c_i c_i^\dagger$ (1/2 and $\sum_i h_i$ in H are constants which can be thrown away). Both H and $H_{BdG}$ are diagonal matrixes.

$$H = \begin{pmatrix} h_1 &   &   & \\   & h_2 &   & \\   &   & ... &   \\   &   &   & h_n \end{pmatrix}$$



$$H_{BdG} = \begin{pmatrix}
  h_1 &   &   &  & & & & \\
    &  h_2 &   &  & & & &\\
    &   &  ...  &  & & & &\\
    &   &   & h_n & & & &\\
    &   &   &  & -h_1 & & &\\
    &   &   &  & & -h_2& &\\
    &   &   &  & & & ...&\\
    &   &   &  & & & & -h_n
\end{pmatrix}$$

Eigenvalues of $H_{BdG}$ are $(h_1,h_2,...,h_n,-h_1,-h_2,...,-h_n)$. It should be noted that $h_i$ may be negtive and $-h_i$ may be positive. While $h_1$, $h_2$, ..., $h_n$ are the eigenvalues of $H$, $-h_1$, $-h_2$, ..., $-h_n$ are irregular numbers, just something simililar to guidelines or image charges.


Now we turn to the case where there is hopping or superconducting coupling. What will be the difference? Since there is interaction now, $H$ can't be diagonalized in the above signle particle way. The Hilbert space we are intersted in changes from $n$ dimensional $\lvert1,0,0,...\rangle$,$\lvert0,1,0,...\rangle$,$\lvert0,0,1,...\rangle$ to $2^n$ dimennsional $\lvert\pm1,\pm1,\pm1,...\rangle$. But we still get an "unchanged" $H_{BdG}$. In fact, there exits a  linear transformation from $(c_1, c_2, ..., c_1^\dagger, c_2^\dagger, ...)$ to $(d_1, d_2, ..., d_1^\dagger, d_2^\dagger, ...)$, the latter also obey fermionic commutation relations and $H_{BdG} = \sum_i h'_i d_i^\dagger d_i + \sum_i (-h'_i) d_i d_i^\dagger$. We can always choose the transformation so that $h_i \geq 0$ for all $i$. If $\lvert G\rangle$ is the ground state with engergy $0$, then,

$$H_{BdG} (d_i^\dagger \lvert G\rangle) = 2 h'_i (d_i^\dagger \lvert G\rangle)$$

$$H_{BdG} (d_i \lvert G\rangle) = - 2 h'_i (d_i \lvert G\rangle)$$

$d_i \lvert G\rangle = 0$, otherwise $d_i \lvert G\rangle$ will be an eigenstate with engegy lower than the ground state, which is not right. Then $d_i^\dagger \lvert G\rangle \neq 0$, is an eigenstate with energy $h'_i$  (we dropped 1/2 when defined $H_{BdG}$, now we put it back). $d_i (d_i^\dagger \lvert G \rangle) = \lvert G \rangle$. We got the quasiparticles! It should be noted that transition from $\lvert G \rangle$ to $d_i^\dagger \lvert G \rangle$ does not necessary means a particle number change of $\pm 1$.

Since every state has an energy $\geq$ ground state, it looks like every quasi-particle should have an energy $\geq 0$. One may then ask what are those negative states in the spectrum of superconductors, are they just "mirror" states of positive ones? I don't think so. $d_i^\dagger \lvert G\rangle$ sometimes have less particles (not quasiparticles) than $\lvert G\rangle$, in that case, we say it is a quasihole. When we plot the spectrum of quasielectrons, that state stays below 0.

How to broke particle-hole symmetry?
