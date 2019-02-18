---
layout: post
---
I edit .md files on the GitHub website and GitHub converts those files into web pages (.html files) for me. Today my computer froze up when I was preparing this post. I restarted the computer. Everything went away since I didn't commit changes before the freezes. No autosaving :(

Let's get down to business. I forked a [project](https://github.com/cover-me/qtplot) from [qtplot](https://github.com/Rubenknex/qtplot). It is quite useful for visualizing/filtering/line-cutting 2d (or 1d) data. The program outputs figures using matplotlib as it's written by python. And there is an interesting problem.

Several functions are provided generating 2d figures in matplotlib, such as pcolormesh, pcolor, imshow, matshow, figimage... The first two are similar, so does the second two, for the last, I don't know so far. If the data, which consists of 3 2d-arrays X, Y, Z, are uniformly sampled from the x-y plane, we can just use imshow which presents each datapoint by a "pixel". Otherwise one needs pcolormesh, as pcolormesh draw polygon patches for each datapoint in the way below:

![](/images/pcolormesh_patch.png)

This is a snapshot captured [here](https://matplotlib.org/api/_as_gen/matplotlib.pyplot.pcolormesh.html)

Here is an example of pcolormesh:

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

Looks good. But if we save the figure into some vector graphic formats, such as svg or pdf, and open it in other programs:

```python
plt.pcolormesh(np.zeros((10,10)))#or 300,300
```

.svg file in Chrome, 10*10 array. There are white lines between patches, even though the space between them should be 0:

![](/images/snapshoot_chrome_pcolormesh_svg_10x10.png)

.svg file in IE, 300*300 array:

![](/images/snapshoot_ie_pcolormesh_svg_300x300.png)

.pdf file in chrome, 300*300 array, looks fine. While .pdf files look fine in Chrome or Adobe Reader, they are not good in AI or Inkscape. It depends on the kernel of the viewer:

![](/images/snapshoot_chrome_pdfviewer_pcolormesh_pdf_300x300.png)

.pdf file in chrome, using the previewer on overleaf.com, 300*300 array:

![](/images/snapshoot_chrome_overleaf_pcolormesh_pdf_300x300.png)

This problem has been discussed in many places [here's a link](https://stackoverflow.com/questions/27092991/white-lines-in-matplotlibs-pcolor). The reason for the white lines is that some (or many) programs aren't smart enough to show two polygons sharing the same edge. In addition to the white lines, another disadvantage of pcolormesh (pcolor) generated vector graphs is that when there are too many patches, it will be very slow to open it from other programs. One solution is to rasterize the figure so that it is based on pixels, but you may lose some information if the dpi is not high enough. imshow also resamples your data by default as discussed [here](https://github.com/matplotlib/matplotlib/issues/322). A trick is to set interpolation='none'. It works for pdf but doesn't work for svg ([ref.](https://matplotlib.org/gallery/images_contours_and_fields/interpolation_methods.html)). I prefer to save figures as .svg files for Inkscape (pdf is also fine...). Just as the old saying goes, you press the gourd from one side, then the other side bobs up. :(

