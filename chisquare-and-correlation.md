---
title: "Tests of Association - Chi-Square and Correlation"
author: "Ben Gonzalez"
date: '2023-10-09'
output: 
  html_document:
    css: columns.css
---

```{r setup, klippy, echo=FALSE, include=TRUE,message=FALSE}
klippy::klippy(tooltip_message = "Copy Code",tooltip_success = "Copy Successful!!!",position = "right")
library(mathjaxr)
library(latexpdf)
library(dplyr)
library(kableExtra)
library(ggplot2)
knitr::opts_chunk$set(echo = TRUE)
student <- read.csv(file = 'student.csv',sep = ",")
###Drop the unneeded first column
student <- student[,-c(1)]
```

<!-- :::::::::::::: {.columns} -->
<!-- ::: {.column width="30%"} -->

<!-- ```{r} -->
<!-- mtcars %>% -->
<!--     select(disp, mpg) %>% -->
<!--     sample_n(10) %>% -->
<!--     kbl() %>% -->
<!--     kable_styling() -->
<!-- ``` -->

<!-- ::: -->
<!-- ::: {.column width="5%"} -->

<!-- \ -->

<!-- ::: -->
<!-- ::: {.column width="65%"} -->


<!-- \ -->

<!-- ```{r, fig.height=4, fig.width=7} -->
<!-- mtcars %>% -->
<!--     ggplot(aes(x=disp, y=mpg)) + -->
<!--     geom_point() -->
<!-- ``` -->

<!-- ::: -->
<!-- :::::::::::::: -->

#### Steven's four scales of measurement

<br>

<style>
.container {
  border: 0.5px outset black;
  background-color: white;
  text-align: center;
  width: 80%;
}
</style>

<div class="container">
<style>
table {
  <!-- border-collapse: collapse; -->
  width: 30%;
}

th {
  text-align: center;
  padding: 20px;
  background-color: white;
}

tr {
  background-color: #D6EEEE;
  padding: 45px;
}
</style>
  <table class="table" id="myTable20">
    <thead>
      <tr>
        <th onclick="sortTable(0)">Characteristic of Scale</th>
        <th onclick="sortTable(2)">Nominal</th>
        <th onclick="sortTable(2)">Ordinal</th>
        <th onclick="sortTable(2)">Interval</th>
        <th onclick="sortTable(2)">Ratio</th>
      </tr>
    </thead>
    <tbody>
  <tr>
    <td style="text-align:left">Applies names or numbers to categories?</td>
    <td>Yes</td>
    <td>Yes</td>
    <td>Yes</td>
    <td>Yes</td>
  </tr>
  <tr>
    <td style="text-align:left">Orders categories according to quantity?</td>
    <td ></td>
    <td>Yes</td>
    <td>Yes</td>
    <td>Yes</td>
  </tr>
    <tr>
    <td style="text-align:left">Displays equal intervals between consecutive numbers?</td>
    <td ></td>
    <td></td>
    <td>Yes</td>
    <td>Yes</td>
  </tr>
  <tr>
  <td style="text-align:left">Diplays a "true zero point?"</td>
    <td ></td>
    <td></td>
    <td></td>
    <td>Yes</td>
  </tr>
    </tbody>
     </table>
</div>

<br>

### Student Peformance Data Set Information

##### Attributes for both student-mat.csv (Math course) and student-por.csv (Portuguese language course) datasets:


:::::::::::::: {.columns}
::: {.column width="50%"}

+ 1 school - student's school (binary: 'GP' - Gabriel Pereira or 'MS' - Mousinho da Silveira)
+ 2 sex - student's sex (binary: 'F' - female or 'M' - male)
+ 3 age - student's age (numeric: from 15 to 22)
+ 4 address - student's home address type (binary: 'U' - urban or 'R' - rural)
+ 5 famsize - family size (binary: 'LE3' - less or equal to 3 or 'GT3' - greater than 3)
+ 6 Pstatus - parent's cohabitation status (binary: 'T' - (living) together or 'A' - apart)
+ 7 Medu - mother's education (numeric: 0 - none,  1 - primary education (4th grade), 2 (5th to 9th grade), 3 (secondary education) or 4  (higher education))
+ 8 Fedu - father's education (numeric: 0 - none,  1 - primary education (4th grade), 2 (5th to 9th grade), 3 (secondary education) or 4  (higher education))
+ 9 Mjob - mother's job (nominal: 'teacher', 'health' care related, civil 'services' (e.g. administrative or police), 'at_home' or 'other')
+ 10 Fjob - father's job (nominal: 'teacher', 'health' care related, civil 'services' (e.g. administrative or police), 'at_home' or 'other')
+ 11 reason - reason to choose this school (nominal: close to 'home', school 'reputation', 'course' preference or 'other')
+ 12 guardian - student's guardian (nominal: 'mother', 'father' or 'other')
+ 13 traveltime - home to school travel time (numeric: 1 - <15 min., 2 - 15 to 30 min., 3 - 30 min. to 1 hour, or 4 - >1 hour)
+ 14 studytime - weekly study time (numeric: 1 - <2 hours, 2 - 2 to 5 hours, 3 - 5 to 10 hours, or 4 - >10 hours)
+ 15 failures - number of past class failures (numeric: n if 1<=n<3, else 4)
+ 16 schoolsup - extra educational support (binary: yes or no)

