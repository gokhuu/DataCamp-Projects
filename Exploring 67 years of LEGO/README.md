## 1. Introduction
<p>Everyone loves Lego (unless you ever stepped on one). Did you know by the way that "Lego" was derived from the Danish phrase leg godt, which means "play well"? Unless you speak Danish, probably not. </p>
<p>In this project, we will analyze a fascinating dataset on every single lego block that has ever been built!</p>
<p><img src="https://s3.amazonaws.com/assets.datacamp.com/production/project_10/datasets/lego-bricks.jpeg" alt="lego"></p>

## 2. Reading Data
<p>A comprehensive database of lego blocks is provided by <a href="https://rebrickable.com/downloads/">Rebrickable</a>. The data is available as csv files and the schema is shown below.</p>
<p><img src="https://s3.amazonaws.com/assets.datacamp.com/production/project_10/datasets/downloads_schema.png" alt="schema"></p>
<p>Let us start by reading in the colors data to get a sense of the diversity of lego sets!</p>


```python
# Import modules
import pandas as pd
import numpy as np

# Read colors data
colors = pd.read_csv('datasets/colors.csv')

# Print the first few rows
colors.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>name</th>
      <th>rgb</th>
      <th>is_trans</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>-1</td>
      <td>Unknown</td>
      <td>0033B2</td>
      <td>f</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>Black</td>
      <td>05131D</td>
      <td>f</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>Blue</td>
      <td>0055BF</td>
      <td>f</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2</td>
      <td>Green</td>
      <td>237841</td>
      <td>f</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3</td>
      <td>Dark Turquoise</td>
      <td>008F9B</td>
      <td>f</td>
    </tr>
  </tbody>
</table>
</div>



## 3. Exploring Colors
<p>Now that we have read the <code>colors</code> data, we can start exploring it! Let us start by understanding the number of colors available.</p>


```python
# How many distinct colors are available?
colorDF = pd.DataFrame(colors)
num_colors = len(colorDF.index)
print(num_colors)
```

    135
    

## 4. Transparent Colors in Lego Sets
<p>The <code>colors</code> data has a column named <code>is_trans</code> that indicates whether a color is transparent or not. It would be interesting to explore the distribution of transparent vs. non-transparent colors.</p>


```python
#colors_summary: Distribution of colors based on transparency
colors_summary = colors.groupby('is_trans').count()
colors_summary
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>name</th>
      <th>rgb</th>
    </tr>
    <tr>
      <th>is_trans</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>f</th>
      <td>107</td>
      <td>107</td>
      <td>107</td>
    </tr>
    <tr>
      <th>t</th>
      <td>28</td>
      <td>28</td>
      <td>28</td>
    </tr>
  </tbody>
</table>
</div>



## 5. Explore Lego Sets
<p>Another interesting dataset available in this database is the <code>sets</code> data. It contains a comprehensive list of sets over the years and the number of parts that each of these sets contained. </p>
<p><img src="https://imgur.com/1k4PoXs.png" alt="sets_data"></p>
<p>Let us use this data to explore how the average number of parts in Lego sets has varied over the years.</p>


```python
%matplotlib inline
# Read sets data as `sets`
sets = pd.DataFrame(pd.read_csv('datasets/sets.csv'))
# Create a summary of average number of parts by year: `parts_by_year`
group = sets.groupby('year')
parts_by_year = group['num_parts'].agg(np.mean)
# Plot trends in average number of parts by year
print(parts_by_year)
```

    year
    1950     10.142857
    1953     16.500000
    1954     12.357143
    1955     36.857143
    1956     18.500000
    1957     42.619048
    1958     44.452381
    1959     16.250000
    1960    175.333333
    1961     70.588235
    1962     81.750000
    1963     33.333333
    1964     82.636364
    1965    107.100000
    1966     40.651685
    1967     98.666667
    1968    127.200000
    1969     64.594203
    1970     84.793103
    1971     67.022222
    1972    102.842105
    1973    103.367647
    1974    116.769231
    1975    155.225806
    1976    153.029412
    1977     91.500000
    1978    146.616438
    1979    105.414634
    1980    126.636364
    1981     97.835443
               ...    
    1988    144.250000
    1989    102.061404
    1990    202.035294
    1991    166.424528
    1992    119.617391
    1993    148.432432
    1994    127.640625
    1995    179.039062
    1996    201.770833
    1997    129.221649
    1998    141.126154
    1999    105.543333
    2000    104.376147
    2001    104.365782
    2002    115.700224
    2003    159.681928
    2004    138.862534
    2005    198.745455
    2006    246.904594
    2007    229.025078
    2008    231.644699
    2009    196.898263
    2010    210.646396
    2011    160.452191
    2012    149.808130
    2013    181.359191
    2014    169.320280
    2015    200.223881
    2016    248.945813
    2017    300.121277
    Name: num_parts, dtype: float64
    

## 6. Lego Themes Over Years
<p>Lego blocks ship under multiple <a href="https://shop.lego.com/en-US/Themes">themes</a>. Let us try to get a sense of how the number of themes shipped has varied over the years.</p>


```python
# themes_by_year: Number of themes shipped by year
themes_by_year = group['theme_id'].size()
print(themes_by_year)
```

    year
    1950      7
    1953      4
    1954     14
    1955     28
    1956     12
    1957     21
    1958     42
    1959      4
    1960      3
    1961     17
    1962     40
    1963     18
    1964     11
    1965     10
    1966     89
    1967     21
    1968     25
    1969     69
    1970     29
    1971     45
    1972     38
    1973     68
    1974     39
    1975     31
    1976     68
    1977     92
    1978     73
    1979     82
    1980     88
    1981     79
           ... 
    1988     68
    1989    114
    1990     85
    1991    106
    1992    115
    1993    111
    1994    128
    1995    128
    1996    144
    1997    194
    1998    325
    1999    300
    2000    327
    2001    339
    2002    447
    2003    415
    2004    371
    2005    330
    2006    283
    2007    319
    2008    349
    2009    403
    2010    444
    2011    502
    2012    615
    2013    593
    2014    715
    2015    670
    2016    609
    2017    470
    dtype: int64
    

## 7. Wrapping It All Up!
<p>Lego blocks offer an unlimited amount of fun across ages. We explored some interesting trends around colors, parts, and themes. </p>
