# Milestone 3

## Brief description.

This folder contains files for Milestone 3 of STAT 545 course, which builds on Milestone 2, and aims at further exploring flow_sample dataset by creating/transforming a plot using forecats and lubridate packages. Moreover, the linear model is applied to explore whether the maximum flow events exhibited a statistically significant increase in magnitude since 1909.



This folder contains:
1. **mini-data-analysis-3.Rmd** containing the analysis of the selected dataset. The code should be run in R.

2. **mini-data-analysis-3.md** which is knitted version of the mini-data-analysis-3.Rmd

3. **mini-data-analysis-3_files** containing .png files of the figures from the mini-data-analysis-3.md, which allows to correctly display the mini-data-analysis-3.md


## Detailed description

In Milestone 3 of the data analysis, I have:

* Built a plot showing the flow values for **minimum** extreme type per each of the months using jitter plot and boxplot (as two geom layers on one plot)

* Reordered the plot, so that months appear in a descending order (based on the number of observations)

* Created a new column which holds information on the week number when each of the minimum flow events occurred

* Plotted the values of minimum flow events with the corresponding week number. 

* Fitted linear models to explore whether the maximum flow events exhibited a statistically significant increase in magnitude since 1909. Retrieved p-value for the model.

* Saved a summary table from Milestone 2 (Exercise 1.2) as .csv in the output folder.

* Saved a linear model as a RDS file in the output folder. 




