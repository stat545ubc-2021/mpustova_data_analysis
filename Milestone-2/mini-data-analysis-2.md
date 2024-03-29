Mini Data Analysis Milestone 2
================

Begin by loading your data and the tidyverse package below:

``` r
suppressMessages(library(datateachr)) # <- might contain the data you picked!
suppressMessages(library(tidyverse))
```

# Task 1: Process and summarize your data (15 points)

### 1.1 (2.5 points)

<!-------------------------- Start your work below ---------------------------->

After the further exploration of data, some of the research questions
should be adjusted.

Specifically, the initial version of Question 1 was: *Have the maximum
flow events increased in magnitude or frequency since 1909?*. However,
the exploration of the dataset showed that onle one maximum and minimum
flow event was recorded per year. Hence, one cannot analyze whether the
frequency of such events have increased. Therefore, Question 1 was
changed to: *Have the maximum and minimum flow events increased in
magnitude since 1909?*. The new question focus on both maximum and
minimum flow, which additionally adds complexity to the analysis.

Another question that have been adjusted is Question 4, which initial
version was as follows: *Have the number of data entries that are
estimated or have significant gaps during the day (sym=E or sym=B,
respectively) decreased over time?* First issue that have been found, is
that the question had a typo: symbol A, not B, corresponds to the
estimates of flow due to significant gaps during the day. Secondly, it
has been found, that most of the symbols present in the dataset was “B”,
which indicates that the streamflow value was estimated with
consideration for the presence of ice in the stream. Hence, it may be
valuable to include symbols A, B, and E in the analysis. Hence, the new
version of the research question is: *Have the number of data entries
that are estimated, have significant gaps during the day, or have been
affected by the presence of ice (sym=E, sym=A, and sym=B, respectively)
decreased over time?*

Hence, the final version of the research question is as follows:

1.  **Have the maximum and minimum flow events increased in magnitude
    since 1909?**

2.  **When did the absolute maximum and minimum magnitude of the flow
    occur?**

3.  **Does the annual distribution of the extreme flow events changed
    over time? For example, were most of the minimum/maximum events that
    have occurred in past 10 years observed in a similar time than those
    in 1909-1919 or have the pattern shifted?**

4.  **Have the number of data entries that are estimated, have
    significant gaps during the day, or have been affected by the
    presence of ice (sym=E, sym=A, and sym=B, respectively) decreased
    over time?**

<!----------------------------------------------------------------------------->

### 1.2 (10 points)

<!------------------------- Start your work below ----------------------------->

**I would like to note that in the following work I have tried to
incorporate as many of the proposed summarizing and graphing task as
possible. However, task 4 of graphing did not work for any of the
research question. This is due to the fact that my dataset only have one
variable which distribution would be valuable to explore - i.e. flow.
This variable have been previously explored in Milestone 1 using
histogram, hence it did not make sense to complete this task in this
milestone.**

Before starting the analysis, additional libraries should be loaded:

``` r
suppressMessages(library(lubridate))
```

## Question 1: Have the maximum and minimum flow events increased in magnitude since 1909?

Before starting the analysis, I would like to mention again that the
first research question was changed (initial version was: *Have the
maximum flow events increased in magnitude or frequency since 1909?*)

This is due to the fact that after further exploring the dataset, it
became apparent that only one maximum and one minimum extreme weather
event is recorded per each year (as shown in the following code):

``` r
#The following function computes summary statistics showing the number of entries for each year and each extreme type 

flow_sample_observations <- flow_sample %>%
    select(year, extreme_type, flow) %>%
    group_by(year, extreme_type) %>%
    summarize(n=n())
```

    ## `summarise()` has grouped output by 'year'. You can override using the `.groups` argument.

``` r
print(flow_sample_observations)
```

    ## # A tibble: 218 x 3
    ## # Groups:   year [109]
    ##     year extreme_type     n
    ##    <dbl> <chr>        <int>
    ##  1  1909 maximum          1
    ##  2  1909 minimum          1
    ##  3  1910 maximum          1
    ##  4  1910 minimum          1
    ##  5  1911 maximum          1
    ##  6  1911 minimum          1
    ##  7  1912 maximum          1
    ##  8  1912 minimum          1
    ##  9  1913 maximum          1
    ## 10  1913 minimum          1
    ## # ... with 208 more rows

**Because of this, the following analysis will be focused on answering a
revised research question - i.e. Have the maximum and minimum flow
events increased in magnitude since 1909?**

### Summarizing

To achieve this, I decided to do the following *summarizing tasks* for
this research question:

**Create a categorical variable with 3 or more groups from an existing
numerical variable. You can use this new variable in the other tasks! An
example: age in years into “child, teen, adult, senior”.**

and

**Based on two categorical variables, calculate two summary statistics
of your choosing:**

Firstly, new categorical data will be created - decade - based on
numerical variable “year”. Next, based on the categorical variables
“decade” and “extreme_type”, the range and mean of the flow will be
derived using summary statistics.By looking at the range and mean of the
maximum and minimum flow events per each of the decade, I should be able
to conclude whether the magnitude of extreme flow events changed over
time.

As have been outlined before, I will firstly create a new categorical
variable “decade” which will hold information on which decade the data
entries correspond to:

