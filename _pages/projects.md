---
permalink: /projects/
title: "Publications and Projects"
---

Tempor velit sint sunt ipsum tempor enim ad qui ullamco. Est dolore anim ad velit duis dolore minim sunt aliquip amet commodo labore. Ut eu pariatur aute ea aute excepteur laborum. Esse ea esse excepteur minim mollit qui cillum excepteur ex dolore magna. Labore deserunt fugiat incididunt incididunt sint ea. Consequat dolore aute laboris quis proident quis non et est consectetur ex eiusmod sit culpa.

# CSS_project

## Topic
This is the repository for an academic study focusing on the development **Computational Social Science (CSS)**. The project focuses on *Political Science* programs in the top universities of the world. We have created a **new dataset** that you can find the number CSS courses offered in **graduate** level in various subjects.

## Scope
The dataset contains the the **top 40 universities** from the **Times Higher Education (THE)** World University Rankings 2021. Since 7 of these universities do not house a political science department or a graduate program on political science, they have *NA* values for all variables.

## Variables
After the NA values are removed, the dataset contains 33 observations and 11 variables. These variables are:
  - `rank`: The THE ranking for the university. Indicating the 2021 rank. Rankings can be reached via this [link](https://www.timeshighereducation.com/world-university-rankings/2021/world-ranking#!/page/0/length/25/sort_by/rank/sort_order/asc/cols/stats).
  - `uni_name`: Full formal name of the university.
  - `short`: Shortened name of the university. Some universities do not have abbreviations or formal short names, these cases were shortened by removing the "university" or something else.
  - `Country`: Indicating in which country the university is located. Transforming country names to region or COW code can be done by the `countrynames` library in R.
  - `intro_stat`: Indicating the number of courses (or modules) on a **introductory** level, dealing with statistics/mathematics, causal inference, quantitative/mixed research methods, simple/multivarate regression techniques.
  - `advanced_stat`: Indicating the number of **advanced** courses (or modules) on statistics and other fields explained above.
  - `game`: Indicating courses (or modules) on **game theory** or **formal modelling**.
  - `network`: Indicating courses (or modules) on **network analysis**.
  - `abm`: Indicating courses (or modules) on **agent-based modelling**.
  - `data_sci`: Indicating courses (or modules) on **data science**. Covering topics on big data and machine learning.
  - `coding`: Indicating courses (or modules) on coding. Specifically designed to teach a programming software for political science students.
  - `total`: Sum of values (rowwise) of number of courses for all subject variables.

> Be reminded that only courses on **graduate level** that were given by a **political science** department were counted in this study. Undergraduate courses, independent workshops, or CSS courses given by other departments were not considered.

## Goal
This study aims to see, to what extent CSS courses are given in top universities around the world. Which subjects are prioritized and which subjects are not generally covered. We have observed that no course on agent-based modelling was given in any of these universities. We also observed that network analysis courses were given quite rarely than the other subjects.

# Analyses and Visualization of the Data 

## Summary statistics of the data

```r 
summary(css2[,4:11])
sapply(css2[,4:11], mean)
sapply(css2[,4:11], range)
```
```
   intro_stat advanced_stat         game          network            abm       data_sci
 Min.   :1    Min.   : 0.000   Min.   :0.000   Min.   :0.0000   Min.   :0   Min.   :0  
 1st Qu.:2    1st Qu.: 1.000   1st Qu.:0.000   1st Qu.:0.0000   1st Qu.:0   1st Qu.:0  
 Median :3    Median : 2.000   Median :1.000   Median :0.0000   Median :0   Median :1  
 Mean   :3    Mean   : 2.091   Mean   :1.485   Mean   :0.2727   Mean   :0   Mean   :1  
 3rd Qu.:4    3rd Qu.: 3.000   3rd Qu.:2.000   3rd Qu.:0.0000   3rd Qu.:0   3rd Qu.:2  
 Max.   :6    Max.   :13.000   Max.   :5.000   Max.   :2.0000   Max.   :0   Max.   :3  
     coding           total   
 Min.   :0.0000   Min.   : 2  
 1st Qu.:0.0000   1st Qu.: 4  
 Median :0.0000   Median : 8  
 Mean   :0.1515   Mean   : 8  
 3rd Qu.:0.0000   3rd Qu.:10  
 Max.   :2.0000   Max.   :26  
> sapply(css2[,4:11], mean)
   intro_stat advanced_stat          game       network           abm      data_sci        coding 
    3.0000000     2.0909091     1.4848485     0.2727273     0.0000000     1.0000000     0.1515152 
        total 
    8.0000000 
> sapply(css2[,4:11], range)
     intro_stat advanced_stat game network abm data_sci coding total
[1,]          1             0    0       0   0        0      0     2
[2,]          6            13    5       2   0        3      2    26
```

## Scatter Plot - Total Number of CSS Courses per University

```r
library(ggplot2)
library(hrbrthemes)

css2$Country <- as.factor(css2$Country)
Region <- ifelse(css2$Country == "USA", "US Universities", "Other")

totalplot <- ggplot(css2, aes(rank, total)) +
  geom_point(aes(color= Region, size = total)) +
  guides(size = FALSE) +
  labs(x = "University ranking", y = "Number of courses") +
  geom_hline(yintercept = mean(css2$total), linetype = "dotdash") +
  geom_hline(yintercept = mean(US_schools$total), linetype = "longdash") +
  annotate(geom = "text", x = 22, y = 27, label = "University of Michigan", 
           hjust = "left", size = 3) +
  annotate(geom = "text", x = 43.5, y = mean(css2$total), label = "Average number of courses (8)", 
           hjust = "left", size = 2) +
  coord_cartesian(xlim = c(0,41), clip = "off") +
  annotate(geom = "text", x = 43.5, y = mean(US_schools$total), label = "Average number of courses in US (9.5)", 
           hjust = "left", size = 2) +
  coord_cartesian(xlim = c(0,41), clip = "off")
totalplot
```
![Rplot05.pdf](https://github.com/egemenbuyukkaya/egemenbuyukkaya.github.io/files/6409417/Rplot05.pdf)

{% include_relative north_south.html %}

