---
layout: post
title: "pandas: Drawing secondary y axis with MultiIndex columns"
date: 2017-08-07
categories: python
---

When creating visualization with pandas, drawing secondary y axis seems trivial. According to the [official document](https://pandas.pydata.org/pandas-docs/stable/visualization.html), you can simply specifiy `secondary_y` option as below.

```python
ax = df.plot(secondary_y=['A', 'B'])
```

However, it doesn't always draw the secondary y axis when a DataFrame has Multi-Indexed columns. So I will walk you through the steps to draw secondary y axis in such case. 

- - -
## Creating a DataFrame
Consider the following DataFrame.


```python
>>> import pandas as pd
>>> import numpy as np
>>> import matplotlib.pyplot as plt

>>> index = pd.MultiIndex.from_tuples([("top",1),("top",2),("bottom",1),("bottom",2)], names=['first', 'second'])
>>> index
MultiIndex(levels=[['bottom', 'top'], [1, 2]],
           labels=[[1, 1, 0, 0], [0, 1, 0, 1]],
           names=['first', 'second'])
```
Here, a MultiIndex is created from a list of tuples. We will use tihs MultiIndex to create a DataFrame with MultiIndexed columns.

```python
>>> df = pd.DataFrame(np.random.randn(3, 4), index=['A', 'B', 'C'], columns=index)
>>> print(df.head(3))

first        top              bottom
second         1         2         1         2
A      -0.289443 -0.438610  0.020704  0.521503
B       0.240510  1.057098 -0.672878  0.381689
C       0.479786  2.037174 -0.795506  0.238856
```
In this DataFrame, we have a MultiIndexed column with the first level named "first", and the second level named "second". Now, let's try to make a plot with two different y axes (note that "axes" as a mathematical term, not Axes class in matplotlib).

- - -
## Making plots
Making plots with DataFrame is simple. First, create a figure and a axes (now Axes class) using matplotlib as below.

```python
>>> fig, ax1 = plt.subplots()
```
If you want to make a plot with "top" columns as main y varialble and "bottom" columns as secondary y variable, according to the official document,

```python
>>> df.plot(use_index=True,y="top", secondary_y="bottom", ax=ax1)
<matplotlib.axes._subplots.AxesSubplot object at 0x11090aa58>

>>> plt.show()
```
![Example:secondary_y not drawn](/images/secondary_y_1.png)

Here, we set `use_index=True` so that the index is taken as x, and we are drawing "top" as y. The option `ax=ax1` specify on which Axes we want to make plot. It seems that this is all we need to draw two sets of columns on different y axes. However, as you can see, the secondary y axis is not drawn. The workaround that I found is the following. First, draw the columns that you want to plot on the main (left) y axis.

```python
>>> df.plot(use_index=True,y="top", ax=ax1)
```

Then we draw the second set of columns as y variable but with `secondary_y` set True.

```python
>>> df.plot(use_index=True,y="bottom", secondary_y=True, ax=ax1)
```

![Example:secondary_y drawn](/images/secondary_y_2.png)

You will see that in this way, we can obtain the desired plot with "bottom" columns properly drawn on the secondary y axis. 

- - -
## Decorating secondary y axis
Of course, there is no fun if you don't decorate your new plot. Here are some tips to set limits and labels on the axes.

```python
>>> ax1.set_ylabel('$I_{PIN}$ [mA]')
>>> ymin, ymax = ax1.get_ylim()
>>> ax1.set_ylim([ymin*1.5,ymax*1.5])
>>> ax1.autoscale(enable=True, axis='x')
```
Here, you can set the label on the primary y axis using `set_ylabel`. If you don't want to set the range of y axis manually, you can get the maximum and minimum of data by `get_ylim()`. Then you can use these values to set the limits on the primary y axis. The last option auto-scales the x axis so that all data points are shown. 

```python
>>> if hasattr(ax1, 'right_ax'):
>>>     ax1.right_ax.set_ylabel('active modules')
>>>     ax1.right_ax.set_ylim([-3,2])
```
Here, we set the label and the limits on the secondary y axis. In case you are generating many plots, you would want to check if `right_ax` is available before setting the label or the limits. This is possible if a plot has no data on the columns that you are drawing as the secondary y axis.

Here is the output of the resulting plot. Looks good to me!

![Example:secondary_y drawn](/images/secondary_y_3.png)

- - -
