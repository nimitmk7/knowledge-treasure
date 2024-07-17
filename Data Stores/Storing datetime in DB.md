## datetime as datetime
Store the string as is, on the disk. 

**Example**: 02-04-2022T09:01:36Z

1. Convenient
2. Sub-optimal
3. Heavy in size → 20 bytes of data approx.
4. Heavy on Index
## datetime as epoch integer
Store it as epoch seconds/milliseconds. Represent it as seconds/milliseconds elapsed since 1st Jan 1970 00:00:00.000 

Example: 1721146684

1. Less Readability
2. Efficient and Optimal
3. Lightweight

## datetime as Custom Format - Example
Example: Store it as YYMMDD(integer). 


<iframe width="560" height="315" src="https://www.youtube.com/embed/zrl_odkY5tI?si=npi4UZ8yHcbwQ82S" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
