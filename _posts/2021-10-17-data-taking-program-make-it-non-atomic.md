---
layout: post
title: Data taking program\: make it non-atomic
---

# Motivation

Unlike [InstrDAQ](https://github.com/cover-me/instrDAQ), data taking and output channel setting in [qtlab](https://github.com/heeres/qtlab) and many other similar programs are usually atomic, i.e., operations are done by a single function, such as `instrument_X.get_reading_Y()` or `instrument_X.set_output_Y()`. On the other hand, non-atomic means that an operation is split into more than one steps, e.g., `instrument_X.send_query_message()` followed by `instrument_X.get_response()`.

There are two reasons to make these operations non-atomic. First, an instrument may take tens of milliseconds to prepare the data after it receives a query message. It is more efficient to send query messages to all instruments, wait once, read all responses, than query-wait-read one instrument by one instrument. It also decreases the time misalignment of measurements from different instruments. Second, even if the hardware delay in the first case is zero, we still want to make the output setting non-atomic so that we can set all output channels, wait only once, for stability or avoiding gate voltages to change too fast, instead of set- instrument1-wait, set- instrument2-wait, set- instrument3-wait (wait multiple times)...

# Method

The script can be found in https://github.com/cover-me/repository/tree/master/qt/qtlab%20scan%20scripts. It is named as `Qscan.210729.draft10.py` but may be renamed in the future. This is a script for [qtlab](https://github.com/heeres/qtlab) ([demo]( https://cover-me.github.io/2019/03/31/qtplot-demo.html)). With  `Qscan` we can do any linear 1D, 2D or 3D scans with code `scan(D1,D2,D3)`, where `Di (i=1,2,3)` has the form `([channel_i1, channel_i2, channel_i3, ...], [start_i1, start_i2, start_i3, ...], [stop_i1, stop_i2, stop_i3, ...], num_of_points_i, stabilization_time_after_set_i)`, e.g., a 2d scan: `scan(['I1(e-2uA)','I2(e-2uA)'],['dac1','dac2'],[-100,-100],[100,100],100,['Bx(T)'],['magnetX'],[0],[1],100)`. It can also communicate with qtplot and MS Word to show realtime plot and log measurement automatically.

## Non-atomic reading

`self._query_list` is the list of channels to read. A group of data is generated after going through `self._query_list`. To run non-atomic reading, the get function of a reading channel must have the` flag` argument, which may not always be in the case. See Sec. Note.

```python
def take_data_nonatomic(self):
    # flag 0 (default): write command and read respond, 1: write only, 2: read only
    val = []
    for instr, para_name, label in self._query_list:
        if instr is not None:
            instr.get(para_name, flag=1)

    for instr, para_name, label in self._query_list:
        if instr is not None:
            ans = instr.get(para_name, flag=2)
            if type(ans) == list:
                val += ans
            else:
                val.append(ans)
    val += self.get_prcss(val)#add processed data
    return val      
```

##  Non-atomic setting

Non-atomic setting is a little more complicated. There are two kinds of output channels. One is like the magnet, we set, it ramps gradually. The other one is a DAC or a voltage source, we set, it changes immediately. For the second type of outputs, qtlab has the feature that they can be assigned with `maxstep` and `stepdelay`. If the difference between the target value (SV) and the current value (PV) is larger than the `maxstep`, qtlab will sweep the channel step by step, each step by the change of `maxstep` and wait for `stepdelay` except the last step which may be smaller than the `maxstep` and no waiting.  The code bellow only takes care of the second type of output channels (see Sec. Note).

```python
def do_set_nonatomic(self, setpoint_list):
    '''
    Set a list of channels
    input:
        setpoint_list: a list of setpoints. A setpoint is like [instr_name, para_name, sv]
    '''
    step = 0
    delay = 0
    pv_list = []
    delta_list = []

    # calculate step, delay, pv_list, delta_list, sign_list
    for i in setpoint_list:
        instr_name, para_name, sv = i
        instr = qt.instruments.get(instr_name)
        para = instr.get_parameters()[para_name]
        if 'maxstep' in para:# channels like DAC
            pv = para['value']
            if pv is None:
                pv = instr.get(para_name)
            d = pv - sv
            pv_list.append(pv)
            delta_list.append(d)
            if step==0 or step>para['maxstep']:
                step = para['maxstep']
            if delay<para['stepdelay']:
                delay = para['stepdelay']
        else:# channels like magnet
            pv_list.append(sv)
            delta_list.append(0)
            instr.set(para_name, sv)

    sign_list = [int(i<0)*2-1 for i in delta_list]

    # ramp channels
    while 1:
        for i in range(len(setpoint_list)):
            if delta_list[i] != 0:
                instr_name, para_name, sv = setpoint_list[i]
                instr = qt.instruments.get(instr_name)
                if abs(delta_list[i])> step:
                    pv_list[i] += sign_list[i] * step
                    delta_list[i] += sign_list[i] * step
                else:
                    pv_list[i] = sv
                    delta_list[i] = 0
                instr.set(para_name, pv_list[i])
        if all(d==0 for d in delta_list):# True if delta_list is empty
            break
        else:
            qt.msleep(delay/1000.)
```

## Notes
`Qscan.210729.draft10` still uses the atomic reading and setting strategy by default. To make it non-atomic, one should add

```python
g = get_set()
g.take_data = g.take_data_nonatomic
g.do_set = g.do_set_nonatomic
e = easy_scan()
```

to the scan script.