:::
::: {.column width="50%"}



+ 17 famsup - family educational support (binary: yes or no)
+ 18 paid - extra paid classes within the course subject (Math or Portuguese) (binary: yes or no)
+ 19 activities - extra-curricular activities (binary: yes or no)
+ 20 nursery - attended nursery school (binary: yes or no)
+ 21 higher - wants to take higher education (binary: yes or no)
+ 22 internet - Internet access at home (binary: yes or no)
+ 23 romantic - with a romantic relationship (binary: yes or no)
+ 24 famrel - quality of family relationships (numeric: from 1 - very bad to 5 - excellent)
+ 25 freetime - free time after school (numeric: from 1 - very low to 5 - very high)
+ 26 goout - going out with friends (numeric: from 1 - very low to 5 - very high)
+ 27 Dalc - workday alcohol consumption (numeric: from 1 - very low to 5 - very high)
+ 28 Walc - weekend alcohol consumption (numeric: from 1 - very low to 5 - very high)
+ 29 health - current health status (numeric: from 1 - very bad to 5 - very good)
+ 30 absences - number of school absences (numeric: from 0 to 93)


###### **These grades are related with the course subject, Math or Portuguese:**


+ 31 G1 - first period grade (numeric: from 0 to 20)
+ 31 G2 - second period grade (numeric: from 0 to 20)
+ 32 G3 - final grade (numeric: from 0 to 20, output target)

:::
::::::::::::::

### Look at the structure of our data

```{r}
str(student)
```
```{r,echo=FALSE }
print(paste('There are ',nrow(student),' observations in our student data set.',sep = " "))
```


### Chi-square


$$
x^2\;=\;\sum\frac{(O_{i}\;-\;E_{i})^2}{E_{i}}
\\
\\
$$
$$
x^2 = chi-squared\\
O_{i}=\;observed\;value
\\
E_{i}=\;expected\;value
$$

### Chi-square Assumptions

+ Chi-Square test – statistical test used to compare observed results with expected results. This is a test of association.
<br>
+ Used with Nominal or Categorical data.


### Chi-Square Test of Association

Here we want to investigate if there is an association between the type of job a mother has and access to the internet.


```{r}

mjob_internet<- xtabs(formula = ~Mjob+internet,data = student)
chisq.test(mjob_internet ,correct = FALSE)


```

