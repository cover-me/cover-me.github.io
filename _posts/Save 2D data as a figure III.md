---
layout: post
title: Save 2D data as a figure III
---
The subtitle: Interpolation, Shading, and SVG in Matplotlib.

SVG output is a little problematic in Matplotlib. However, when I use Matplotlib in Jupyter notebooks, neither PDF nor PNG is perfect. Browsers don't treat PDF figures as figures. Text in PNG is blurred due to low DPI settings. SVG sounds a better choice for Jupyter notebook. The ipympl backend may be an even better choice in the future, it also enables the ability of interaction. But at this moment, images can't be saved to the notebook if ipympl backend is used (issue [#16](https://github.com/matplotlib/ipympl/issues/16)). Many basic widgets just don't show up in mainstream Jupyter notebook viewers (even Jupyter's nbviewer). That is very uncool.

As mentioned in the old posts with the same title, SVG has at least two problems. 1) Viewers offer unwanted interpolation for scaled-up pixels, 2) the anti-aliasing issue between two adjacent objects, which creates white lines that shouldn't exist. The former issue can be solved by adding `attrib['style'] = 'image-rendering:pixelated'` in this [line](https://github.com/cover-me/matplotlib/blob/svg_image_avoid_interpolation/lib/matplotlib/backends/backend_svg.py#L877). `attrib['preserveAspectRatio'] = 'none'` is no longer needed as the Matplotlib SVG backend now sets the width and height in \<image\> tag  to the number of pixels and use a translation matrix to scale the \<image\> up. The width and height are set directly to corresponding physical values before, in that case, a pixel can not be stretched to a rectangle without `attrib['preserveAspectRatio'] = 'none'`. The latter is an issue due to viewers (Chrome), though the same viewers (Chrome) can deal with the same issue much better for PDF files.

Recently I found another interesting issue for SVG: doing Gouraud shading in SVG files. For Matplotlib's functions like `pcolormesh`, there is a parameter called `shading`. Its value can be either `flat` or `gouraud`. `gouraud` doesn't work for SVG files at this moment. Shading is the vector graphic version of bitmap's interpolation. While pixels in bitmaps align in a rectangular grid, vertices in vector graphics form any polygons. 

The flat shading simply fills a polygon with a constant color. The Gouraud shading does the bilinear interpolation for colors inside a trapezoid ([ref](http://medialab.di.unipi.it/web/IUM/Waterloo/node84.html)). When it comes to a triangle, the Gouraud shading is simply a linear interpolation.

How to apply the Gouraud shading to a triangular-meshed SVG image? Each point on the XY plane has a color. Each color has 4 channels, red, green, blue and alpha (transparency). For each channel, say channel c, there is a linear interpolation on the XY plane that c(x,y)=a1+a2*x+a3*y. We can get the coefficients:

```python
a1,a2,a3 = np.dot(np.linalg.inv([[1,x1,y1],[1,x2,y2],[1,x3,y3]]),[c1,c2,c3])
```

For SVG, linear interpolations (red channel, for example) is done by 

```xml
<linearGradient x1="x1" y1="y1" x2="x2" y2="y2">
    <stop offset="0"  stop-color="#000" />
    <stop offset="1" stop-color="#f00" />
</linearGradient>
```

We need to find (x1,y1) and (x2,y2) where c(x1,y1)=0, c(x2,y2)=1 (requires c not a constant) and (x1-x2,y1-y2) is parrallel to the projection of a1+a2*x+a3*y's normal vector on the c=0 plane. We can simply assume (x1,y1)=t0*(a2,a3), (x2,y2)=t1*(a2,a3),

```python
x1,y1 = (-a1/(a2*a2+a3*a3))*np.array([a2,a3])
x2,y2 = ((1-a1)/(a2*a2+a3*a3))*np.array([a2,a3]) 
```

Once we get all four linear interpolations, we put them together to get a Gouraud shaded triangle. To sum up RGB channels, we use filters. `feImage` imports images, `feComposite` composite images with the formula `result = k1*in1*in2 + k2*in1 + k3*in2 + k4 `. 

```xml
<filter>
    <feImage result="r0" xlink:href="#red"/>
    <feImage result="r1" xlink:href="#green"/>
    <feImage result="r2" xlink:href="#blue"/>
    <feComposite in="r0" in2="r1" k2="1" k3="1" operator="arithmetic"/>
    <feComposite in2="r2" k2="1" k3="1" operator="arithmetic"/>
</filter>
```

The transparency channel is applied through `mask`.

Code is [here](https://github.com/cover-me/matplotlib/blob/svg_gouraud/lib/matplotlib/backends/backend_svg.py#L656). I tried to rewrite `draw_gouraud_triangle` in Matplotlib's SVG backend, however, there are two problems make the effort useless:

1. Firefox and IE only support `feImage` to import objects external, not internal.

2. The anti-aliasing issue between two adjacent objects in mainstream SVG viewers.

Below are some results (**Only Visible In Chrome**):

```python
import matplotlib.tri as mtri
import matplotlib.pylab as plt
x = np.asarray([0, 2, 1])
y = np.asarray([0, 0, np.sqrt(3)])
z = [-1,0,1]
triangles = [[0, 1, 2]]
triang = mtri.Triangulation(x, y, triangles)
plt.tripcolor(triang, z, shading='gouraud',cmap='brg')
```

![](/images/gouraud_triangle.svg)

```python
import numpy as np
import matplotlib.pylab as plt
z = np.array([[-1,0],[0,1]])
zm = np.ma.masked_where([[1,0],[0,0]], z)
plt.pcolormesh(zm,cmap='brg',shading='gouraud',vmin=-1,vmax=1)
```

![](/images/gouraud_mask.svg)

Use code [here](https://matplotlib.org/2.0.1/examples/pylab_examples/quadmesh_demo.html)

![](/images/gouraud_complicated.svg)