``` r
#creating new column "decade" based on the year:
flow_sample_decades <- flow_sample %>%
  mutate(decade = case_when(year<1920 ~ "1909-1919",
                          year<1930 ~ "1920-1929",
                          year<1940 ~ "1930-1939",
                          year<1950 ~ "1940-1949",
                          year<1960 ~ "1950-1959",
                          year<1970 ~ "1960-1969",
                          year<1980 ~ "1970-1979",
                          year<1990 ~ "1980-1989",
                          year<2000 ~ "1990-1999",
                          year<2010 ~ "2000-2009",
                          year<2019 ~ "2010-2018"))

#Checking the result:
head(flow_sample_decades)
```

    ## # A tibble: 6 x 8
    ##   station_id  year extreme_type month   day  flow sym   decade   
    ##   <chr>      <dbl> <chr>        <dbl> <dbl> <dbl> <chr> <chr>    
    ## 1 05BB001     1909 maximum          7     7   314 <NA>  1909-1919
    ## 2 05BB001     1910 maximum          6    12   230 <NA>  1909-1919
    ## 3 05BB001     1911 maximum          6    14   264 <NA>  1909-1919
    ## 4 05BB001     1912 maximum          8    25   174 <NA>  1909-1919
    ## 5 05BB001     1913 maximum          6    11   232 <NA>  1909-1919
    ## 6 05BB001     1914 maximum          6    18   214 <NA>  1909-1919

Next, I will compute summary statistics of flow based on the categorical
variables “extreme_type” and “decade”. Since the analysis will be
focused on categorical variables “extreme_type” and “decade”, as well as
the numerical variable “flow”, it is important to remove missing values
from the corresponding column that have missing values. Newly created
column “decade” does not have missing values, because the column years
did not have missing values (as was evident in Milestone 1). Hence,
missing values have to be removed only for variables “extreme_type” and
“flow”

``` r
#Filtering out missing data from the column "extreme_type" and "flow" and storing the result in a new dataframe called "flow_q1" (which stands for flow_question1)
flow_q1 <- flow_sample_decades %>%
  filter(!is.na(extreme_type), !is.na(flow))
```

Next, the summary statistics of numerical variable “flow” based on
categorical variables “extreme_type” and “decade” shall be calculated.

``` r
#Calculating summary statistics:
flow_q1_summary <- flow_q1 %>%
  group_by(decade,extreme_type) %>%
  summarize(flow_mean = mean(flow), flow_range = range(flow))
```

    ## `summarise()` has grouped output by 'decade', 'extreme_type'. You can override using the `.groups` argument.

``` r
print(flow_q1_summary)
```

    ## # A tibble: 44 x 4
    ## # Groups:   decade, extreme_type [22]
    ##    decade    extreme_type flow_mean flow_range
    ##    <chr>     <chr>            <dbl>      <dbl>
    ##  1 1909-1919 maximum         243.       174   
    ##  2 1909-1919 maximum         243.       345   
    ##  3 1909-1919 minimum           6.15       4.56
    ##  4 1909-1919 minimum           6.15       7.16
    ##  5 1920-1929 maximum         224.       130   
    ##  6 1920-1929 maximum         224.       377   
    ##  7 1920-1929 minimum           5.98       4.9 
    ##  8 1920-1929 minimum           5.98       6.57
    ##  9 1930-1939 maximum         227.       148   
    ## 10 1930-1939 maximum         227.       311   
    ## # ... with 34 more rows

This summary stastics helps answering the question on whether the
magnitude of extreme flow events have increased over time by providing
insight on the mean and range of flow by decade. However, for more
effective analysis, this data should be visualized.

### Graphing

I decided to do the following tasks for this research question:

**Create a graph out of summarized variables that has at least two geom
layers.**

and

**Create a graph of your choosing, make one of the axes logarithmic, and
format the axes labels so that they are “pretty” or easier to read.**

By plotting mean and range of flow per decade as two separate geom layer
on a single graph, one should be able to see whether there have been
changes in the magnitude of flow over time.

The following code will create a graph showing: 1. the range of minimum
and maximum extreme flow events per each of the decades (*first geom
layer*), where the two types of extreme flow events will be represented
by different colors 2. the mean of the minimum and maximum extreme flow
events per each of the decade (*second geom layer*), where the two types
of extreme flow events will be represented by different colors.

Logarithmic scale is utilized to ensure that the range of both minimum
and maximum flow events is clearly visible.

``` r
flow_q1_figure <- flow_q1_summary %>%
  ggplot(aes(decade, flow_range))+
  geom_point(aes(color=extreme_type))+ #first geom layer
  geom_point(shape=8,aes(decade, flow_mean, color=extreme_type))+ #second geom layer
  scale_y_log10()+ #logarithmic scale
  labs(y="Streamflow (m3/s)", x="Decades", colour="Extreme Type")+ #changing the labels
  coord_flip() #flipping coordinates for better visual representation

#NOTE: units of measurement of flow were obtained from https://www.for.gov.bc.ca/hfd/library/documents/bib100015.pdf

print(flow_q1_figure)
```

