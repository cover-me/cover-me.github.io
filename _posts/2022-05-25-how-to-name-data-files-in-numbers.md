
---
layout: post
title: "How to name data files in numbers"
---

Assume we have multiple measurement systems, and there are many people. Anyone can do any experiment on any system. Each experiment generates plenty of data files. Then we plot data, make summaries, and share them with people. We put the file name in the title of each figure for future reference. How do we organize these data files? In particular, how do we name them?

In [instrDAQ](https://github.com/cover-me/instrDAQ), files are named after the timestamp. The name is unique if we use different prefixes on different computers. However, this type of name does not look natural to both humans and programs. For example, one can not tell which file is "the next scan" or how many scans are made on May 25 2022 looking at the whole file list.

In [qtlab](https://github.com/heeres/qtlab), files are named with an increased number. Names are also unique if we add computer-dependent prefixes to them. However, using this type of name requires the data-taking program to check the file list (or a stored parameter) every time to generate a new and correct number. qtlab can only do this for single folders. The file number is reset to 1 for each new folder. We want to use many subfolders not only because it is slow to open a folder containing too many files on Windows, but also because we want to have independent folders for different experiments (we call "cooldowns" as they are low-temperature experiments).

The file structure is as follows (we see that there are two "chip A" made by different persons...):

```
- Computer Datmon
  |- data
    |- datmon_2022-05-25_chipA_person1person2person3_cooldown1
    |  |- data
    |  |  |- datC123456.dat
    |  |  |- datC123457.dat
    |  |- logs
    |  |  |- log_chipA_deviceA.docx
    |  |  |- log_chipA_deviceB.docx
    |  |  |- summary_chipA_deviceA.pptx
    |  |  |- summary_chipA_deviceB.pptx
    |  |  |- random slides 1.pptx
    |  |  |- random slides 2.pptx
    |  |  |- random slides 3.pptx
    |- datmon_2022-05-26_chipA_person4_cooldown1
    |  |- data
    |  |  |- datC123458.dat
    |  |- logs
    ...
```

To make this work, we need to overwrite qtlab's file-number generating function in our scan scripts (see Qscan.220509.py [here](https://github.com/cover-me/repository/tree/master/qt/qtlab%20scan%20scripts), may be renamed):

```python
# overwrite the method qtlab used for checking data file count.
import data as d
def _check_last_number2(self, start):
    # 10 ms for 724*3 files, tims is consumed by os.listdir

    def get_max_file_num(folder):
        if not os.path.isdir(folder):
            return 0
        # filename = basename + *_%d.dat
        num_list = [int(i.split('_')[-1][:-4]) for i in os.listdir(folder) if i.endswith('.dat')]
        if num_list:
            return max(num_list)
        else:
            return 0
            
    folder, file_pre = os.path.split(self._basename)# basename = root/cooldown/data/ + file_pre (without '_%d.dat')
    max_num = get_max_file_num(folder)

    if max_num == 0:
        head, tail = os.path.split(folder)# folder = root/cooldown/, tail = data/
        head, tail = os.path.split(head)# head = root, tail = cooldown
        cd_folders = [os.path.join(head,i,'data') for i in os.listdir(head) if i != tail]
        for i in cd_folders:
            max_num = max(max_num, get_max_file_num(i))
    
    # print time() - t0
    return max_num + 1

d.IncrementalGenerator._check_last_number = _check_last_number2
```
