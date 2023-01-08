# State Testing Analysis

![Fletcher](/Resources/fletcher.jpg)

### Introduction
The aim of this project is to understand the performance of public school students in Columbia and Montour counties in Pennsylvania.
It was requested by Jeffrey Emanuel, the director of the Foundation of the Columbia Montour Chamber of Commerce.
Our professor Dr. Calhoun, of the Bloomsburg University of Pennsylvania, advises and coordinates with us on this project.
Our team consists of senior data science students [Kadir O. Altunel](https://github.com/KadirOrcunAltunel-zz), [Anna T. Schlecht](https://github.com/atschlecht) and [myself](https://github.com/ronny-phoenix).
There are two major tasks in this project:
- Setup/Data Wrangling - Worked on by me [testing_data.R](/testing_data.R)
- Project Objectives - Worked on by our team [analysis.R](/analysis.R)

### Data Wrangling
The data sets used in this project are the results from the [PSSA](https://www.education.pa.gov/DataAndReporting/Assessments/Pages/PSSA-Results.aspx) and [Keystone](https://www.education.pa.gov/DataAndReporting/Assessments/Pages/Keystone-Exams-Results.aspx) state tests.
These sets contain aggregate data at the state level for both the PSSA and Keystone.
They contain more granular data at the school level including counties, districts and schools.

To wrangle this data and prepare it for analysis I created a script in R [testing_data.R](/testing_data.R) that loads the raw XLSX files and generates 3 CSV files.
The XLSX files are organized by year (2015 - 2022) and level (state, local).
The CSV files generated are:
- [pssa.csv](/PSSA/pssa.csv) - A data set containing the PSSA data at the state and local levels.
- [keyston.csv](/Keystone/keystone.csv) - A data set containing the Keystone data at the state and local levels.
- [cohorts.csv](/Cohorts/cohorts.csv) - A data set containing both PSSA and Keystone data at the local level that follow cohorts.

###### Data Conventions
Score types:
- Top (Created by adding Advanced and Proficient)
- Advanced
- Proficient
- Basic
- Below Basic

The convention used in this project for counties is as follows:
- 0 is the state level aggregate data
- 1 is Columbia county
- 2 is Montour county

The convention used for subjects is the following:
- English - is used for English, English Language Arts and Literature
- Math - is used for Math and Algebra I
- Science - is used for Science and Biology

The convention for cohorts is:
- 1 - Cohort 1: Grade 8 (2016), Grade 11 (2019)
- 2 - Cohort 2: Grade 6 (2016), Grade 7 (2017), Grade 8 (2018), Grade 11 (2021)
- 3 - Cohort 3: Grade 5 (2016), Grade 6 (2017), Grade 7 (2018), Grade 8 (2019), Grade 11 (2022)
Grades 5-8 were extracted from the PSSA set.
Grade 11 was extracted from the Keystone set.

##### Issues, mitigations, design choices
- The PSSA and Keystone local level data set for 2015 doesn't have a County column.
   - Mitigation: I used the school districts from Columbia and Montour counties from the following years to select them in 2015.
- Columbia county data is mostly missing for 2015.
   - Mitigation: I removed 2015 data and the cohort which was in Grade 8 in 2015
- For the Keystone sets all Grades are 11
   - Mitigation: I selected Grade == "Total" and dropped the Grade Column to avoid duplicate entries.
- There was a group column in PSSA and Keystone whose values were "All students" or "Historically Underperforming" that was not available for all years.
   - Mitigation: I selected Group == "All Students" and dropped the group column.
- The original data sets contained the columns Advanced, Proficient, Basic, BelowBasic but Kadir needed them as a type column and percent column for ANOVA testing.
   - Mitigation: I used the names of the columns as a type (Baseline) and their values as the percentage (Score).
- Dr. Calhoun suggested creating a Top type by summing Advanced and Proficient.
- Dr. Calhoun noticed that in some of our analysis we were taking averages of percentages that had different sizes and suggested weighted averages.
   - Mitigation: I created a column named WScore (Students) by multiplying Scores by the amount of students Scored.
- The PSSA state level data for 2017 and 2022 don't include a "Total".
   - Mitigation: For English and Math I used the other grades to recreate a "Total" row.
   - No mitigation: Science doesn't have the individual Grades to recreate a "Total" from.
- The PSSA for 2021 doesn't include score percentages for all subjects.
   - No mitigation
   
### Project Objectives
1. How our local districts in Columbia and Montour Counties are trending since 2016? (Anna)
2. How they compare to the state trend since 2016? (Kadir)
3. Is there any COVID impact we might be able to deduce? ((Kadir))
4. Visualizing the averages of scores from each year. (Anna)
   - As a whole
   - Grouped by subject
   - Grouped by district
5. Compare scores between districts. (Anna)
6. Study cohorts as they progress from PSSA to Keystone (Ronny)
7. Any other information that data might tell us?

### Objective 1: How our local districts in Columbia and Montour Counties are trending since 2016?

##### Keystone

![obj1a](/Resources/Obj1ak.png)

We see from taking the averages of Keystone scores from Columbia(1) and Montour(2) counties along with the State(0), Columbia county has the highest average overall of people scoring in all five categories with major differences being in the top and proficient categories from 2016 to 2022. Both the Montour county and the state had signicficantly less people scoring within each category, however, Montour county had on average had slightly more people scoring in comparison to the state average with the exceptions of basic and below basic scores.

##### PSSA

![obj1b](/Resources/Obj1bp.png)

We see from taking the averages of PSSA scores from Columbia(1) and Montour(2) counties along with the State(0), Columbia county has the highest average overall of people scoring in all five categories with major differences being in the top and proficient categories from 2016 to 2022.  Both the Montour county and the state had signicficantly less people scoring within each category, however, Montour county had on average had slightly more people scoring in comparison to the state average with the exception of below basic scores, where the State and montour county averages were equal basically.


### Objective 2: How they compare to the state trend since 2016?

##### Keystone

![obj2a](/Resources/Obj2a.png)

We see that in general, Montour and Colombia county fared better than State 
since 2015 in terms of baseline levels. Basic level showed a sharp increase in Montour
and Colombia counties in 2022. Proficient level also showed a sharp
increase especially in 2022 for Montour county

##### PSSA

![obj2b](/Resources/Obj2b.png)

**Top** baseline showed a slight decline between 2019 - 2021 in State and Colombia County.
Top baseline remained steady in Montour throughout the time frame. 

**Advanced** baseline showed a sharp decline in Colombia County and State from 2019
to 2021. Montour County baseline were pretty much stable.

**Proficient** baseline showed an increase in Colombia County in 2021 and then
it swung back to its previous levels in 2022. State and Montour County fluctuated
slightly throughout the time frame without any significant observation.

**Basic** baseline  dropped slightly in Colombia county from 2018 to 2019. It remained
steady in State from 2017 to 2019. Montour County showed a slight decline
in 2019. Basic baseline increased in both counties and state in 2021, only to
drop slightly in Montour County and State in 2022. Colombia County showed
a moderate decline in Basic baseline. 

**BelowBasic** baseline remained stable for Montour County until 2019. There was a
sharp increase between 2016 and 2017 in Colombia County and State. Both
counties and state showed a decline in BelowBasic baseline in 2019. There was
a significant increase in all places in the baseline from 2019 to 2021,
especially in Colombia. From 2021 to 2022, baseline remained stable in State 
and dropped moderately in both counties

### Objective 3: Is there any COVID impact we might be able to deduce?

##### Keystone

![obj3a](/Resources/Obj3a.png)

Checking baseline averages throughout the years for counties and subjects, it
is interesting to see that COVID didn't impact Math and English for baselines
dramatically for Colombia, Montour and State. In fact, there was a significant
improvement in the scores in 2022 compared to 2019 for math top, advanced
and proficient baseline in Colombia county.
However, there was a steep decline in Science for baselines between 2021
and 2022 throughout the state which may be an indication of a COVID impact.

##### PSSA

![obj3b](/Resources/Obj3b.png)

From 2019 to 2022, baseline levels seemed stable for the counties and the state
There were only slight fluctuations which were inconclusive if COVID affected
baseline levels or not.


### Objective 4: Visualizing the averages of scores from each year.

#### As a whole.

##### Keystone

![obj4ak](/Resources/Obj4ak.png)

When looking at the Keystone Score averages by year, there was no data collected for the 2020 year, as the pandemic haulted all testing. Even so, we see an extreme dip in Top and Proficient scores from 2019 to 2022. There was also a slight increase in Basic and Below Basic scores for the 2022 year.

##### PSSA

![obj4ap](/Resources/Obj4ap.png)

We see when analyzing the data for PPSA Score averages by year, there are little to no changes in scoring and in some cases. Top and advanced scores from 2019 to 2022 slightly suffered most likely due to the pandemic. There was also a slight increase in Basic and Below Basic scores from 2019 to 2022.

#### Grouped by subject.

##### Keystone

![obj4bk](/Resources/Obj4bk.png)

We see when analyzing the data for Keystone Score averages by subject, students on average scored highly in Math and English while more likely struggling with science. Both in top and advanced categories, more students scored well on the Math sections, with English being the highest in the proficient category. When looking at the scores for below basic, Science has the highest average for those scoring in th is category.

##### PSSA

![obj4bp](/Resources/Obj4bp.png)

Contrary to the Keystone analysis, the data for PSSA score averages by subject, student on average scored highly in Science and English, while having higher below basic and basic scores within Math. Science was the highest subject for both Top and advanced categories, with English remaining the highest subject for proficient scores. 

#### Grouped by district.

##### Keystone

![obj4ck](/Resources/Obj4ck.png)

When looking at the Keystone average scores grouped by district, we can see a strong amount of Top and Proficient scores overall. We can see that the majority of average scores remain in the Top category and that there is an extreme difference when comparing to those who scored below basic and basic. Otherwise, there is no clear correlation between districts directly.

##### PSSA

![obj4cp](/Resources/Obj4cp.png)

We can see when analyzing the PSSA average scores grouped by district, that there is a strong amount of top scores from students especially within the Berwick Area School District and Bloomsburg Area School District. We can see, these two school districts amount of students scoring within the top four categories is considerably higher in comparison to other school district. Otherwise, there is no direct correlation between districts.

### Objective 5: Compare scores between districts.

##### Keystone

![obj5ak](/Resources/Obj5ak.png)

When comparing Keystone average scores between districts, there seems to be no correlation between districts and has no conclusive evidence towards an affect due to the pandemic.
##### PSSA

![obj5ap](/Resources/Obj5ap.png)

When comparing PSSA average scores between districts, there seems to be no correlation between districts and has no conclusive evidence towards an affect due to the pandemic.

### Objective 6: Cohort analysis

##### Cohort timeline

![obj6ac1](/Resources/Obj6_timeline.png)
- Cohort 1: years: 2016 - 2019, grades: 8(PSSA),     11(Keystone)
- Cohort 2: years: 2016 - 2021, grades: 6 - 8(PSSA), 11(Keystone)
- Cohort 3: years: 2016 - 2022, grades: 5 - 8(PSSA), 11(Keystone)

##### Cohorts 1-3 All Grades All Scores

![obj6ac1](/Resources/Obj6ac1.png)

##### Cohorts 1-3 All Grades Top Scores

![obj6bc1t](/Resources/Obj6bc1t.png)

##### Cohorts 1-3 Grades 8, 11 All Scores

![obj6cc2](/Resources/Obj6cc2.png)

##### Cohorts 1-3 Grades 8, 11 Top Scores

![obj6dc2t](/Resources/Obj6dc2t.png)

### Contents
- /Cohorts/       (Generated Cohorts Data)
- /Keystone/      (Keystone Exams Data)
- /PSSA/          (PSSA Exams Data)
- analysis.R      (Analysis of the data)
- testing_data.py (Processes and Generates data sets)

### Supplementary Graphs
[Tableau Graphs of School Performance by Year by Kadir](https://public.tableau.com/app/profile/kadir7189/viz/SchoolPerfomanceByYear/Sheet1)

### Authors
- [Ronny Toribio](https://github.com/ronny-phoenix) - Project lead, Data Wrangling, Statistical Analysis
- [Anna T. Schlecht](https://github.com/atschlecht) - Statistical Analysis
- [Kadir O. Altunel](https://github.com/KadirOrcunAltunel-zz) - Statistical Analysis
