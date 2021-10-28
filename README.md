# Data Analysis Margaryta Pustova

The repository was created for mini-data analysis assignments, which are part of the STAT545 course. The mini-analysis is focused on exploring the dataset from The Government of Canada’s Historical Hydrometric Database. This dataset contains the information on the minimum and maximum flow that have been recorded each year since 1909 at a specific stream. It includes information on the year, month, day, extreme type (e.g. maximum or minimum), flow value, and associated symbol (e.g. "E" for estimated value). The project aims to explore the data in this dataset, as well as to pose and answer research questions based on this data.


**The repository consist of the following folders:**


1. **Milestone-1** containing the first part of the data analysis, which aimed at exploring various available datasets, selecting a dataset for further analysis, as well as getting familiar with the variables of the dataset and their potential relationships. This folder consist of **Mini-data-analysis-1.Rmd**, **Mini-data-analysis-1.md**, and **Mini-data-analysis-1_files**. 


2. **Milestone-2** containing the second part of the data analysis, which aimed at further exploring flow_sample dataset and address four research questions posed at the end of the milestone1. For each of the research questions, at least one summary and one graphing exercise was completed.

3. **Milestone-3** containing the third and final part of the data analysis.In this part, *forecats* and *lubridate* packages were used to transform/create a meaningful plot. Moreover, the linear model is applied to explore whether the maximum flow events exhibited a statistically significant increase in magnitude since 1909. Milestone 3 creates two outputs, as described below.

4. **output** containing the output of the Milestone-3. It consists of **flow_model.rds** file which holds the linear model created in Milestone 3, as well as **flow_q1_summary.csv**, which holds a summary table from Milestone 2 (Exercise 1.2)

**When firstly reading through the data analysis, it is important to read in the correct sequence - i.e. Milestone 1, Milestone 2, and Milestone 3. This is because the Milestones are building on each other: For example, research questions that are posed in one milestone may be answered in other. Moreover, the findings from earlier Milestone can be references in the later Milestones.**
