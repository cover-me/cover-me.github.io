---
layout: post
title: A prototype for data taking programs
---
As you can infer from the name, Condensed matter physicists study matters that condensed. The name doesn't really matter. Is there anything that never condenses? Even Helium condenses into the liquid at 4.2 K and solid at lower temperatures + higher pressures. Not all condensed matters are studied by condensed matter physicists. For example, the sun and stars are studied by astronomer physicists. Condensed matter physicists usually focus on matters that can be obtained on Earth and are small enough to put into a scientific machine for experiments.

One kind of those experiments is called electronic transport experiment. Electrons transport from one side to the other in a conductor if a voltage is applied, forming a current. Electronic transport measurements measure those currents and voltages, calculate resistance and conductance. 

The idea of electronic transport is very simple. But the physics is rich. Many effects are revealed by simply measuring resistance, such as the superconducting, Hall effect family, non-linear behavior of PN junctions, transistors, LED, Qubits...

In order to do the measurements, one need instruments applying or measuring signals, and a program to control these instruments. The connection between a computer running the program and the instruments may look like this:

![](https://github.com/cover-me/instrDAQ/raw/master/documentation/images/setup.jpg)

There are many kinds of connectors that can link a computer and an instrument. GPIB is widely used for its robustness and scalability. The serial connection is favored because of cheapness. Ethernet and USB are more and more popular due to their success in consumer electronics. Which connector to use doesn't really matter for a data acquisition program. The communication between instruments and computers is based on commands/APIs that don't relly on the choice of interfaces.

The first thing a data acquisition program needs to do is

>1 sending commands to/read response from instruments.

>> a. The program should at least be able to fetch the readings and set the outputs.
  
>> b. It is better if the program can also get/set other parameters such as the filtering, time constant... 
  
>> c. Easy to add support for new instruments.
  
>> d. The program is portable. It can be put into a USB stick and run on another computer. This is irrelevant to (a)-(c) but it is not difficult to achieve.

For most script languages, it is not hard to achieve the goals above. [qtlab](https://github.com/heeres/qtlab), which is a python based data acquisition program, has a folder for "drivers". Here, a driver is a script that has above features. If there is a new instrument, one can easily add a new driver (which is a text file of code) and put it into that folder.

LabVIEW can load VIs dynamically. Therefore one can also create a folder of "drivers" for LabVIEW programs and throw VI files in. I haven't tried this feature. The problem for LabVIEW is that not every computer has it installed, you need to purchase a license. It is painful if one is asked to add support for a new instrument while there is no LabVIEW on the computer. The portability is not a problem for LabVIEW because the program can be packed into an executable file. [instrDAQ](https://github.com/cover-me/instrDAQ) put instrument information into a text file so that there is no need to do more LabVIEW programming for new instruments. However, this program doesn't achieve goal 1.b.

Once we are able to set and read from instruments, we begin to worry about controlling the experiments. My experience told me, experiments are just scans, i.e. set one or a group of output channels in a for-loop while getting readings from instruments. Most scans are 1D or 2D scans, sometimes they are 3D scans. 1D and 2D scans can be treated as special 3D scans, with extra dimensions the length of 1. I don't find it necessary to bother myself with higher dimensions so far. 

There are continuous outputs and discrete outputs. Sometimes a magnetic field is a continuous output, the program triggers a scan and the output field keep ramping until it receives a "pause" message or reaches limits. The program keeps reading from instruments during the scan. Gate voltages and magnetic fields (again) on some other times are discrete outputs. During the scan, the program not only reads from instruments but also sets the outputs point by point, discretely. Whether an output should be a continuous one or a discrete one is determined by people who do the experiments, not the insturments.  

The following is the second feature  a data acquisition program needs to do:

>2 Build-in support for linear scans up to 2 dimensions, would be better if 3 dimensions or more. 1D and 2D scans are special cases of 3D scans. Even though the linear 3D scan doesn't cover all scans, it is very powerful and useful for most of the time.

>>a. Read data from a specific group of instruments during the scan.

>>b. Support discretely outputs. Also, support continuous outputs if necessary.

>>c. Each dimension of output can be a vector instead of a single channel. For example, make it possible to do scan(D1,D2,D3), where Di has the form `([channeli1, channeli2, channeli3, ...], [starti1, starti2, starti3, ...], [stopi1, stopi2, stopi3, ...], num_of_pointsi, delay_after_seti)`.

[instrDAQ](https://github.com/cover-me/instrDAQ) was first designed for 1D scans. Later a "sequence" feature and a "sequence from file" feature were added so 2D and higher dimensional scans are possible. For qtlab, I wrote a [script](https://github.com/cover-me/repository/tree/master/qt/qtlab%20scan%20scripts) so it is possible to do scans up to 3D like this:

```python
e.scan('g3','dac3',0,3,10,)#or e.scan(['g3'],['dac3'],[0],[3],10,)
e.scan(['g1','g2'],['dac1','dac2'],[0,0],[3,4],10,  ['g3'],['dac3'],[0],[3],10,  ['g5'],['dac5'],[0],[3],10)
```

The program should also show realtime figures, save data and some sort of log. It makes me feel a headache to choose a format for the data. I am interested in .npy files + metafiles + the file system provided by operating system. I don't feel excited about HDF5 especially after reading [this post](https://cyrille.rossant.net/moving-away-hdf5/). By the way, I am still using ASCII characters for storing data. Why bother it, even the most exciting curves, showing quantization of some combinations of physical constants, have only several hundred points.

>3 Save data, take notes and real-time visualization.

LabVIEW has some advantages when it comes to realtime visualization. It is a graphical programming language natively supporting parallel processing. However, the program [instrDAQ](https://github.com/cover-me/instrDAQ) was first designed for 1D scans, there are only 1D real-time curves. For python, you have to write the GUI yourself! Luckily, cool guys on GitHub shared [qtplot](https://github.com/Rubenknex/qtplot) and [qtlab](https://github.com/heeres/qtlab). After some modification (see my [fork of qtplot](https://github.com/cover-me/qtplot) and [scan script](https://github.com/cover-me/repository/tree/master/qt/qtlab%20scan%20scripts) ) data can be taken from qtlab while visualized with qtplot in realtime. Here are [demos](https://cover-me.github.io/2019/03/31/qtplot-demo.html). The math filters in qtplot are also very useful for 2D data analysis. The combination also makes note-taking easy. The scan script sends every scan commands to a Word file automatically while qtplot sends every figure to Word by clicking a button. The Word looks better for notes with the web layout and the navigation pane. For instrDAQ (I once called it Paramecium and drew the icon), it doesn't take notes in a document, but save screenshots of the front panel for every scan.

The prototype

```
data file, notes                
    ↑    ↘(1)                    
-----------------------------
| scan   →(2) visualization |
|   ↑                       |
| driver                    |
-----------------------------
    ↑
instruments
```

I prefer (2) as the dataflow for visualization. I think (1) should also be fine. But I am not clear about how efficient (1) is, as it involves the operating system and hard disks.



