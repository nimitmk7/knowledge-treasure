## Types

1. Measurement Data (Value at time T) E.g. temperature, stock price
2. Event Data. E.g. Taxi pickup, Alarm

## Representation of time

### ISO 8601 format(Most standard)
![[Pasted image 20240215183436.png]]

## Other forms
Feb 28, 2022
02/28/2022
2022.16(Decimal Date)
Week 9
Monday(week day)
Day 59(year day)
11:59:59 am
11:59:59.283
11:59:59.283+0.00

## Properties

### 1. Can be Sequential or Cyclic 

![[Pasted image 20240215183925.png]]
Example:
![[Pasted image 20240215184007.png]]
### 2. Time is hierarchical/multi-resolution

![[Pasted image 20240215184122.png]]
![[Pasted image 20240215184211.png]]

## Code

Python: 

1. Use `datetime` package.

``` Python
import datetime

d = datetime.datetime(2023, 1, 1, 1, 55, 30)
d.isoformat()
```

Once we have a `datetime` object, we can easily extract different temporal attributes such as `year`, `month`, etc.


2. Use `numpy.datetime64`

```Python
import numpy as np

d = np.datetime64("2023-01-01T01:55:30")
```

We can also use the `item()` method to convert the `numpy.datetime64` object back to a `datetime.datetime` object. This allows us to access the same set of temporal attributes easily

3. Use `pandas.Timestamp`

Pandas provides its date/time data structure that wraps around `numpy.datetime64`, namely `pandas.Timestamp`. It also improves the API to make working with temporal data in a data science setting easier.

``` Python
import pandas as pd

d = pd.to_datetime("2023-01-01T01:55:30-0500")
```

What makes `pandas` special is that it can easily batch process a series of date/time data. For example, we can use `pandas.to_datetime` to convert a list of ISO 8601 strings to a series of `pandas.Timestamp` objects.

We can also batch extract different temporal attributes from this series easily just like working with a single `pandas.Timestamp` object. This capability is super convenient! This means we can very easily extract different properties of a column of temporal data and store the result as new column.


