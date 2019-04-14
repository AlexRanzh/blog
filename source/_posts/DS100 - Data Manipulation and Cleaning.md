---
title: DS100&More - Data Manipulation and Cleaning
date: 2019-04-06 21:16:24
layout: post
tags:
    - Data Science
    - Python
    - Pandas
    - Data Cleaning
categories:
    - Study Notes
    - DS Notes
---
This is the first section for my study note: DS100&More, which records some useful points in data ETL(Extract, Transform and Loaded), visualisation, and modelling. The motivation is to keep a record of my second learning of UC Berkeley DS100 course:  Principles and Techniques of Data Science. By learning relative knowledge, reader should be familiar with some python tools that are in use now, as well as making some inferences from data by hand.

Please contact me at zr116@ic.ac.uk for any correction and improvement, general discussion on data science is also welcome.

### Today's Objectives

1. EDA(Exploratory Data Analysis)
2. Use pyhton Pandas to do EDA

### Tabular Data
[DS100 Textbook: Tabular Data](https://www.textbook.ds100.org/ch/03/pandas_intro.html)

The first thing to do when we have a new data set in hand is to look at its inside. Data can be structured in different ways:
1.1 Comma-Separated Values (CSV)
[example]
1.2 Tab-Separated Values (TSV)
2.1 JavaScript Object Format (JSON)
[curly brackets]
3.1 eXtensible Markup Language (XML)
[how websites store info]
3.2 HyperText Markup Language (HTML)

>CSV and TSV data is structured in a tabular structure, in rows and columns.
>JSON, XML, HTML data has a hierarchical structure, like a tree.
(see Hokodo intern note for JSON processing methods)

Check file format:
1. right click and view profile
2. fancier way:
use command-line interface (CLI) tools/terminal
```
ls
```
or if you are in Jupyter notebook:
```
!ls
```
> !+command line in Jupyter

Although all these data formats are often used, CSV is often the easiest and most frequently used structure.

Before reading data, need to figure out how much memory it is going to take.

```
!ls -l -h folder
!du -h folder
#specific file usage
!du -sh folder/*
```
As a rule of thumb, reading in a file of x GB requires 4x GB memory. `Pandas` needs 2x but has to share memory with other programs.

>**Memory Overhead**
Note that memory is shared by all programs running on a computer, including the operating system, web browsers, and yes, Jupyter notebook itself. A computer with 4 GiB total RAM might have only 1 GiB available RAM with many applications running. With 1 GiB available RAM, it is unlikely that `pandas` will be able to read in a 1 GiB file.

- To view CSV data in python:

```
import pandas as pd
data = pd.read_csv('')
```

- How many rows and columns:
```
data.shape
```
> Check the size of data before inpecting it to avoid printing too many columns, which is not helpful to getting to konw the data.

- Total size:
```
data.size
```
- Inspecting important statistical values of numerical variables in a data set:
```
data.describe()
```
This returns count, mean, standard deviation, min, max, and quartiles. [Details](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.describe.html)

- inspect first/last several rows(default 5)
```
data.head(5)#first 5
data.tail(3)#last 3
```
- other common command
```
#set 'col_name' column as index column, in_place replace the old index column
data.set_index('col_name', in_place = True)

#list all column names
data.columns

#selection
##select entries in column 'col1'
data['col1']
##select two columns: col1, col2
data[['col1','col2']]
##make selected part into a DataFrame
data[['col1','col2']].to_frame()
##select rows that has val1 in col1
data[data['col1']==val1]

#column manipulation
##count number of each value in a column
data['col1'].value_count()
##print unique values in a column
data['col1'].unique()
##statistical values of a column
data['col1'].min()
data['col1'].max()
data['col1'].median()

#locator
data.loc[row_list,col_list]
data.iloc[[row_index],[column_index]]

```
