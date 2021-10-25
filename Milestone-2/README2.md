# Milestone 2

## Brief description.

This folder contains files for Milestone 2 of STAT 545 course, which builds on Milestone 1, and aims at further exploring flow_sample dataset and address four research questions posed at the end of the milestone1. For each of the research questions, at least one summary and one graphing exercise is completed.

This folder contains:
1. **mini-data-analysis-2.Rmd** containing the analysis of the selected dataset. The code should be run in R.

2. **mini-data-analysis-2.md** which is knitted version of the mini-data-analysis-2.Rmd

3. **mini-data-analysis-2_files** containing .png files of the figures from the mini-data-analysis-2.md, which allows to correctly display the mini-data-analysis-2.md


## Detailed description

In Milestone 2 of the data analysis, I have:

* explored whether maximum and minimum flow events increased in magnitude since 1909
* explored whether annual distribution of the extreme flow events changed over time by comparing 1909-1919 decade and 2010-2018 decade
* explored when the absolute maximum and minimum magnitude of the flow occurred
* explored whether the number of data entries that are estimated, have significant gaps during the day, or have been affected by the presence of ice decreased over time
* summarized the findings
* practiced tidying and untidying data, producing a tidy dataset
* derived new, revised, research questions
* cleaned the dataset, so that it contains the tidy version of data to answer the new research question and does not contain redundant information
Finally, 4 research questions were derived for future data exploration in the subsequent milestones.


The conclusions reached by the analysis (so far):

* The magnitude of maximum flow events have increased since 1909, while minimum remained approximately the same albeit some fluctuations between the decades. No significant change in the mean of maximum or minimum flow events have been detected. 

* The absolute minimum flow (3.62 m3/s) occurred in January, most probably in 1932. The absolute maximum flow (466 m3/s) was observed in June around year 2014. It might be helpful to conduct further analysis to determine the precise date of both of
these events.

* The timing of the maximum flow events have shifted between 1909-1919 and 2010-2018, with the later events occurring on average earlier in a year. The minimum flow events were also observed earlier in a year in 2010-2018 decade, as compared 1909-1919.

* No significant change has been found in the number of data entries that
are estimated, have significant gaps during the day, or affected by the
presence of ice (sym=E, sym=A, and sym=B, respectively)
