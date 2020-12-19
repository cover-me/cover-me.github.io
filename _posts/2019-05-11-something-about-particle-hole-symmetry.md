---
layout: post
title: Something about the particle-hole symmetry in BdG Hamiltonian
---
This [page](https://topocondmat.org/w1_topointro/0d.html) provides a very helpful introduction to the relationship between the topology and the symmetry. Both topology and symmetry can be used for classification. For example, a ball is _symmetrically_ different from a cube so they can be classified into two different groups. In fact, a ball has more symmetry operations than a cube--one can rotate a ball by any degrees and get the same shape, which is not the case for a cube. In the real world, a different symmetry means a different phase ([water and ice](http://www.lassp.cornell.edu/sethna/OrderParameters/BrokenSymmetry.html)). However, does a different phase always mean a different symmetry? There is a [discussion](https://physics.stackexchange.com/questions/105166/symmetry-breaking-and-phase-transition) about it. I think the word 'symmetry' is not well defined. If two systems are different, one can always find an operation that keeps one unchanged and the other one changed (I can't prove this now...). By calling that 'operation' a symmetric operation the two systems are symmetrically different. In this scenario, every difference is a symmetric difference, it would be meaningless using symmetry for classification!

Particle-hole symmetry is discussed in the notebook mentioned at the beginning of this post. In the case of a particle-hole symmetry, energy spectrums are symmetric with respect to $E = 0$. Confusion arises when I read that the parity of the system changes once two energy levels cross each other at zero. Because for an "ordinary" spectrum, the crossing does nothing to the particle number because the total number of states below zero doesn't change, let alone the parity! Why in this notebook the parity changes? The answer is that the spectrum is calculated with the BdG Hamiltonian, which is a Hamiltonian with auxiliary states. Those states below the zero energy are merely a mirror of those above zero.

First, let's assume that there is no hopping or superconducting coupling. The Hamiltonian $H = \sum_{i} h_{i} c_{i}^{\dagger} c_{i}$, equals to 

$$\frac{1}{2} \sum_i h_i (2c_i^\dagger c_i) = \frac{1}{2} \sum_i h_i (c_i^\dagger c_i - c_i c_i^\dagger + 1) = \frac{1}{2} (\sum_i h_i c_i^\dagger c_i + \sum_i (-h_i) c_i c_i^\dagger + \sum_i h_i)$$

Let $H_{BdG} = \sum_i h_i c_i^\dagger c_i + \sum_i (-h_i) c_i c_i^\dagger$ (1/2 and $\sum_i h_i$ in H are constants which can be thrown away). Translate them in to the form of matrices. Both of H and $H_{BdG}$ are diagonal.

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

Eigenvalues of $H_{BdG}$ are $(h_1,h_2,...,h_n,-h_1,-h_2,...,-h_n)$. It should be noted that $h_i$ may be negtive and $-h_i$ may be positive. While ($$h_1$$, $h_2$, ..., $$h_n$$) are the eigenvalues of $H$, ($$-h_1$$, $-h_2$, ..., $$-h_n$$) are auxiliary numbers, just something simililar to guidelines or image charges.


Now we turn to the scenario where there is hopping or superconducting coupling. What will be the difference? With general interactions we need to consider the $2^n$-dimensional many-body/Hilbert/Fock space (the same thing with multiple names...): ${\lvert\pm1,\pm1,\pm1,...\rangle}$. With a non-zero hopping term, we are still able to sovle the problem in a single-partilcle picture. This is becasue hopping only mixes states with same particle numbers, i.e., single-particle eigenstates of the system are still in the N-dimensional space spanned by single-particles basis states: $\lvert1,0,0,...\rangle$,$\lvert0,1,0,...\rangle$,$\lvert0,0,1,...\rangle$. We can simply write down the Hamiltonian in that subspace and diagnolize it. A non-zero superconducting term couples states with different particle numbers (but same parity), which indicates that we had better to take many-body states into account. This [post](https://cover-me.github.io/2020/05/08/The-phase-diagram-of-a-single-dot-coupled-to-a-superconductor.html) shows how to find eigensteates of a dot with superconductivity in the $2^n$ dimensional many-body space $${\lvert 00⟩,\lvert 01⟩,\lvert 10⟩,\lvert 11⟩}$$. Interestingly, with the magic of 2N-dimensional BdG Hamiltonian, we are still able to understand the system in a "single-quasiparticle" picture. In fact, there exits a linear transformation from $(c_1, c_2, ..., c_1^\dagger, c_2^\dagger, ...)$ to $(d_1, d_2, ..., d_1^\dagger, d_2^\dagger, ...)$, the latter also follow fermionic commutation relations and $H_{BdG} = \sum_i h'_i d_i^\dagger d_i + \sum_i (-h'_i) d_i d_i^\dagger$. We can always choose the transformation so that $h_i \geq 0$ for all $i$. If $\lvert G\rangle$ is the ground state with engergy $0$, we have

$$H_{BdG} (d_i^\dagger \lvert G\rangle) = 2 h'_i (d_i^\dagger \lvert G\rangle)$$

$$H_{BdG} (d_i \lvert G\rangle) = - 2 h'_i (d_i \lvert G\rangle)$$

$d_i \lvert G\rangle$ should be 0 (when $$h'_i \neq 0$$), otherwise $d_i \lvert G\rangle$ will be an eigenstate whose energy ($$-2 h'_i$$) is lower than the ground state, violating the definition that a _ground state_ has the lowest energy. $d_i^\dagger \lvert G\rangle \neq 0$ (otherwise $$0 = d_i (d_i^\dagger \lvert G \rangle) = \lvert G \rangle$$), is an eigenstate with energy $h'_i$ .(we dropped 1/2 when defining $$H_{BdG}$$, now we put it back). $d_i (d_i^\dagger \lvert G \rangle) = \lvert G \rangle$. We got the quasiparticles! In this picture of quasiparticles, negative engery 'states' are auxiliary. It should be noted that a transition from $\lvert G \rangle$ to $d_i^\dagger \lvert G \rangle$ does not necessary means a particle number change of $\pm 1$.

Since every state has an energy $\geq$ ground state energy, it looks like every quasi-particle should have an energy $\geq 0$. One may then ask what are those negative states in the spectrum of superconducting dots, are they always "mirror" states of positive ones? Mathematically yes, but I would rather think in another way. $d_i^\dagger \lvert G\rangle$ sometimes have less _particles_ (not _quasiparticles_) than $\lvert G\rangle$, in that case, we say it is a quasihole. When we plot the spectrum of quasielectrons, that state is below 0.

So, what is the particle-hole symmetry? How to broke it?