We can see that our $X^2$ value is 28.861 and the p-value is $p<.05$. We check our $X^2$ value against the critical table value of 9.488. Since our $X^2$ value 1.401 is below our critical table value of 9.488 we **can** reject our null hypothesis. Therefore there is a significant association between Mjob type and internet.
<br>
[Chi-Square Critical Table](https://www.itl.nist.gov/div898/handbook/eda/section3/eda3674.htm){target="_blank"}

---

Next we can investigate if there is an association between the type of guardian a student has and access to the internet.

```{r}

guardian_internet<- xtabs(formula = ~guardian+internet,data = student)
chisq.test(guardian_internet ,correct = FALSE)


```
We can see that our $X^2$ value is 1.401 and the p-value is $p>.05$. We check our $X^2$ value against the critical table value of 5.991. Since our $X^2$ value 1.401 is below our critical table value of 5.991 we **cannot** reject our null hypothesis. Therefore there is no significant association between guardian type and internet.
<br>
[Chi-Square Critical Table](https://www.itl.nist.gov/div898/handbook/eda/section3/eda3674.htm){target="_blank"}

### Correlation


$$\Large
r_{xy} = \frac{\sum(x_{i}-\bar{x})(y_{i}-\bar{y})}{\sqrt{\sum(x_{i}-\bar{x})^2\sum(y_{i}-\bar{y})^2}}$$

### Correlation Assumptions

The following assumptions are in place when utilizing the Pearson’s $r$ when calculating a correlation coefficient.
<br>

+ **Interval or ratio scale** – both variables should be quantitative variables. Both should be assessed on an interval or ratio scale.
+ **Normally-distributed sampling distribution** – sampling distribution of the statistic should be normally distributed. Likely to be met if the sample data are approximately normal or the sample is large.
+ **Random Sample from bivariate normal distribution** – the data points used to compute the Pearson $r$ are the pairs of scores on the predictor and criterion variables. The data points should be a random sample drawn from a bivariate normal distribution. 


##### Index of Effect Size Table


+ Index of effect size – a statistic that conveys the strength of the association between a predictor variable and criterion variable.
+ $r$ – the larger the absolute value of the correlation coefficient the larger the “effect”.


<br>

<style>
.container {
  border: 0.5px outset black;
  background-color: white;
  text-align: center;
  width: 70%;
}
</style>

<div class="container">
<style>
table {
  <!-- border-collapse: collapse; -->
  width: 30%;
}

th {
  text-align: center;
  padding: 20px;
  background-color: white;
}

tr {
  background-color: #D6EEEE;
  padding: 45px;
}
</style>
  <table class="table" id="myTable20">
    <thead>
      <tr>
        <th onclick="sortTable(0)">Correlation Coefficient</th>
        <th onclick="sortTable(2)">Effect Size</th>
      </tr>
    </thead>
    <tbody>
  <tr>
    <td >Small Effect</td>
    <td >r +/- .10</td>
  </tr>
  <tr>
    <td >Medium Effect</td>
    <td >r +/- .30</td>
  </tr>
    <tr>
    <td >Large Effect</td>
    <td >r +/- .50</td>
  </tr>
    </tbody>
     </table>
</div>

<br>


#### Correlation between student absences and first period grade

```{r}
cor.test(student$absences,student$G1)
```

Here we can see that our correlation $r$ = -0.031 and our $p>.05$. Since our $p-value$ is greater than $0.05$ we can say there is no relationship between $absences$ and $G1$.


#### Correlation between student absences and final grade

```{r}
cor.test(student$absences,student$G3)
```

Here we can see that our correlation $r$ = 0.034 and our $p>.05$. Since our $p-value$ is greater than $0.05$ we can say there is no relationship between $absences$ and $G3$.


### Point-biserial Correlation Assumptions

+ **Point-biserial correlation coefficient** – appropriate when one variable is a roughly continuous , multi-value variable assessed on an interval or ratio scale and the other variable is a dichotomous(2 values) variable.
<br>
+ - must have a true dichotomy e.g. sex (male vs female)


#### Point-biserial correlation: Student gender and final grade

Here we are looking at a true dichotomy sex (male vs female) and the association with final $G3$ grade. We need to recode the variable into a binary variable of $1$ and $0$ with $1$ being $Male$ and $0$ being $Female$.

```{r}
student$sexrecoded<- ifelse(student$sex=="M",1,0)
cor.test(student$G3,student$sexrecoded)
```

### Biserial Correlation

+ **Biserial correlation coefficient** – appropriate in exactly the same situation where the point-biserial correlation would be used; one continuous variable and one dichotomous variable.
<br>
+ - appropriate when the dichotomous variable is not a true dichotomy e.g. pass/fail


#### Biserial Correaltion: Family educational support and final grade

Here we can look at whether there is an association between a students family support and their final grade. Again we need to re-code the variable so it is binary e.g. $0$ and $1$

```{r}
student$schoolsuprecoded <- ifelse(student$famsup=="yes",1,0)
cor.test(student$G3,student$schoolsuprecoded)
```

#### Biserial Correaltion: Extra educational support and final grade

Here we can look at if there is an association between $extra\;educational\; support$ $schoolsup$ and final grade $G3$

```{r}

student$schoolsuprecoded <- ifelse(student$schoolsup=="yes",1,0)

cor.test(student$G3,student$schoolsuprecoded)
```

#### Biserial Correaltion: Attended nursery school and final grade

Here we can look at if there is an association between those who attended nursery school $nursery$ and final grade $G3$

```{r}

student$nurseryrecoded <- ifelse(student$nursery=="yes",1,0)

cor.test(student$G3,student$nurseryrecoded)

```


### Spearman Correlation

Spearman correlation coefficient for ranked data (Spearman’s Rho or $r_s$) – displays the correlation between two variables whose values have been ranked.

In the student data set we see that studytime and traveltime have been ranked. Therefore we can utilize the Spearman's Rho $r_s$ to see the association between these variables and final grade $G3$.


##### Spearman Correlation: Study time and final grade

```{r}

cor.test(student$studytime,student$G3,method="spearman")

```


##### Spearman Correlation: Travel time and final grade

```{r}

cor.test(student$traveltime,student$G3,method="spearman")

```


### Phi Coefficient - measurement of the degree of association between two binary variables

Assumptions for phi $\phi$ coefficient:

+ Phi coefficient ($\phi$)– appropriate when both variables are true dichotomous variables

+ E.g subject sex (males versus female) with sport-team status (team-member vs not a team-member)


<br>

<style>
.container {
  border: 0.5px outset black;
  background-color: white;
  text-align: center;
  width: 70%;
}
</style>

<div class="container">
<style>
table {
  <!-- border-collapse: collapse; -->
  width: 30%;
}

th {
  text-align: center;
  padding: 20px;
  background-color: white;
}

tr {
  background-color: #D6EEEE;
  padding: 45px;
}
</style>
  <table class="table" id="myTable20">
    <thead>
      <tr>
        <th onclick="sortTable(0)"></th>
        <th onclick="sortTable(2)">$y=1$</th>
        <th onclick="sortTable(0)">$y=0$</th>
        <th onclick="sortTable(2)">$total$</th>
      </tr>
    </thead>
    <tbody>
  <tr>
    <td >$x=1$</td>
    <td >$n_{11}$</td>
    <td >$n_{10}$</td>
    <td >\medium$n_{1\cdot}$</td>
  </tr>
  <tr>
    <td >$x=0$</td>
    <td >$n_{01}$</td>
    <td >$n_{00}$</td>
    <td >\medium$n_{0\cdot}$</td>
  </tr>
    <tr>
    <td >$total$</td>
    <td >\medium$n_{\cdot1}$</td>
    <td >\medium$n_{\cdot0}$</td>
    <td >$n$</td>
  </tr>
    </tbody>
     </table>
</div>

<br>


$$\Large Phi\;equation: 
\phi = \frac{n_{11} \cdot n_{00}\;-\;n_{10} \cdot n_{01}}{\sqrt{n_{1\cdot}n_{0\cdot} n_{\cdot0} n_{\cdot1}} }$$

<br>

<style>
.container {
  border: 0.5px outset black;
  background-color: white;
  text-align: center;
  width: 70%;
}
</style>

<div class="container">
<style>
table {
  <!-- border-collapse: collapse; -->
  width: 30%;
}

th {
  text-align: center;
  padding: 20px;
  background-color: white;
}

tr {
  background-color: #D6EEEE;
  padding: 45px;
}
</style>
  <table class="table" id="myTable20">
    <thead>
      <tr>
        <th onclick="sortTable(0)"></th>
        <th onclick="sortTable(2)">$y=1$</th>
        <th onclick="sortTable(0)">$y=0$</th>
      </tr>
    </thead>
    <tbody>
  <tr>
    <td >$x=1$</td>
    <td >$A$</td>
    <td >$B$</td>
  </tr>
  <tr>
    <td >$x=0$</td>
    <td >$C$</td>
    <td >$D$</td>
    </tbody>
     </table>
</div>

<br>

$$Phi\;equation: 
\phi = \frac{A \cdot D\;-\;B \cdot C}{\sqrt{(A+B)(C+D)(A+C)(B+D)} }$$

### Phi coefficient:

First we will want to create a contingency of the table. We can do this in R simply by utilizing the $table$ function and specifying two columns of $dichotomous$ or $binary$ variables.

```{r}
gender_internet <- table(student$sex,student$internet)
print(gender_internet)

gender_address <- table(student$sex,student$address)
print(gender_address)
```

How to calculate a $phi$ coefficient manually for $gender$ and $internet$
```{r}
# Φ = (AD-BC) / √(A+B)(C+D)(A+C)(B+D)

phi_coefficient = ((38*159)-(170*28))/sqrt((38+170)*(28+159)*(38+28)*(170+159))
print(phi_coefficient)
```

We can measure the $Phi$ coefficient similarly as to how we measure a Pearson's $r$ correlation. The closer to $0$ there is no relationship. The closer to $-1$ to $1$ the stronger the relationship between the two dichotomous variables.

Here we can utilize the $psych$ package to perform a $Phi$ $\phi$ coefficient analysis.

```{r,message=FALSE}
library(psych)
phi(gender_internet,digits = 7)
```




<center><h2>References</h2></center>

Hatcher, L. (2013). *Advanced statistics in research: Reading, understanding, and writing up data analysis results. Shadow Finch Media.*

***Student Peformance*** (2014). https://archive.ics.uci.edu/dataset/320/student+performance

https://www.r-bloggers.com/2021/07/how-to-calculate-phi-coefficient-in-r/

William Revelle (2023). _psych: Procedures for Psychological,
  Psychometric, and Personality Research_. Northwestern University,
  Evanston, Illinois. R package version 2.3.9,
  <https://CRAN.R-project.org/package=psych>
