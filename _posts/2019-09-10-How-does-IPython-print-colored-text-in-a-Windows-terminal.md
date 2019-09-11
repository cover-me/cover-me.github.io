---
layout: post
---
I thought there were only two colors in the Windows command line, black and white, or so. One day I realized that it was not true. Sometimes the prompts of IPython may be colorful. That is so mysterious to me at first. You can search "ipython terminal" images for examples. What technique do they use to make text colorful? I decided to find it out, even though it looks useless for me, like a dragon-killing skill (there is no dragon in the world) or calculating Landau levels with three different gauges (inspired by the story of Kong Yiji).

First, I noticed that the ANSI escape codes were usually used for coloring text. ANSI codes are natively supported on Linux. For Windows, one should hook up some Win APIs to use it. In IPython, this job is done by colorama (newer versions) or pyreadline (older versions). These modules make ANSI codes on Windows possible for IPython. However, they are hidden from dear IPython users. I can't make ANSI codes work by the "print" function (somehow I can use it in the prompt string of "input" function, which is less funny).

Secondly, I searched in the source code of IPython and pyreadline and found out how to make it work. Below are testing codes and screenshots for coulorful text. pyreadline seems not to support codes which move the cursor.

```
In [26]: IPython.version_info
Out[26]: (2, 4, 1, '')

In [27]: a='aaa'+'\033[1;31m'+'bbb'

In [28]: print a
aaa←[1;31mbbb

In [29]: IPython.core.interactiveshell.io.stdout.write(a)
aaabbb
In [30]: print a
aaa←[1;31mbbb


In [48]: import pyreadline

In [49]: c = pyreadline.console.Console(0)

In [50]: c.write_color('aaa\n',11)
aaa
Out[50]: 4

In [51]: del c
```
![](/images/ipython_ansi_test.png)
