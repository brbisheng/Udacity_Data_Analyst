**First version:**
https://public.tableau.com/profile/shengbi1015#!/vizhome/Udacity_Tableau_Version1/Story1?publish=yes

**Second version:** (from Udacity mentor)
https://public.tableau.com/profile/shengbi1015#!/vizhome/Book1_15543894970930/Story1?publish=yes

**Final version:**



## Data 
Baseball data contains information from 1157 players. The following features are included
- Physical features:
    - `Height`
    - `Weight`
    - `Handeness`
- Performances:
    - `Avg`: Batting Average
    - `HR`: Number of Home runs


### Summary
We want to study the interactions between the physical features of baseball players and their performances. I have the following discoveries:
1. The shape of histogram of the physical features can vary to a large extent when we adjust the bin size.     
2. I find that both-handed players have most stable performance, but they have significantly low number of homeruns in terms of quantiles.
3. Homerun (`HR`) appears to be exponentially increasing in Batting average (`Avg`), but there is significant heteroskedasticity.
4. Both Homerun (`HR`) and Batting average (`Avg`) tend to decrease for players who have large `Weight` and `Height` at the same time..

### Design

1. Univariate analysis

I start with univariate analysis. I find that the histogram of physical features depend crucially on the bin size we choose. This is partly because `Weight` and `Height` are both whole numbers. Since Weight, Height and HR are all whole numbers. It would be more reasonable that the chosen bin size be also a whole number.

When I created a new feature `BMI`, I find that it does not appear to bring us additional insight. This could also be attributed to the whole number nature of `Weight` and `Height`.


2. Bivariate analysis

When I use `Handedness` to explain `Avg` and `HR`, I find that both handed players have most stable performance: the dispersions of `Avg` and `HR` are both the least, however, this can be due to the fact that the number of both handed players is the least.

Batting average (`Avg`) has a nonlinear positive relationship with Homerun (`HR`). In fact, as `Avg` increases, the variation of `HR` becomes larger and larger, this is termed as heteroskedastic in statistics.

3. Multivariate analysis

Both Homerun (`HR`) and Batting average (`Avg`) tend to decrease for players who have large `Weight` and `Height` at the same time.


### Feedback

First feedback:
    - **Comment**: It seems that `Weight` and `Height` are whole numbers. Maybe you could add a parameter to allow users to adjust bin size.
        - **Reply**: Yes, I have added a parameter for this sake.
    - **Comment**: You can try to create a new feature `BMI` from `Weight` and `Height`.
    - **Comment**: The hypothesis of linear relationship between performance and physical features do not seem correct. You claim that `HR` is increasing in both `Weight` and `Height`, `Avg` is increasing in both `Weight` and `Height`. But you also find that `HR` is nonlinearly increasing in `Avg`. That is contradictory to intuitions and is very confusing. Maybe there is some lurking variable, but you do not have that lurking variable because you only have a small data set. You should try something which imposes less stringent assumptions on your statistical model.
        - **Reply**: 

### Resource

References:
1. Calculation of BMI
  - https://www.cdc.gov/healthyweight/assessing/bmi/childrens_bmi/childrens_bmi_formula.html
2. Create Bins from continuous measures
  - https://onlinehelp.tableau.com/current/pro/desktop/en-us/calculations_bins.htm
