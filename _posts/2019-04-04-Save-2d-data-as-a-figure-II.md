---
layout: post
---
Base on the discussion in [this](https://cover-me.github.io/2019/02/17/Save-2d-data-as-a-figure.html) post, I can either use imshow() or pcolormesh() from Matplotlib to show heatmaps of uniformly-sampled 2D matrixes. imshow() is a clearer way as it doesn't increase pixels (e.g., a 7\*7 matrix will result in a heatmap of 7\*7 pixels) and you don't need to rasterize the result (because it's already rasterized with 7\*7 pixels...). However, as mentioned in the old post, there are issues with the imshow() I am using. I use the old WinPython-32bit-2.7.9.5, on which you may laugh at, hahaha!

One issue is that there are white spaces between axes and the heatmap. This only applies to the GUI of Matplotlib. Sometimes you need to zoom in to see clearly. The exported files should be free of this issue.

Another problem comes up when I export figures as .svg files. I prefer .svg because it's better supported by Inkscape. I plot a 7*7 matrix with my [forked version](https://github.com/cover-me/qtplot) of [qtplot](https://github.com/Rubenknex/qtplot), and here are the results:

* White spaces between axes and the figure:

![](/images/7x7_snapshoot.png)

* Export as a .pdf file, open in Chrome. Looks good, no white spaces. (the .png file also looks good)

![](/images/7x7_pdf_in_chrome.png)

* Export as a .svg file, open in Chrome and Inkscape. Looks very different from .pdf files, some kinds of strange.

![](/images/7x7_svg_in_chrome.png)

![](/images/7x7_svg_in_inkscape.png)

If I save the .pdf file as a .svg file or the .svg file as a .pdf file with Inkscape, the results are both bad. If I open the .pdf file and save it as a .pdf file in Inkscape, the result is good. I extracted images in these files and found no problem. The images in .svg files were extracted by opening them in a text editor and copy the encoded base64 string of image to an online decoder. The images in .pdf files were extracted by copying-pasting them from Adobe Reader to Windows Paint. Finally, I find the reason for the white spaces and blurred images in exported .svg files, by comparing the XML attributes in Inkscape.

Just add "preserveAspectRatio: none" and "style: image-rendering:optimizeSpeed" abttributes to the image in .svg file!
