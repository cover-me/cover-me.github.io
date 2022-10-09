---
layout: post
title: Export Qcodes data as DAT for qtplot
---

[Qcodes](https://github.com/QCoDeS/Qcodes) is a Python-based data acquisition framework for quantum measurement. It stores data in DB files. qtplot ([my fork repo](https://github.com/cover-me/qtplot)) is a convenient Python-based data visualization tool for analyzing and publication. One can apply math filters (such as a derivative in y direction and smooth), explore linecuts, play with the colorbar in qtplot. In this post I will show readers how to export Qcodes datasets as DAT files so that they can be analyzed with qtplot.

# Why?

To read Qcodes datasets and instrument status stored in DB files, one need to install the python package Qcodes or lower-level database packages. This is very inconvenient.

Even with Qcodes installed, the visualization tool is less powerful than qtplot (see the next section).

# Qcodes data structure and its data viewer

For what I understand, Qcodes stores binary data in a database (DB file). Inside the DB file, each dataset has a unique id called "run_id". Datasets are also tagged with experiment names ('characterization', 'go to regime 1', 'go to regime 2'), sample names ('device A', 'device B'), and 'run' names. Qcodes provides a data viewer, Plottr, which looks like this:

![image](https://user-images.githubusercontent.com/22870592/194747264-412f2212-b9a4-4012-bff7-38b8a6b6a28e.png)

Data can be visualized, however no filters, linecuts, or colormap settings can be applied.

![image](https://user-images.githubusercontent.com/22870592/194747575-6d316d02-d85b-48a9-950a-3189a70f1f71.png)


# Code for exporting

First, define functions:

```python
import pandas as pd
import numpy as np
import os

import qcodes as qc
from qcodes import Station, initialise_or_create_database_at, \
    load_or_create_experiment, Measurement, Parameter
    
    
def are_indexes_equal(df_dict):
    '''
    Check if all dataframes in the dictionary have the same index
    '''
    index_list = [i.index for i in df_dict.values()]
    # check index (array values)
    are_vals_equal = np.all([i==index_list[0] for i in index_list])
    # check index names
    index_names_list = [i.names for i in index_list]
    are_names_equal = np.all([i==index_names_list[0] for i in index_names_list])
    
    return are_vals_equal and are_names_equal

def add_index_to_col(df):
    '''
    Add index to columns
    The first column is the index changes fastest.
    '''
    if len(df.index.names)>1:# more than 1 indexes
        ind_shape = df.index.levshape
        # df.index.levels[0]： values (list) of the first index
        if not list(df.index.levels[0]) == [i[0] for i in df.index[:ind_shape[0]]]:
            step = -1
        else:
            step = 1
        new_col_names = list(df.index.names)[::step] + list(df.columns)
        size_list = list(ind_shape[::step])
    else:
        new_col_names = list(df.index.names) + list(df.columns)
        size_list = [len(df.index)]

    # Convert index into normal columns
    df = df.reset_index()
    # Reset index columns so that first col changes fastest
    df = df.reindex(columns=new_col_names)
    return df,size_list

def dataset_to_dataframe(dataset):
    df_dict = dataset.to_pandas_dataframe_dict()
    if are_indexes_equal(df_dict):
        # combine multiple dataframes of parameters into one
        df = pd.concat(df_dict.values(), axis='columns')
        df,size_list = add_index_to_col(df)
        return df,size_list
    else:
        raise('Indexes are not equal, check the dataset!')
        
def get_dat_filename(dataset):
    return f'{os.path.split(dataset.path_to_db)[-1][:-3]}.{dataset.run_id}.dat'
        
def get_dat_meta(dataset, dataframe, size_list):
    paramspecs = dataset.paramspecs
    counter = 0
    meta_string = f'''\
# Filename: {get_dat_filename(dataset)}
# Timestamp: {dataset.run_timestamp()}

'''
    for name in dataframe.columns:
        p = paramspecs[name]
        label = f'{p.name}_({p.label}({p.unit}))'
        meta_string += f'''\
# Column {counter+1}:
#\tname: {label}
'''
        if counter<len(size_list):
            meta_string += f'''\
#\tsize: {size_list[counter]}
'''
        elif counter<3:# otherwise qtplot won't import the data
            meta_string += f'''\
#\tsize: {1}
'''
        counter += 1
    meta_string += '\n'
    return meta_string

def export_setting_file(dataset):
    dat_path = get_dat_filename(dataset)
    settings_str = f'''\
Filename: {dat_path}
Timestamp: {dataset.run_timestamp()}

'''
    
    instr_dict = dataset.snapshot['station']['instruments']
    for ins_key, ins in instr_dict.items():
        settings_str += f'Instrument: {ins_key}\n'
        if 'address' in ins:
            settings_str += f"\taddress: {ins['address']}\n"
        for para_key, para in ins['parameters'].items():
            if 'value' in para:
                settings_str += f"\t{para_key}: {para['value']}\n"
        settings_str += '\n'
        
    with open(dat_path.replace('.dat','.set'), 'w') as f:
        f.write(settings_str)
        
def export_dat_file(dataset):
    df,size_list = dataset_to_dataframe(dataset)
    meta = get_dat_meta(dataset,df,size_list)
    save_to_path = get_dat_filename(dataset)
    with open(save_to_path, 'w') as f:
        f.write(meta)
        df.to_csv(f, sep='\t', float_format='%.12e', lineterminator='\n', index=False, header=False)
```

Then load a dataset and export files: 

```python
db_file_path = os.path.join(os.getcwd(), 'plottr_for_live_plotting_tutorial.db')
initialise_or_create_database_at(db_file_path)
dataset = qc.load_by_id(34)

export_dat_file(dataset)
export_setting_file(dataset)
```

Drag and drop the DAT file onto qtplot:

![image](https://user-images.githubusercontent.com/22870592/194759078-f00a3b85-3588-43be-9ff8-c0655ecd9ada.png)



