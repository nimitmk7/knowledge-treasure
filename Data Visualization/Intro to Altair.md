- **Chart** is the most fundamental data structure in Altair.
- It takes a Pandas dataframe as input.
- So far, this chart has not mark or encoding.

## Data

- Altair recognizes many different forms of data. 
- The most popular forms:
 1. pandas DataFrame
 2. Altair DataObject
 3. URL

```Python
source = data.cars() # <- Pandas dataframe
chart = alt.Chart(source)
```
## Mark

``` Python
# Use point as mark (scatter plot)
chart = chart.mark_point()
```
- Other marks include `mark_line`, `mark_bar` etc.
- `chart.mark_point()` does not modify the existing chart.
- Thus we need to assign the output back to the chart variable.

```Python
chart = chart.mark_point(color="black", fill="yellow", strokeWidth = 1.5)
```

- One can specify a number of properties, which will be constant across all observations.

## Encoding

```Python
chart = chart.encode(x = "Year", y = "Miles_per_Gallon")
```

- Each encoding is passed as an argument.
- Other encodings include `color`, `shape`, `fill`, `size`, etc.

### Our chart so far


![[visualization.png]]

- Instead of assigning x and y with strings representing column names, we can directly assign them to encoding schema objects.
- More options can be set via the encoding schema objects(like `title`)

```Python
chart = chart.encode(x = alt.X("Year"), y = alt.Y("Miles_per_Gallon", title = "Fuel Economy"), color = "Origin")
```

![[Pasted image 20240209224150.png]]

## Facet

