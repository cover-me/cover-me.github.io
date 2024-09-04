---
layout: post
title: Josephson junctions and Andreev bound states
---

# Superconductor-insulator-superconductor junction

![image](https://github.com/user-attachments/assets/8bd40dd1-a980-4e94-ab68-e64bdcda0f08)



The energy phase relation of a superconductor-insulator-superconductor (SIS) junction is

$$E = -E_J \cos \varphi$$

$$E_J = \frac{\Phi_0}{2 \pi} I_c,$$

The current-phase relation can be deduced from the energy-phase relation, 

$$I({\varphi}) = \frac{2\pi}{\Phi_0} \frac{\text{d} E}{\text{d} \varphi}$$

#  Superconductor-normal metal-superconductor or superconductor-constriction-superconductor junction

![image](https://github.com/user-attachments/assets/1269aafb-a145-4878-b136-74b182c6e1db)


For a superconductor-normal metal-superconductor (SNS) junction or a superconductor-constriction-superconductor (SIS) junction, the energy phase relations of the two Andreev bound states (ABSs) are

$$E = \pm \Delta \sqrt{1-T \sin^2 (\varphi/2)},$$

where T is the transparency.

When $T \rightarrow 0$, 

$$E \rightarrow \pm \Delta (1-\frac{T}{2} \sin^2 (\varphi/2)) =  \pm \Delta (1-\frac{T}{4} (1-\cos \varphi)) =  \pm ( (1-\frac{T}{4})\Delta  + \frac{T\Delta }{4} \cos \varphi).$$

The ABS energies oscillate as $\cos \varphi$. The lower energy branch resembles the situation in SIS junctions.

When $T = 1$,

$$E = \pm \Delta \sqrt{1- \sin^2 (\varphi/2)} = \pm \Delta \cos (\varphi/2).$$

The energies oscillate as $\cos (\varphi/2)$. The Landau-Zener transition could make an ABS stay in the same branch (with the parity of the system untouched) when the phase passes $\pi$ where the two branches degenerate. 

#  Superconductor-dot-superconductor junction

![image](https://github.com/user-attachments/assets/b93be917-adcd-4a5d-9e5b-aaf8d7bcbbd7)

ABS energies of a superconductor-dot-superconductor (S-dot-S) junction can be deduced with the following equation

$$ 2 E^2 \Gamma + \sqrt{\Delta^2 - E^2} (E^2 -\epsilon^2 -\Gamma^2) + \frac{4 \Delta^2 \Gamma_1 \Gamma_2 \sin^2 (\varphi/2)}{\sqrt{\Delta^2 - E^2}} = 0 $$

$$ \Gamma = \Gamma_1 + \Gamma_2$$




When $\Delta >> E$, $\Delta >> \Gamma$, i.e., the gap is large


$$ \Longrightarrow (2  \Gamma + \sqrt{\Delta^2 - E^2}) E^2 = \sqrt{\Delta^2 - E^2} (\epsilon^2 +\Gamma^2) - \frac{4 \Delta^2 \Gamma_1 \Gamma_2 \sin^2 (\varphi/2)}{\sqrt{\Delta^2 - E^2}}  $$

$$ \Longrightarrow E^2 = \frac{ \sqrt{\Delta^2 - E^2}}{2  \Gamma + \sqrt{\Delta^2 - E^2}} (\epsilon^2 +\Gamma^2) - \frac{4 \Delta^2 }{(2  \Gamma + \sqrt{\Delta^2 - E^2}) \sqrt{\Delta^2 - E^2} } \Gamma_1 \Gamma_2 \sin^2 (\varphi/2) $$

$$ \Longrightarrow E^2 \approx  \epsilon^2 +\Gamma^2 - 4 \Gamma_1 \Gamma_2 \sin^2 (\varphi/2) $$

$$ \Longrightarrow E \approx  \pm \sqrt{\epsilon^2 +\Gamma^2} \sqrt{1 -\frac{ 4 \Gamma_1 \Gamma_2}{\epsilon^2 +\Gamma^2} \sin^2 (\varphi/2)}, $$

Resembling the SNS situation with $\Delta = \sqrt{\epsilon^2 +\Gamma^2}$ and $T = 4 \Gamma_1 \Gamma_2/(\epsilon^2 + \Gamma^2)$. When $\varphi = 0$  and $\Gamma_1 = \Gamma_2$ the model is reduced to the dot-superconductor situdation discussed in [this previous post](https://cover-me.github.io/2020/05/08/The-phase-diagram-of-a-single-dot-coupled-to-a-superconductor.html).

![image](https://github.com/user-attachments/assets/7cc6952e-bf20-4b29-a209-2c20eb221f10)

