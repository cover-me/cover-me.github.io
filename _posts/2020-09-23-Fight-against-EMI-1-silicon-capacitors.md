---
layout: post
title: "Fight against EMI 1: Silicon capacitors"
---

![](/images/EMI%20and%20filters_0.png)

![](/images/EMI%20and%20filters.png)

![](/images/EMI%20and%20filters_1.png)

In short, one needs filters in real-world sensitive measurements. Even if there is an excellent shielding outside the device, the cables, and the instruments, there will still be problems caused by power cables (who go through the shielding), microprocessors, crystal oscillators, digital buses, magnetic fields, triboelectric/piezoelectric effect (vibrations), thermoelectric effect, cosmic rays, and so on. Ideal cap act as a good conductor for high-frequency signals,  diverting those signals to the shield (skin effect). Ironically, common caps are not ideal caps above GHz.

There are usually multiple-stage filters in a system. Among them is the RC filter. The name indicates that it consists of Resistors and Capacitors. For quantum transport measurements operating at ultra-low temperatures, many resistors and capacitors would fail. Ref. 1 provides part numbers of reses and caps that work well in a dilution fridge, on P. 153. Ref. 2 compares different kinds of resistors and caps against the temperature. Reses with similar datasheets may behave differently at low temperatures. One of the resistors I tested has its resistance almost unchanged at 77K but increased by a factor of 3 at mK. I'm not sure whether it is due to the thin layer of "high-reliability metal film" (Kondo effect) or the electrode (heterojunction). Or may it is caused by stress? Anyway, you don't want a temperature-sensitive resistor in series with your state-of-the-art device at fragile low temperatures.

Caps change capacitance or even lose capacitance at low temperatures because of the freezing of ferroelectric dielectric materials [Ref. 2]. NP0 (negative-positive zero) caps are caps with nearly 0 capacitance variation against temperatures. The dielectric layer is mainly based on barium neodymium titanate (BNT) in the past and CaZrO3-based materials nowadays [Ref. 3]. They do good jobs in dilution fridges, even though with relatively low dielectric constants (thus larger sizes) comparing to other "bad" caps.

They are good. But the silicon cap is the rising star.

The silicon cap is expensive, delicate (people in the electronics shop would complain about picking up and soldering it), stable, and high-performance (especially at high frequencies). They are more ideal than traditional ceramic caps. I think the dielectric is SiOx or something similar, so k is not so high, but Q is high. The size can be very small because it utilized technologies from the semiconductor industry. Introductions of silicon caps can be found in Ref. 4 (2D version, thin-film caps) and Ref. 5 (3D version, pillar caps)

References
1. https://web.stanford.edu/group/GGG/theses/Andrew%20Bestwick%20-%20Quantum%20Edge%20Transport%20in%20Topological%20Insulators.pdf
2. https://aip.scitation.org/doi/pdf/10.1063/1.5004484
3. https://nepp.nasa.gov/files/24320/Liu_2013_Pt2Capacitors.pdf
4. https://www.avx.com/products/rfmicrowave/capacitors/accu-p/
5. https://www.murata.com/en-us/products/capacitor/siliconcapacitors
