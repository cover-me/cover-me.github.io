---
layout: post
title: Josephson junctions and qubits
---

## SIS junction

![image](https://github.com/user-attachments/assets/8bd40dd1-a980-4e94-ab68-e64bdcda0f08)


The energy phase relation of a superconductor-insulator-superconductor (SIS) junction is

$$E = -E_J \cos \varphi$$

$$E_J = \frac{\hbar}{2 e} I_c,$$

![Untitled](https://github.com/user-attachments/assets/0fbefee3-7bbc-48ca-8ee3-26f414e1d621)


The current-phase relation can be deduced from the energy-phase relation, 

$$I({\varphi}) = \frac{2 e}{\hbar} \frac{\text{d} E}{\text{d} \varphi}$$

##  SNS or SCS junction

![image](https://github.com/user-attachments/assets/1269aafb-a145-4878-b136-74b182c6e1db)


For a superconductor-normal metal-superconductor (SNS) junction or a superconductor-constriction-superconductor (SIS) junction, the energy phase relations of the two Andreev bound states (ABSs) are (note: there are usually multiple modes in an SNS junction, here only a single mode is considered for simplicity)

$$E = \pm \Delta \sqrt{1-T \sin^2 (\varphi/2)},$$

where T is the transparency.

![Untitled-1](https://github.com/user-attachments/assets/3dd52366-56d7-497e-92e4-d95f018b60f7)

When $T \rightarrow 0$, 

$$E \rightarrow \pm \Delta (1-\frac{T}{2} \sin^2 (\varphi/2)) =  \pm \Delta (1-\frac{T}{4} (1-\cos \varphi)) =  \pm ( (1-\frac{T}{4})\Delta  + \frac{T\Delta }{4} \cos \varphi).$$

The ABS energies oscillate as $\cos \varphi$. The lower energy branch resembles the situation in SIS junctions.

When $T = 1$,

$$E = \pm \Delta \sqrt{1- \sin^2 (\varphi/2)} = \pm \Delta \cos (\varphi/2).$$

The _ABS_ energies oscillate as $\pm \cos (\varphi/2)$. As a Josephson junction usually stays in the ground state, which means the lower-energy ABS is occupied while the higher-energy state is unoccupied, the _junction_ energy does not oscillate as $-\cos (\varphi/2)$. However, the Landau-Zener transition could make an ABS stay in the same branch (with the parity of the system untouched) when the phase passes $\pi$ where the two branches degenerate, restoring the $-\cos (\varphi/2)$ features. 



##  S-dot-S junction

![image](https://github.com/user-attachments/assets/b93be917-adcd-4a5d-9e5b-aaf8d7bcbbd7)

ABS energies of a superconductor-dot-superconductor (S-dot-S) junction, if the charging energy in the dot is negligible, can be deduced using the following equation

$$ 2 E^2 \Gamma + \sqrt{\Delta^2 - E^2} (E^2 -\epsilon^2 -\Gamma^2) + \frac{4 \Delta^2 \Gamma_1 \Gamma_2 \sin^2 (\varphi/2)}{\sqrt{\Delta^2 - E^2}} = 0 $$

$$ \Gamma = \Gamma_1 + \Gamma_2$$




When $\Delta >> E$, $\Delta >> \Gamma$, i.e., the gap is large


$$ \Longrightarrow (2  \Gamma + \sqrt{\Delta^2 - E^2}) E^2 = \sqrt{\Delta^2 - E^2} (\epsilon^2 +\Gamma^2) - \frac{4 \Delta^2 \Gamma_1 \Gamma_2 \sin^2 (\varphi/2)}{\sqrt{\Delta^2 - E^2}}  $$

$$ \Longrightarrow E^2 = \frac{ \sqrt{\Delta^2 - E^2}}{2  \Gamma + \sqrt{\Delta^2 - E^2}} (\epsilon^2 +\Gamma^2) - \frac{4 \Delta^2 }{(2  \Gamma + \sqrt{\Delta^2 - E^2}) \sqrt{\Delta^2 - E^2} } \Gamma_1 \Gamma_2 \sin^2 (\varphi/2) $$

$$ \Longrightarrow E^2 \approx  \epsilon^2 +\Gamma^2 - 4 \Gamma_1 \Gamma_2 \sin^2 (\varphi/2) $$

$$ \Longrightarrow E \approx  \pm \sqrt{\epsilon^2 +\Gamma^2} \sqrt{1 -\frac{ 4 \Gamma_1 \Gamma_2}{\epsilon^2 +\Gamma^2} \sin^2 (\varphi/2)}, $$

Resembling the SNS situation with $\Delta = \sqrt{\epsilon^2 +\Gamma^2}$ and $T = 4 \Gamma_1 \Gamma_2/(\epsilon^2 + \Gamma^2)$. 

![image](https://github.com/user-attachments/assets/0898bb68-2a4f-4807-a57c-7c9e189dbee3)

When $\varphi = 0$  and $\Gamma_1 = \Gamma_2$ the model is reduced to the dot-superconductor situdation (with negligible charging energy) discussed in [this previous post](https://cover-me.github.io/2020/05/08/The-phase-diagram-of-a-single-dot-coupled-to-a-superconductor.html).

![image](https://github.com/user-attachments/assets/7cc6952e-bf20-4b29-a209-2c20eb221f10)


## Superconducting qubits: Junctions with a charging energy from a parallel capacitor

![image](https://github.com/user-attachments/assets/792948eb-e1da-487c-86a1-bc30212f9149)

### LC

First, we consider the simpler case in the subfigure (a), which consists of an inductor and a capacitor. The Hamiltonian can be written as

$$H = \frac{Q^2}{2C} + \frac{\Phi^2}{2L} $$

$$[Q,\Phi] = i\hbar$$

We normalize $Q$ and $\Phi$

$$n = \frac{1}{-2e} Q$$

$$\varphi = \frac{2 \pi}{\Phi_0}\Phi = \frac{2 e}{\hbar}\Phi$$

So

$$[n,\varphi] = \frac{1}{-2e} \frac{2 e}{\hbar} [Q,\Phi] = -i$$

$$H =  \frac{2 e^2 }{C}n^2 + \frac{\hbar^2}{8 e^2 L} \varphi^2$$

This Hamiltonian is the same as that of a harmonic oscillator, let

$$H =  A n^2 + B \varphi^2$$

$$a = \frac{\sqrt{A} n - i \sqrt{B} \varphi}{\sqrt{2\sqrt{AB}}}$$

$$a^\dagger =  \frac{\sqrt{A} n + i \sqrt{B} \varphi}{\sqrt{2\sqrt{AB}}}$$

Then

$$[a,a^\dagger] = \frac{1}{2\sqrt{AB}}[\sqrt{A} n - i \sqrt{B} \varphi,  \sqrt{A} n + i \sqrt{B} \varphi]$$

$$= \frac{1}{2\sqrt{AB}}([\sqrt{A} n,   i \sqrt{B} \varphi] + [-i \sqrt{B} \varphi,  \sqrt{A} n )$$

$$ =\frac{1}{2\sqrt{AB}} i \sqrt{A B}([ n, \varphi ] - [\varphi ,  n])$$

$$ =\frac{1}{2\sqrt{AB}} i \sqrt{A B}(-2 i)$$

$$ =1$$

$$a^\dagger a =  \frac{1}{2\sqrt{AB}}(A n^2 + B \varphi^2 + i\sqrt{AB} [\varphi, n])$$

$$=  \frac{1}{2\sqrt{AB}}(A n^2 + B \varphi^2 - \sqrt{AB}) $$

$$=  \frac{1}{2\sqrt{AB}}(A n^2 + B \varphi^2) - \frac{1}{2} $$

$$H = 2 \sqrt{AB} (a^\dagger a + \frac{1}{2}) = 2 \sqrt{\frac{2 e^2 }{C}\frac{\hbar^2}{8 e^2 L}} (a^\dagger a + \frac{1}{2}) =  \frac{1}{\sqrt{LC}}\hbar (a^\dagger a + \frac{1}{2})$$

The energy levels are evenly spaced by $\hbar \omega$, where $\omega = 1/\sqrt{LC}$. $A$ and $B$ are sometimes defined as $4 E_C$ and $E_L/2$ in literature.

### JC

Next, we replace the inductor with Josephson junctions, depicted in subfigure (b). Note that multiple parallel Josephson junctions, such as a SQUID, are analog to single JJs with extra tunability enabled by external fluxes. The Hamiltonian now becomes

$$H =  \frac{2 e^2 }{C}n^2 + E(\varphi),$$

Where $E(\varphi)$ is the energy of Josephson junctions discussed in previous sections.

For a $\cos \varphi$ potential,

$$H =  \frac{2 e^2 }{C}n^2 + E_J cos(\varphi)$$

$$ \approx  \frac{2 e^2 }{C}n^2 - E_J (1 - \frac{1}{2}\varphi^2+ \frac{1}{24}\varphi^4)$$

$$= \frac{2 e^2 }{C}n^2 + \frac{E_J}{2}\varphi^2- \frac{E_J}{24}\varphi^4 + \text{const}$$

Let

$$H =  A n^2 + B \varphi^2+ D\varphi^4 = H_0 + H_1$$

Where $H_0 = A n^2 + B \varphi^2$ has been dicussed above, $H_1 = D \varphi^4$. We treat $H_1$ as a perturbation and calculate the first-order energy shift of the n-th state $\left| n \right>$

$$E^1_n = \left< n \left| H_1 \right| n \right>$$

With

$$n = \frac{\sqrt{2\sqrt{AB}}}{2  \sqrt{A}}(a^\dagger+a)$$

$$\varphi = \frac{\sqrt{2\sqrt{AB}}}{2 i \sqrt{B}}(a^\dagger-a),$$

we have

$$H_1 = D\varphi^4 = D( \frac{\sqrt{2\sqrt{AB}}}{2 i \sqrt{B}}(a^\dagger-a))^4 = \frac{AD}{4B}(a^\dagger-a)^4$$

$$ = \frac{AD}{4B} \sum_{\sigma_1,\sigma_2,\sigma_3,\sigma_4 \in \{I,\dagger\}} (-1)^{sign(\sigma_1,\sigma_2,\sigma_3,\sigma_4)} a^{\sigma_1}a^{\sigma_2}a^{\sigma_3}a^{\sigma_4}$$

For $E^1_n = \left< n \right| H_1 \left| n \right>$ to be nonzero, we only need to consider terms with the same number of raising and lowering operators

$$H_1 \approx  \frac{AD}{4B} (a^\dagger a ^\dagger a a + a^\dagger a a^\dagger a + a^\dagger a a a^\dagger + a a^\dagger a^\dagger a + a a^\dagger a a^\dagger + a a a^\dagger a^\dagger ) $$

$$= \frac{AD}{4B} (a^\dagger a ^\dagger a a + a^\dagger a a^\dagger a + (a^\dagger a a^\dagger a + a^\dagger a) + a a^\dagger a^\dagger a + a a^\dagger a a^\dagger + (a  a^\dagger a a^\dagger + a a^\dagger))$$

$$= \frac{AD}{4B} (a^\dagger a ^\dagger a a + 2 a^\dagger a a^\dagger a + a^\dagger a + a a^\dagger a^\dagger a + 2 a a^\dagger a a^\dagger + a a^\dagger)$$

$$= \frac{AD}{4B} ((a^\dagger a a ^\dagger a - a^\dagger a) + 2 a^\dagger a a^\dagger a + a^\dagger a + a a^\dagger a^\dagger a + 2 (a a^\dagger a^\dagger a + a a^\dagger) + a a^\dagger)$$

$$= \frac{3 AD}{4B} ( a^\dagger a a^\dagger a +  a a^\dagger a^\dagger a + a a^\dagger)$$

$$= \frac{3 AD}{4B} ( a^\dagger a a^\dagger a +  (a^\dagger a a^\dagger a + a^\dagger a) + a a^\dagger)$$

$$= \frac{3 AD}{4B} ( 2 a^\dagger a a^\dagger a  + a^\dagger a + a a^\dagger)$$

$$= \frac{3 AD}{4B} ( 2 a^\dagger a a^\dagger a  + 2 a^\dagger a + 1)$$

So 

$$E_n^1 = \left< n \right| H_1 \left| n \right> = \frac{3 AD}{4B}(2n^2+2n+1)$$

$$E_{n,n-1}  =  \frac{3 AD}{B} n + \hbar \omega$$

$$E_{n, n-1} - E_{n-1, n-2}   =  \frac{3 AD}{B}$$

$3 AD/B = - A/4 = -e^2/2C$ for $\cos \varphi$, $-e^2/8C$ for $\cos \varphi/2$.

![Untitled](https://github.com/user-attachments/assets/fc524f75-786d-4b6f-b7a1-a200b7cfc437)

### JCG

The system in subfigure (c) can be described as

$$H =  A (n-n_g)^2 + V(\varphi)$$

The $n_g$ term is usually introduced by coupling to the environment. So it would be nice if the energy spectrum is insensitive to $n_g$.















