---
layout: post
---
I edit the .md files on the webpage of github and GitHub Pages converts them into .html files automatically. Today may computer stucked and I restarted it. Then everything I wrote had gone since I didn't commit the change before the computer stucked and I can't do it after that :(

I have forked a [project](https://github.com/cover-me/qtplot) of [qtplot](https://github.com/Rubenknex/qtplot). It is quite useful for visualizing/filtering/linecuting 2d or 1d data. The program outputs figures using matplotlib as it's written by python. And there is a interesting problem.

There are several functions generating 2d figures in python, such as pcolormesh, pcolor, imshow, matshow... The first two are similar, so does the last two. If the data, which consists of 2d arrays X,Y,Z, are uniformly located, we can just use imshow that presents each datapint as a "pixle". Otherwise one needs pcolormesh, as pcolormesh draw polygen patches for each datapoint in the following way. 

![](/images/pcolormesh_patch.png)

This is a snapshoot captured [here](https://matplotlib.org/api/_as_gen/matplotlib.pyplot.pcolormesh.html)

An example of pcolormesh
```python
import matplotlib.pyplot as plt
import numpy as np
def get_quad(x):
    '''
    calculate the patch corners
    '''
    l0, l1 = x[:,[0]], x[:,[1]]
    r1, r0 = x[:,[-2]], x[:,[-1]]
    x = np.hstack((2*l0 - l1, x, 2*r0 - r1))
    t0, t1 = x[0], x[1]
    b1, b0 = x[-2], x[-1]
    x = np.vstack([2*t0 - t1, x, 2*b0 - b1])
    x = (x[:-1,:-1]+x[:-1,1:]+x[1:,:-1]+x[1:,1:])/4.  
    return x

x = np.array([[0.,1.],
            [0.2,1.3]])
y = np.array([[0.,0.3],
            [1,1.2]])
z = np.array([[0,1],
            [2,3]])
x1,y1 = get_quad(x),get_quad(y)

#plot the data
plt.pcolormesh(x1,y1,z)
#plot location of datapoint
plt.scatter(x.ravel(),y.ravel(),s=200,color='k')
#plot the patch corners
plt.scatter(x1.ravel(),y1.ravel(),s=200,marker='*',color='k')
plt.show()
```

![](/images/pcolormesh_example.png)

Looks good. But if we save the figure into some vector grahic formats, like svg, and open it in other programs.

```python
plt.pcolormesh(np.zeros((10,10)))
```

.svg file in IE, 300*300 array

![](/images/snapshoot_ie_pcolormesh_svg_300x300.png)

.svg file in Chrome, 10*10 array

![](/images/snapshoot_chrome_pcolormesh_svg_10x10.png)

.pdf file in chrome, 300*300 array, looks fine. While .pdf files looked fine in chrome or adobe reader, but not good in AI or inkscape

![](/images/snapshoot_chrome_pdfviewer_pcolormesh_pdf_300x300.png)

.pdf file in chrome, through a preview from overleaf.com, 300*300 array

![](/images/snapshoot_chrome_overleaf_pcolormesh_pdf_300x300.png)

This problem has been discussed for a long time [a link](https://stackoverflow.com/questions/27092991/white-lines-in-matplotlibs-pcolor). Some (or many) programs aren't smart engouh to show two polygons sharing a same edge. There are always white lines between them polygons. Another disatavanget of pcolormesh generated vector graphics is that when there are too many patches, it will be very slow to open it from other programs. One solution is to rasterize the figure so that it is based on pixles again, and you may lost information (imshow also resamples your data by default as descussed [here](https://github.com/matplotlib/matplotlib/issues/322)). Just as the old saying goes, you press the gourd from one side, then the other side bobs up. :(