![](mini-data-analysis-2_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

The figure suggest that magnitude (range of values) of maximum flow
events have increased since 1909, with the greatest range observed in
2010-2018. As for the magnitude (range) of minimum flow events, however,
no significant trend can be observed. Mean values show fluctuations
between the decades for both maximum and minimum flow decades. However,
no significant trend (e.g. increase or decrease over time) can be
derived.

**To summarize the findings, the magnitude of maximum flow events have
increased since 1909, while minimum remained approximately the same
albeit some fluctuations between the decades. No significant change in
the mean of maximum or minimum flow events have been detected. This
answers the research question.**

## Question 2: When did the absolute maximum and minimum magnitude of the flow occur?

### Summary

To approach this question, I will do the following task:

**Compute the range, mean, and two other summary statistics of one
numerical variable across the groups of one categorical variable from
your data.**

Based on the categorical variable “extreme type”, a summary statistics
will be computed including:

1.  mean

2.  range

3.  median

4.  standard deviation

5.  maximum

6.  minimum

7.  number of observations

This will, firstly, allow to determine absolute maximum and minimum
value of flow on record in this dataset, which is a first step to
determining when such event have occurred.

Secondly, it will improve my understanding of the data - i.e. is
standard deviation large and how does standard deviation of maximum and
minimum flow events compare, is the mean and median of each type of
extreme events close (are the two distributions symmetrical)?

The following code will compute summary statistics of flow grouped by
the categorical variable “extreme_type”:

``` r
#variable flow_q1 is used as it doesn't contain missing values in extreme_type and flow variables. 

flow_q2_summary <- flow_q1 %>%
  group_by(extreme_type) %>% #grouping by extreme_type
  summarize(flow_mean = mean(flow), flow_range = range(flow), flow_median =median(flow), flow_sd = sd(flow), flow_max = max(flow), flow_min = min(flow), n=n())
```

    ## `summarise()` has grouped output by 'extreme_type'. You can override using the `.groups` argument.

``` r
print(flow_q2_summary)
```

    ## # A tibble: 4 x 8
    ## # Groups:   extreme_type [2]
    ##   extreme_type flow_mean flow_range flow_median flow_sd flow_max flow_min     n
    ##   <chr>            <dbl>      <dbl>       <dbl>   <dbl>    <dbl>    <dbl> <int>
    ## 1 maximum         212.       107         204     61.7     466      107      109
    ## 2 maximum         212.       466         204     61.7     466      107      109
    ## 3 minimum           6.27       3.62        6.15   0.965     8.44     3.62   107
    ## 4 minimum           6.27       8.44        6.15   0.965     8.44     3.62   107

As can be seen from the table, the absolute minimum value of flow is
3.62, while the absolute maximum value is 466. The mean and the medium
is quite close for both maximum and minimum extreme events, hence the
distribution of each of them is likely symmetrical. However, based on
standard deviation, the spread of the values for maximum flow events is
significantly larger than for minimum flow events.

Computing summary statistics is a first step to answering the research
question. However, after determining the value of absolute minimum and
maximum flow, future analysis need to be conduced to determine when
these events have occurred.

### Graphing

To visualize data for this research question, the following tasks will
be completed:

**Make a graph where it makes sense to customize the alpha
transparency.**

and

**Create a graph out of summarized variables that has at least two geom
layers.**

In the following, two graphs will be created:

1.  a graph showing all the data points of “flow” over time. This will
    allow to determine in which year or decade the absolute minimum and
    maximum flow have occurred.

2.  a graph showing all the data points of “flow” sorted by month. This
    will allow to determine in which month the absolute maximum and
    minimum flow have occurred.

#### Graph1

To plot the first graph, the new column should be created with the date
of each flow event (combining year, month, and day).

``` r
#In the following the rows with missing values of month, year, and day will be filtered out, and new column holding date will be created. Variable flow_q1 is used as it doesn't contain missing values in extreme_type and flow variables :

flow_date <- flow_q1 %>%
   filter(!is.na(month), !is.na(year), !is.na(day)) %>%
  mutate(date = mdy(paste(flow_q1$month, flow_q1$day, flow_q1$year, sep=" ")))
```

Next, the graph can be plotted. The logarithmic scale will be used so
that the variations in both minimum and maximum flow events are
represented. Minimum and maximum flows will be represented in different
colors. *The transparency will be adjusted due to the fact that some of
the points overlap*.

``` r
#creating the figure
flow_date_figure <- flow_date %>%
  ggplot(aes(date, flow))+
  geom_point(aes(color=extreme_type),alpha=0.5, size=2)+
  scale_y_log10()

print(flow_date_figure)
```

![](mini-data-analysis-2_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->

Presented graph shows the maximum and minimum extreme flow events that
have occurred every year. The minimum and maximum extreme events are
shown in different colors. From the graph, one can derive that the
minimum extreme flow occurred around 1932, while the maximum was
observed around 2014.

#### Graph2

The second graph will show the flow data sorted by month.

To achieve this, firstly, missing values will be filtered out for the
columns “flow”, “extreme_type”, and “month” since the analysis are
focused on these values:

``` r
flow_q2 <- flow_sample %>%
  filter(!is.na(extreme_type), !is.na(flow), !is.na(month))
```

Next, the variable “month” will be transformed into a categorical
variable:

``` r
flow_q2$month=as.factor(flow_q2$month)
```

Next, the data will be plotted as jitter plot and boxplot (*The graph
will consist of two geom layers*). The logarithmic scale will be used so
that the variations in both minimum and maximum flow events are
represented. *The transparency will be adjusted due to the fact that
some of the points overlap*.

``` r
flow_month_figure <- flow_q2 %>%
  ggplot(aes(month, flow))+ #plotting variables month and flow
  geom_boxplot()+ #geom layer 1
  geom_jitter(width=0.2, alpha=0.2, size=2)+ #geom layer 2
  scale_y_log10() #adding logarithmic scale

print(flow_month_figure)
```

![](mini-data-analysis-2_files/figure-gfm/unnamed-chunk-13-1.png)<!-- -->

Presented graph shows the values of flow per months. The data encompass
all of the years in the dataset. The boxplots show the median, lower and
upper quantile, minimum, maximum, and outliers. As can be seen from the
graph, the absolute minimum flow occurred in January, while the absolute
maximum flow occurred in June. Moreover, it can be derived that the
former is an outlier for that month, while the latter is within the
minimum range (upper quntile + 1.5IQR).

**To summarize the findings for this research question, the absolute
minimum flow (3.62 m3/s) occurred in January, most probably in 1932. The
absolute maximum flow (466 m3/s) was observed in June around year 2014.
While this partially answers the research question, it would be helpful
to conduct further analysis to determine the precise date of both of
these events.**

## Question 3: Does the annual distribution of the extreme flow events changed over time? For example, were most of the minimum/maximum that have occurred in past 10 years observed in a similar time than those in 1909-1919 or have the pattern shifted?

### Summary

To make a progress towards answering this question, the following task
will be completed:

**Compute the number of observations for at least one of your
categorical variables.**

Computing a number of observations of maximum and minimum extreme
weather events for decades “1909-1919” and “2000-2018” will help to
determine whether there is a sufficient number of data points for future
analysis, which will focus on comparing these two decades.

Hence, firstly, a subset will be created with only the decades in
question. *flow_q1* dataframe will be used as a basis for the subset, as
it does not contain missing values for variables “flow” and
“extreme_type”, which will be the focus of analysis in this research
question.

``` r
#creating a subset which only contains two decades in question and missing data
flow_q3 <- flow_q1 %>%
  filter(decade == c("1909-1919", "2010-2018"))
```

Computing the number of observations for each of the two decades and
each type of extreme weather events to see whether they are comparable:

``` r
flow_q3_summary <- flow_q3 %>%
  group_by(decade, extreme_type) %>% 
  summarize(n=n())
```

    ## `summarise()` has grouped output by 'decade'. You can override using the `.groups` argument.

``` r
print(flow_q3_summary)
```

    ## # A tibble: 4 x 3
    ## # Groups:   decade [2]
    ##   decade    extreme_type     n
    ##   <chr>     <chr>        <int>
    ## 1 1909-1919 maximum          6
    ## 2 1909-1919 minimum          4
    ## 3 2010-2018 maximum          4
    ## 4 2010-2018 minimum          4

As can be seen from the tibble, the number of observations are
relatively low - i.e. 4-6 observations per extreme_type per decade.
However, it should be sufficient for the purpose of this analysis.

### Graphing

To further explore the differences between the extreme flow events
observed in 1909-1919 and those in 2010-2018, the following task will be
completed:

**Create a graph out of summarized variables that has at least two geom
layers**

Two graphs will be created - one for minimum and one for maximum
extreme_type - showing the timing of extreme weather events in both of
the decades. Timing is defined as a day of the year when the extreme
event have occurred. Moreover, the mean of flow and timing for each
decade will be plotted to enable comparison.

To achieve this, firstly, a column will be added to the subset with the
information on the day of the year in which the extreme flow have
occurred:

``` r
#The following code filters out missing values of month, year, and day, as well as creates a new column which hold which day of the year an extreme flow event corresponds to:

flow_q3_daynumber <- flow_q3 %>%
   filter(!is.na(month), !is.na(year), !is.na(day)) %>%
   mutate(day_number = yday(mdy(paste(flow_q3$month, flow_q3$day, flow_q3$year, sep=" "))))

#checking the result:
print(flow_q3_daynumber)
```

    ## # A tibble: 18 x 9
    ##    station_id  year extreme_type month   day   flow sym   decade    day_number
    ##    <chr>      <dbl> <chr>        <dbl> <dbl>  <dbl> <chr> <chr>          <dbl>
    ##  1 05BB001     1909 maximum          7     7 314    <NA>  1909-1919        188
    ##  2 05BB001     1911 maximum          6    14 264    <NA>  1909-1919        165
    ##  3 05BB001     1913 maximum          6    11 232    <NA>  1909-1919        162
    ##  4 05BB001     1915 maximum          6    27 236    <NA>  1909-1919        178
    ##  5 05BB001     1917 maximum          6    17 174    <NA>  1909-1919        168
    ##  6 05BB001     1919 maximum          6    22 185    <NA>  1909-1919        173
    ##  7 05BB001     2010 maximum          6    30 138    <NA>  2010-2018        181
    ##  8 05BB001     2012 maximum          6     7 332    <NA>  2010-2018        159
    ##  9 05BB001     2014 maximum          6    26 178    <NA>  2010-2018        177
    ## 10 05BB001     2016 maximum          6     8 107    <NA>  2010-2018        160
    ## 11 05BB001     1912 minimum          3    14   5.8  <NA>  1909-1919         74
    ## 12 05BB001     1914 minimum         11    17   7.16 <NA>  1909-1919        321
    ## 13 05BB001     1916 minimum          3     2   6.97 B     1909-1919         62
    ## 14 05BB001     1918 minimum          2    20   6.03 B     1909-1919         51
    ## 15 05BB001     2011 minimum          1    30   7.13 B     2010-2018         30
    ## 16 05BB001     2013 minimum          1     2   5.31 B     2010-2018          2
    ## 17 05BB001     2015 minimum          3     4   6.59 B     2010-2018         63
    ## 18 05BB001     2018 minimum          3    31   6.77 B     2010-2018         90

Next, two new dataframes will be created which will hold the mean flow
and mean timing (day of the year) when extreme_type events occur for
both of the decades in question. One dataframe will hold information for
*maximum flow events*, while another will consist only of *minimum flow
events*. Creating two dataframes (as opposite to one) will enable to
plot the maximum and minimum flow events in two separate graphs.

``` r
#The following code (1) filters out minimum extreme type events, (2) selects columns of interest - i.e. day_number, flow, decade, (3) groups the data by decade, (4) and computes the mean of the day_number and flow per each of the decade:

flow_q3_summary_max <- flow_q3_daynumber %>% 
        filter(extreme_type=="maximum") %>%
        select(day_number, flow, decade) %>%
        group_by(decade) %>%
        summarise(day_number = mean(day_number),
                  flow  = mean(flow))
print(flow_q3_summary_max)
```

    ## # A tibble: 2 x 3
    ##   decade    day_number  flow
    ##   <chr>          <dbl> <dbl>
    ## 1 1909-1919       172.  234.
    ## 2 2010-2018       169.  189.

``` r
#The following code (1) filters out maximum extreme type events, (2) selects columns of interest - i.e. day_number, flow, decade, (3) groups the data by decade, (4) and computes the mean of the day_number and flow per each of the decade:

flow_q3_summary_min <- flow_q3_daynumber %>% 
        filter(extreme_type=="minimum") %>%
        select(day_number, flow, decade) %>%
        group_by(decade) %>%
        summarise(day_number = mean(day_number),
                  flow  = mean(flow))
print(flow_q3_summary_min)
```

    ## # A tibble: 2 x 3
    ##   decade    day_number  flow
    ##   <chr>          <dbl> <dbl>
    ## 1 1909-1919      127    6.49
    ## 2 2010-2018       46.2  6.45

The following code will create a plot which will show on which days of
the year maximum flow occurred in 1909-1919 versus 2010-2018 (**geom
layer 1**). Moreover, the mean of flow and day of the year will be
plotted to enable comparison (**geom layer 2**). Minimum and maximum
flow events, as well as their associated mean values, will be
represented by different colors.

``` r
flow_q3_plot_max <-flow_q3_daynumber %>%
  filter(extreme_type == "maximum")%>% #filtering out minimum flow events
  ggplot(aes(day_number, flow))+ 
  geom_point(aes(color=decade))+ #geom layer 1
  geom_point(data=flow_q3_summary_max, size=6, aes(color=decade),shape=8) #layer 2
  #adding the mean for each decade, ensuring that it is represented with a different shape and that decades are color-coded similarly to the previous layer

print(flow_q3_plot_max)
```

![](mini-data-analysis-2_files/figure-gfm/unnamed-chunk-18-1.png)<!-- -->

As can be seen from the graph, maximum flow occurred on average about 4
days earlier in 2010-2018 than in 1909-1919. This may imply that there
was a change in the pattern over this period of time (e.g. due to
changing climate)

The same graph will be created for minimum flow events:

``` r
flow_q3_plot_min <-flow_q3_daynumber %>%
  filter(extreme_type == "minimum")%>% #filtering out maximum flow
  ggplot(aes(day_number, flow))+
  geom_point(aes(color=decade))+ #layer1
  geom_point(data=flow_q3_summary_min, size=6, aes(color=decade),shape=8) #layer2
print(flow_q3_plot_min)
```

![](mini-data-analysis-2_files/figure-gfm/unnamed-chunk-19-1.png)<!-- -->

As can be seen from the graph, the timing of the minimum flow have
changed significantly. While in 1909-1919 minimum flow was observed on
average on about 125th day of the year, in 2010-2018, it shifted to
about 50th day. However, by looking at the graph, one should note that
one of the minimum flow events in 1909-1919 decade occurred in December,
while none of the 2010-2018 occurred in December. This might have skewed
the results for 1909-1919 decade hence undermining the analysis.

Finally it’s important to note that the number of datapoints for each
type of extreme events and each decade is extremely small. To ensure
more reliable results, one may consider comparing larger periods of
time, e.g. 1909-1929 and 2000-2018.

**To conclude, the presented analysis have successfully answered the
research question: The mean of the maximum flow events have shifted
between 1909-1919 and 2010-2018, with the later events occurring on
average earlier in a year. The minimum flow events were also observed
earlier in a year in 2010-2018 decade, as compared 1909-1919.**

## Question 4: Have the number of data entries that are estimated, have significant gaps during the day, or have been affected by the presence of ice (sym=E, sym=A, and sym=B, respectively) decreased over time?

Before approaching the research question, some theoretical background
need to be provided on the meaning of different symbols.

The website of Government of Canada (Water Level and Flow)
<https://wateroffice.ec.gc.ca/contactus/faq_e.html#Q12> outlines the
meaning of each symbols:

-   E. The symbol E indicates that there was no measured data available
    for the day or missing period, and the water level or streamflow
    value was estimated by an indirect method such as interpolation,
    extrapolation, comparison with other streams or by correlation with
    meteorological data.

-   A. The symbol A indicates that the daily mean value of water level
    or streamflow was estimated despite gaps of more than 120 minutes in
    the data string or missing data not significant enough to warrant
    the use of the E symbol.

-   B. The symbol B indicates that the streamflow value was estimated
    with consideration for the presence of ice in the stream. Ice
    conditions alter the open water relationship between water levels
    and streamflow.

-   D. The symbol D indicates that the stream or lake is “dry” or that
    there is no water at the gauge. This symbol is used for water level
    data only.

-   R. The symbol R indicates that a revision, correction or addition
    has been made to the historical discharge database after January
    1, 1989.

This information is important for future analysis of data.

### Summarizing

To address the research question, the following task was chosen:

**Compute the number of observations for at least one of your
categorical variables. Do not use the function table()!**

Specifically, the number of observations of each of the categories of
sym per decade will be computed.

Before conducting the summary statistics, the rows with missing symbols
will be removed. An assumption is made that missing symbol means lack of
any of the outlined above conditions. It is important to note, however,
that this might not be the case and there could be other reasons for
gaps in data. Hence, large number of missing data for “sym” variable can
constitute a limitation.

The following code will filtering out missing values and selecting only
the columns of interest (sym and decade):

``` r
flow_sym <- flow_q1 %>% #flow_q1 is used because it stores the "decade" column
  select(decade,sym) %>% #selecting variables in question
  filter(!is.na(sym)) #removing rows with missing data in "sym" column
```

Next, summary statistics will be computed showing the number of
observations of each category of sym per each of the decades.

``` r
flow_sym_summary <- flow_sym %>%
  group_by(decade, sym) %>%
  summarize(n=n())
```

    ## `summarise()` has grouped output by 'decade'. You can override using the `.groups` argument.

``` r
print(flow_sym_summary)
```

    ## # A tibble: 15 x 3
    ## # Groups:   decade [11]
    ##    decade    sym       n
    ##    <chr>     <chr> <int>
    ##  1 1909-1919 B         5
    ##  2 1920-1929 B         8
    ##  3 1920-1929 E         1
    ##  4 1930-1939 B        10
    ##  5 1940-1949 B         8
    ##  6 1950-1959 B         8
    ##  7 1960-1969 B         9
    ##  8 1970-1979 B        10
    ##  9 1980-1989 A         1
    ## 10 1980-1989 B         9
    ## 11 1980-1989 E         1
    ## 12 1990-1999 A         1
    ## 13 1990-1999 B        10
    ## 14 2000-2009 B        10
    ## 15 2010-2018 B         8

The presented table shows the number of observations of different
symbols per decade. As can be derived from the table, some of the
symbols have been observed in just a few decades - e.g. sym “E” was only
observed once in decade 1920-1929 and once in 1980-1989. Similarly sym
“A” was recorded only for decades 1990-1999 and 1980-1989 and have only
appeared once in both of these decades. The most popular symbol is “B”,
which appeared in every decade represented in the dataset.

### Graphing

**As for the graphing, none of the proposed tasks could be applied for
this research question. However, the data in question could be
effectively represented using a bar chart, which will be computed in the
following.**

``` r
#The following code plots a bar chart showing the number of symbols for each of the decades, where different symbols are represented by different colors. 
#The coordinates are flipped for better visual representation (to avoid overlapping text on the x-axis).
#The labels include a short explanation of the meaning of each of the symbol

flow_sym_plot <-flow_sym %>% 
  ggplot(aes(decade))+
  geom_bar(aes(fill=sym))+ 
  coord_flip()+
  scale_fill_discrete(name="Symbol",
                      breaks=c("A", "B", "E"),
                      labels=c("A (significant daily gaps)", "B (ice)",
                               "E(estimates for missing data)"))

#Created bar chart, however, starts at the latest decade and ends at the oldest, which may not be the most effective way to visualize data. 

#The following code will flip the categories, so that the graph starts at the oldest decade. The solution was inspired by the post under the following link: https://community.rstudio.com/t/reverse-order-of-categorical-y-axis-in-ggridges-ggplot2/2273/2

flow_sym_plot2 <- flow_sym_plot + scale_x_discrete(limits = rev(unique(sort(flow_sym$decade)))) #while decades are represented on the y-axis, scale_x_discrete is used, because the coordinates were previously flipped and the decades are still regarded as x-axis.

print(flow_sym_plot2)
```

![](mini-data-analysis-2_files/figure-gfm/unnamed-chunk-22-1.png)<!-- -->
As can be seen from the graph, in past two decades, there have not been
any data with significant daily gaps (A) or where the data was missing
entirely and had to be estimated (E). This may be due to improvement in
the recording instruments. However, it is important to note that no A or
E symbols appeared between 1930s-1980s. Hence it cannot be conclusively
derived whether there have been a significant improvement and no missing
data will occur in the future.

Moreover, as can be seen from the graph, there are fluctuations in the
number of “B” symbols which indicate that the streamflow value was
estimated with consideration for the presence of ice in the stream.
While no specific trend can be derived, it can be assumed that the
decrease in the number of “B” symbols in last decade (2010-2018) could
be attributed to either lower presence of ice due to climate change or
the fact that 2010-2018 category has one fewer year as compared to other
categories.

**To conclude, the analysis was able to successfully answer question 1.
No significant change has been found in the number of data entries that
are estimated, have significant gaps during the day, or affected by the
presence of ice (sym=E, sym=A, and sym=B, respectively).**

<!----------------------------------------------------------------------------->

### 1.3 (2.5 points)

<!------------------------- Write your answer here ---------------------------->

**I have managed to make a considerable progress in answering my
research questions.** I was able to successfully answer questions 1, 2,
and 4 (the answers that I have obtained can be found in the
“conclusions” paragraph in the corresponding sections). Question 3, on
the other hand, have not been answered fully. While I was able to
determine the month when the absolute maximum and minimum extreme flow
have occurred, I could only estimate the year based of the graph. Hence,
in the future analysis, I would have focus on determining an exact date
when these two events have occurred.

I find that **Question 1 and Question 3 yielded the most interesting
result**. It was interesting to observe that the magnitude of the
maximum flow events have increased significantly over time, which could
potentially be attributed to climate change and associated increase in
the extreme weather events such as flash floods. Moreover, Question 3
confirmed my pre-existing assumption that the annual distribution of the
extreme flow events (particularly maximum flow) could have shifted over
time, which can also be attributed to climate change and changing
seasonal patters.

However, as have been previously noted, comparing the annual
distribution of the extreme flow events that were observed in 1909-1919
decade and 2010-2018 decade may be associated with limitations due to
limited number of observations for each of the decade (only 4-6
observations per decade per each type of the extreme flow events).
Hence, it may be valuable to compare 1909-1929 and 2000-2018, or
1909-1939 and 1990-2019 (two or three decades on each end of the
dataset, respectively).

Moreover, I found it interesting that in Question 1, no significant
change in mean of maximum flow was found between the decades, while
there have been a significant difference in range. Perhaps, there could
have been a change in the pattern of the maximum flow events that
happened in past 5 years, and hence might have not been adequately
represented in the decadal mean.

An increase in the flow value of maximum events can be seen from the
following graph:

``` r
print(flow_date_figure)
```

![](mini-data-analysis-2_files/figure-gfm/unnamed-chunk-23-1.png)<!-- -->

Hence, to further explore this topic, I would like to fit a model in the
data of maximum flow events to explore whether there have been any
significant changes in this type of the flow

Hence, I would like to continue working on the following research
questions:

1.  **Have the maximum flow events exhibited a statistically significant
    increase in magnitude since 1909?** Magnitude is defined as a value
    of flow, as opposed to an increase in frequency. This question can
    be approached by fitting model in data.

2.  **On what exact date did the absolute maximum and minimum magnitude
    of the flow occur?** This question is included since the answer was
    not researched in the presented analysis.

3.  **Is there a significant difference in the annual distribution of
    the extreme flow events between 1909-1929 and 2000-2018?**

NOTE: I don’t think there is a way to refine Question 4, as it have been
fully answered and it did not yield any interesting results.

<!----------------------------------------------------------------------------->

# Task 2: Tidy your data (12.5 points)

### 2.1 (2.5 points)

<!--------------------------- Start your work below --------------------------->

Since the tidyness of data depend on the research question, I will be
tidying/untidying data specifically for question 1 - i.e. *Have the
maximum and minimum flow events increased in magnitude since 1909?*

To determine whether the data is tidy, it may be valuable to first look
at the dataset:

``` r
head(flow_sample)
```

    ## # A tibble: 6 x 7
    ##   station_id  year extreme_type month   day  flow sym  
    ##   <chr>      <dbl> <chr>        <dbl> <dbl> <dbl> <chr>
    ## 1 05BB001     1909 maximum          7     7   314 <NA> 
    ## 2 05BB001     1910 maximum          6    12   230 <NA> 
    ## 3 05BB001     1911 maximum          6    14   264 <NA> 
    ## 4 05BB001     1912 maximum          8    25   174 <NA> 
    ## 5 05BB001     1913 maximum          6    11   232 <NA> 
    ## 6 05BB001     1914 maximum          6    18   214 <NA>

To answer the research question 1, the **observation** would be a flow
value of the maximum and minimum flow events per each of the years. In
the presented dataset, such observations correspond to the rows of the
dataset.

The **variables** of interest - i.e. year, extreme_type, and flow - are
represented as column of the dataset.

Finally, each of the cell represent the **value** corresponding to these
variables - values that can be used to answer the research question.

**Therefore, it may be concluded that the dataset is in a tidy form for
this research question.**

<!----------------------------------------------------------------------------->

### 2.2 (5 points)

<!--------------------------- Start your work below --------------------------->

#### Untidying data

In the following section, I will untidy the dataset. Specifically, I
will create new columns from the categorical variable “extreme_type”,
which will contain values of flow.

It is important to note, however, that there might be problems tidying
this data later. This is due to the fact that when I will re-create
variable “extreme_type”, it will consist of the double the amount of
rows. For example, my original dataset has a row that shows that on 7th
day of 7th months of 1909, there was a **maximum** extreme type of event
recorded with a flow value of 314. After, I will untidy the dataset by
putting “maximum” and “minimum” categories as separate variables/columns
and tidy it again, I will have one more column created per each
observation. For example, I will have another row that shows that on 7th
day of 7th months of 1909, there was a **minimum** extreme type of event
recorded with a missing value. This is due to the fact that maximum and
minimum extreme type events cannot occur on the same date. This could be
fixed by removing rows where the flow value is missing (NA). However, if
the initial dataset had missing values of flow, they will be removed as
well. To avoid that, before untidying my data I will first replace
missing values in the column “flow” with the string “missing”. When I
will be tidying data, this will allow to differentiate between the rows
with missing flow value due to the tidying/untidying and due to gaps in
the original dataset. Therefore, loss of data will be prevented.

The following code will create a new variable - **flow_sample1** - and
substitute all the missing values in column “flow” with the string
“missing”. New variable was created to leave the original dataset
intact.

``` r
#Creating a copy of the dataset:
flow_sample1 <- flow_sample

#The following code substitutes missing values of flow with "missing":
flow_sample1$flow[is.na(flow_sample1$flow)] <- "missing"
```

Now, the dataset shall be untidied by creating new columns for each type
of the extreme type, which will hold the values of flow:

``` r
flow_sample_untidy <- flow_sample1 %>%
  pivot_wider(id_cols = c(-extreme_type, -flow), 
                names_from = extreme_type,
                values_from = flow)
print(flow_sample_untidy)
```

    ## # A tibble: 218 x 7
    ##    station_id  year month   day sym   maximum minimum
    ##    <chr>      <dbl> <dbl> <dbl> <chr> <chr>   <chr>  
    ##  1 05BB001     1909     7     7 <NA>  314     <NA>   
    ##  2 05BB001     1910     6    12 <NA>  230     <NA>   
    ##  3 05BB001     1911     6    14 <NA>  264     <NA>   
    ##  4 05BB001     1912     8    25 <NA>  174     <NA>   
    ##  5 05BB001     1913     6    11 <NA>  232     <NA>   
    ##  6 05BB001     1914     6    18 <NA>  214     <NA>   
    ##  7 05BB001     1915     6    27 <NA>  236     <NA>   
    ##  8 05BB001     1916     6    20 <NA>  309     <NA>   
    ##  9 05BB001     1917     6    17 <NA>  174     <NA>   
    ## 10 05BB001     1918     6    15 <NA>  345     <NA>   
    ## # ... with 208 more rows

This dataset is not tidy for research question 1, because **each row
does not represent represent each instance of observation** - i.e. a
flow value of the maximum and minimum flow events per each of the years.

#### Tidying data

Next, I will tidy the date by creating the column “extreme_type”, which
will store information on whether a certain recorded flow belonged to
“minimum” or “maximum” category:

``` r
flow_sample_tidy <- flow_sample_untidy %>%
  pivot_longer(cols = c(-station_id, -year, -month, -day, -sym),
               names_to = 'extreme_type',
               values_to = 'flow')
print(flow_sample_tidy)
```

    ## # A tibble: 436 x 7
    ##    station_id  year month   day sym   extreme_type flow 
    ##    <chr>      <dbl> <dbl> <dbl> <chr> <chr>        <chr>
    ##  1 05BB001     1909     7     7 <NA>  maximum      314  
    ##  2 05BB001     1909     7     7 <NA>  minimum      <NA> 
    ##  3 05BB001     1910     6    12 <NA>  maximum      230  
    ##  4 05BB001     1910     6    12 <NA>  minimum      <NA> 
    ##  5 05BB001     1911     6    14 <NA>  maximum      264  
    ##  6 05BB001     1911     6    14 <NA>  minimum      <NA> 
    ##  7 05BB001     1912     8    25 <NA>  maximum      174  
    ##  8 05BB001     1912     8    25 <NA>  minimum      <NA> 
    ##  9 05BB001     1913     6    11 <NA>  maximum      232  
    ## 10 05BB001     1913     6    11 <NA>  minimum      <NA> 
    ## # ... with 426 more rows

As have been mentioned before, the obtained dataset has double the
number of rows due to the fact that two extreme type events (maximum and
minimum) could not have occurred on the same day, month, and year.

To return it to its original (“tidy”) version, the missing values of
flow that were created due to tidying/untidying shall be removed.
Following that I will substitute the values of flow that were missing in
the original dataset with logical value. This way no data will be lost.

``` r
#Removing rows where flow value is missing:
flow_sample_tidy_clean <- flow_sample_tidy %>%
  filter(!is.na(flow))
  

#Substituting missing values from the original dataset back with logical value:
flow_sample_tidy_clean$flow[flow_sample_tidy_clean$flow == "missing"] <- NA


print(flow_sample_tidy_clean)
```

    ## # A tibble: 218 x 7
    ##    station_id  year month   day sym   extreme_type flow 
    ##    <chr>      <dbl> <dbl> <dbl> <chr> <chr>        <chr>
    ##  1 05BB001     1909     7     7 <NA>  maximum      314  
    ##  2 05BB001     1910     6    12 <NA>  maximum      230  
    ##  3 05BB001     1911     6    14 <NA>  maximum      264  
    ##  4 05BB001     1912     8    25 <NA>  maximum      174  
    ##  5 05BB001     1913     6    11 <NA>  maximum      232  
    ##  6 05BB001     1914     6    18 <NA>  maximum      214  
    ##  7 05BB001     1915     6    27 <NA>  maximum      236  
    ##  8 05BB001     1916     6    20 <NA>  maximum      309  
    ##  9 05BB001     1917     6    17 <NA>  maximum      174  
    ## 10 05BB001     1918     6    15 <NA>  maximum      345  
    ## # ... with 208 more rows

The resulting dataframe is tidy. The reasons have been explained in 2.1
(I have re-created the initial dataset).

<!----------------------------------------------------------------------------->

### 2.3 (5 points)

<!--------------------------- Start your work below --------------------------->

In the future analysis, I would like to explore the following research
questions:

1.  **Have the maximum flow events exhibited a statistically significant
    increase in magnitude since 1909?** Magnitude is defined as a value
    of flow, as opposed to an increase in frequency. This question can
    be approached by fitting model in data.

2.  **Is there a significant difference in the annual distribution of
    the extreme flow events between 1909-1929 and 2000-2018?**

I have chosen these questions, because I believe they can yield most
interesting results. I think it is valuable to further explore changes
in maximum flow events over time (by fitting models), as it can provide
insights on whether maximum flow have changed in response to the
changing climate.

Similarly, I think it is important to further explore changes in annual
distribution of extreme flow events (taking larger number of observation
by comparing 2 oldest and 2 most recent decades in the dataset). This
may provide additional insight on whether the maximum flow pattern have
shifted due to changing seasonal patters (e.g. major rainfall happening
earlier leading to the extreme flow).

For the purpose of future analysis, I will produce a cleaner version of
the data (stored as **flow_milestone3_data**). This data set:

-   will include only the columns that may be needed to answer the two
    outlined questions

-   will not include rows with missing values in “extreme_type” and
    “flow” variables. The rows where “month” or “day” is missing are not
    removed, because it would limit data available to answer question 1

-   will be aranged by year, since future analysis will be interested in
    the changes in flow over time.

-   will consist of three additional columns - decade, data, and day of
    the year - which will enable future analysis

This will be achieved using the following code:

``` r
flow_milestone3_data <- flow_sample %>%
  
  #Removing the columns that will not be needed:
  select(-station_id, -sym) %>%
  
  #Removing rows with missing values in "extreme_type" and "flow" variables. The rows where "month" or "day" is missing are not removed, because it would limit data available to answer question 1:
  filter(!is.na(extreme_type), !is.na(flow)) %>%
  
  #arranging observations by year, since we are interested in the changes over time (from oldest to recent):
  arrange(year) %>%
  
  #adding columns for decade, date, and day of the year when extreme flow have occured:
  mutate(decade = case_when(year<1920 ~ "1909-1919",
                          year<1930 ~ "1920-1929",
                          year<1940 ~ "1930-1939",
                          year<1950 ~ "1940-1949",
                          year<1960 ~ "1950-1959",
                          year<1970 ~ "1960-1969",
                          year<1980 ~ "1970-1979",
                          year<1990 ~ "1980-1989",
                          year<2000 ~ "1990-1999",
                          year<2010 ~ "2000-2009",
                          year<2019 ~ "2010-2018"), 
         date = mdy(paste(month, day, year, sep=" ")),
         day_number = yday(mdy(paste(month, day, year, sep=" "))))
  
 

head(flow_milestone3_data)
```

    ## # A tibble: 6 x 8
    ##    year extreme_type month   day   flow decade    date       day_number
    ##   <dbl> <chr>        <dbl> <dbl>  <dbl> <chr>     <date>          <dbl>
    ## 1  1909 maximum          7     7 314    1909-1919 1909-07-07        188
    ## 2  1910 maximum          6    12 230    1909-1919 1910-06-12        163
    ## 3  1911 maximum          6    14 264    1909-1919 1911-06-14        165
    ## 4  1911 minimum          2    27   5.75 1909-1919 1911-02-27         58
    ## 5  1912 maximum          8    25 174    1909-1919 1912-08-25        238
    ## 6  1912 minimum          3    14   5.8  1909-1919 1912-03-14         74

<!----------------------------------------------------------------------------->

### Attribution

Thanks to Victor Yuan for mostly putting this together.
