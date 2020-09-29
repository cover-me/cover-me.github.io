---
layout: post
title: "Fight against EMI 1: Silicon capacitors"
---

![](/images/EMI%20and%20filters_0.png)

![](/images/EMI%20and%20filters.png)

![](/images/EMI%20and%20filters_1.png)

In short, one needs filters in real-world sensitive measurements. Even if there is an excellent shielding outside the device, the cables, and the instruments, there will still be problems caused by power cables (who go through the shielding), microprocessors, crystal oscillators, digital buses, magnetic fields, triboelectric/piezoelectric effect (vibrations), thermoelectric effect, cosmic rays, and so on. Ideal cap acts as a good conductor (insulator) for high-frequency (DC/low-frequency) signals, like a smart valve. Ironically, common caps are not ideal caps above GHz. Even if caps are ideal, the impedance due to inductance in transmission lines would increase significantly for high-frequency signals, making filters with caps less efficient in that regime. 

There are usually multiple-stage filters in a system. Among them is the RC filter. The name indicates that it consists of Resistors and Capacitors. For quantum transport measurements operating at ultra-low temperatures, many resistors and capacitors would fail. Ref. 1 provides part numbers of reses and caps that work well in a dilution fridge, on P. 153. Ref. 2 compares different kinds of resistors and caps against the temperature. Reses with similar datasheets may behave differently at low temperatures. One of the resistors I tested has its resistance almost unchanged at 77K but increased by a factor of 3 at mK. I'm not sure whether it is due to the thin layer of "high-reliability metal film" (Kondo effect) or the electrode (heterojunction). Or may it is caused by stress? Anyway, you don't want a temperature-sensitive resistor in series with your state-of-the-art device at fragile low temperatures.

Caps change capacitance or even lose capacitance at low temperatures because of the freezing of ferroelectric dielectric materials [Ref. 2]. NP0 (negative-positive zero) caps are caps with nearly 0 capacitance variation against temperatures. The dielectric layer is mainly based on barium neodymium titanate (BNT) in the past and CaZrO3-based materials nowadays [Ref. 3]. A quite search shows the dielectric constant of CaZrO3 is 23 to 32. They do good jobs in dilution fridges, even though with relatively low dielectric constants (thus larger sizes) comparing to other "bad" caps (X7R: 2000-4000, Y5V:	>16000..., Ref. 6).

NP0 caps are good folks. But the silicon cap is a rising star. The silicon cap is expensive, delicate (people in the electronics shop would complain about picking up and soldering it, what's more, they may stop working at high voltages), stable, and high-performance (especially at high frequencies). They are more ideal than traditional ceramic caps. The dielectric is SiOx (epsilon=1.45) or silicon oxynitride (1.45<epsilon<2), so k is not so high, but Q is high. The size can be very small because it utilized technologies from the semiconductor industry. Introductions of silicon caps can be found in Ref. 4 (2D version, thin-film caps) and Ref. 5 (3D version, pillar caps). Note: A large capacitance in a small size leads to a small breakdown voltage. 

There are even more crazy caps that utilizes Al2O3, HfO2 and carbon nano-fiber (Ref. 7).

References
1. https://web.stanford.edu/group/GGG/theses/Andrew%20Bestwick%20-%20Quantum%20Edge%20Transport%20in%20Topological%20Insulators.pdf
2. https://aip.scitation.org/doi/pdf/10.1063/1.5004484
3. https://nepp.nasa.gov/files/24320/Liu_2013_Pt2Capacitors.pdf
4. https://www.avx.com/products/rfmicrowave/capacitors/accu-p/
5. https://www.murata.com/en-us/products/capacitor/siliconcapacitors
6. https://johansondielectrics.com/basics-of-ceramic-chip-capacitors
7. https://epci.eu/silicon-and-silicon-wafer-based-integrated-capacitors/
