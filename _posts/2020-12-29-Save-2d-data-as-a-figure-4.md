---
layout: post
title: "Save 2d data as a figure 4: a PDF rasterizing bug"
---
It may sound a good idea to make scientific images as much "vector" as possible. However, this is not true. Images may look very different in different softwares if they are not rasterized, even for PDF files. There are a lot of issues, the most famous one of which is the "white line problem" (see previous 3 posts with the same title). So why not simply rasterize the image? Below is another issue of not rasterizing:

```python
import matplotlib.pyplot as plt
n = 500
z1 = [0]*n + [1]*n +[0]*n
z2 = [1]*n + [0]*n +[1]*n
fig,ax = plt.subplots(figsize=(2,2))
ax.imshow([z1,z2,z1],interpolation='none',origin='lower',aspect='auto', extent=(0,1,0,1))
plt.savefig('a.pdf')
```

The result in the Jupyter notebook, which is PNG by default, looks good:

![](/images/pdf_blur_1.png)

However, the generated PDF file looks bad in Chrome (good in Adobe Reader):

![](/images/pdf_blur.png)

Chrome and some other PDF previewers automatically apply interpolation to pixels in the PDF file. The file may look fine if n is smaller but bad again whem zoomed out.
