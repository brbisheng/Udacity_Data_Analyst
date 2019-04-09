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
1. Both Tthe distribution of `Weight` and that of `Height`appear to be right skewed, and the shape of physical feature distributions can vary to a large extent when we adjust the bin size.
2. I find that both-handed players have most stable performance, but they have significantly low number of homeruns in terms of quantiles.
3. Homerun (`HR`) appears to be exponentially increasing in Batting average (`Avg`), but there is significant heteroskedasticity.
4. Both Homerun (`HR`) and Batting average (`Avg`) tend to decrease for players who have large `Weight` and `Height` at the same time..

### Design

1. Univariate analysis

I start with univariate analysis. I find that the histogram of physical features depend crucially on the bin size we choose. This is partly because `Weight` and `Height` are both whole numbers. Since Weight, Height and HR are all whole numbers. It would be more reasonable that the chosen bin size be also a whole number.

To be more precise, 
- `Height`: The suggested bin size is 1.92. With this bin size, the graph appears to be normal. When I change the bin size to 1, the graph appears to be right skewed.
- `Weight`: The suggested bin size is 7.6. With this bin size, the graph appears to be bimodal. When we change the bin size to 1, we can notice its whole number nature, and the variable appears to be right skewed.
- `Avg`: Batting average is left skewed with a lot of zeros. 
- `HR`: Number of Homeruns has a shape of exponetial distribution. 
- `Handedness`: The majority of players are right-handed, 63.7% of the total obvervations. There are around 9.0% of both-handed players, and 27.3% of left-handed players.

(When I created a new feature `BMI`, I find that it does not appear to bring us additional insight. This could also be attributed to the whole number nature of `Weight` and `Height`.)


2. Bivariate analysis

When I use `Handedness` to explain `Avg` and `HR`, I find that both handed players have most stable performance: the dispersions of `Avg` and `HR` are both the least, however, this can be due to the fact that the number of both handed players is the least.

Batting average (`Avg`) has a nonlinear positive relationship with Homerun (`HR`). In fact, as `Avg` increases, the variation of `HR` becomes larger and larger, this is termed as heteroskedastic in statistics.

3. Multivariate analysis

Both Homerun (`HR`) and Batting average (`Avg`) tend to decrease for players who have large `Weight` and `Height` at the same time.


### Feedback

*First feedback*:

- [x] **Comment**: It seems that `Weight` and `Height` are whole numbers. Maybe you could add a parameter to allow users to adjust bin size.
    - **Reply**: Yes. I have added respective bin_size parameter for each histogram.

- [x] **Comment**: You can try to create a new feature `BMI` from `Weight` and `Height`.
    - **Reply**: Yes. I tried to create the feature `BMI` using the following formula 703 * `Weight`/(`Height`)^2. However, the new feature does not appear to bring me richer insights.
    
- [x] **Comment**: The hypothesis of linear relationship between performance and physical features do not seem correct. You claim that `HR` is increasing in both `Weight` and `Height`, `Avg` is increasing in both `Weight` and `Height`. But you also find that `HR` is nonlinearly increasing in `Avg`. That is contradictory to intuitions and is very confusing. Maybe there is some lurking variable, but you do not have that lurking variable because you only have a small data set. You should try something which imposes less stringent assumptions on your statistical model.
    - **Reply**: It is true. In the current version, I use scatter plot for `Weight` and `Height`; I then divide `Avg` and `HR` respectively into two groups based on whether the value is above or below the median. I then use shape and color to help illustrate the influence of physical qualities on performance. 
    
*Second feedback*:

- [x] **Comment**: Please use automatic size for your dashboard and storyboard.
    - **Reply**: Yes. Changes are applied.
- [x] **Comment**: You could hide unused sheets in your dashboard and storyboard.
    - **Reply**: Yes. Changes are applied.
- [x] **Comment**: You should add your reply to the feedbacks.
    - **Reply**: Yes. Changes are applied.
- [x] **Comment**: In your storyboard, the descriptions is too long to be fully displayed. 
    - **Reply**: Yes. I adjusted the size of the captions.
- [x] **Comment**: In the first part of your story, you mentioned that 'the distributions can vary to a large extent with respect to the bin size.' This is not a valid conclusion, please be more precise.  
    - **Reply**: Yes. I have now added more details.
- [x] **Comment**: Please add necessary labels and title. Please adjust the axis name.
    - **Reply**: Yes. Changes are applied.




### Resource

References:
1. Calculation of BMI
  - https://www.cdc.gov/healthyweight/assessing/bmi/childrens_bmi/childrens_bmi_formula.html
2. Create Bins from continuous measures
  - https://onlinehelp.tableau.com/current/pro/desktop/en-us/calculations_bins.htm
