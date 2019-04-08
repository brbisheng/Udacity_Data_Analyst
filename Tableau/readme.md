First version:
https://public.tableau.com/profile/shengbi1015#!/vizhome/Udacity_Tableau_Version1/Story1?publish=yes

Final version: 
https://public.tableau.com/profile/shengbi1015#!/vizhome/Book1_15543894970930/Story1?publish=yes


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
1. The histogram of the physical features can vary to a large extent when we adjust the bin size.     
2. I find that both handed players have most stable performance, but they have significantly low number of homeruns in terms of quantiles.
3. Homerun (`HR`) appears to be exponentially increasing in Batting average (`Avg`), but there is significant heteroskedasticity.
4. Both Homerun (`HR`) and Batting average (`Avg`) tend to decrease for players who have large `Weight` and `Height` at the same time..

### design 设计： 解释你所做的任何设计选择，包括收集反馈后对可视化进行的更改

1. Univariate analysis

I start with univariate analysis. I find that the histogram of physical features depend crucially on the bin size we choose. This is partly because `Weight` and `Height` are both whole numbers. Since Weight, Height and HR are all whole numbers. It would be more reasonable that the chosen bin size be also a whole number.

When I created a new feature `BMI`, I find that it does not appear to bring us additional insight. This could also be attributed to the whole number nature of `Weight` and `Height`.


2. Bivariate analysis

When I use `Handedness` to explain `Avg` and `HR`, I find that both handed players have most stable performance: the dispersions of `Avg` and `HR` are both the least, however, this can be due to the fact that the number of both handed players is the least.

Batting average (`Avg`) has a nonlinear positive relationship with Homerun (`HR`). In fact, as `Avg` increases, the variation of `HR` becomes larger and larger, this is called heteroskedastic in statistics.

3. Multivariate analysis

Both Homerun (`HR`) and Batting average (`Avg`) tend to decrease for players who have large `Weight` and `Height` at the same time.


### feedback 反馈： 包含从第一份草图到最终可视化期间，你从他人那里获得的针对你的可视化的所有反馈

- It seems that `Weight` and `Height` are whole numbers. Maybe you could add a parameter to allow users to adjust bin size.
- You can try to create a new feature `BMI` from `Weight` and `Height`.
- The linear relationship between `Weight` and `Avg`, as well as that of `Height` and `Avg` does not seem convincing.

### resource. 资源： 列出你创建可视化时参考的任何来源

References:
1. Calculation of BMI
  - https://www.cdc.gov/healthyweight/assessing/bmi/childrens_bmi/childrens_bmi_formula.html
2. Create Bins from continuous measures
  - https://onlinehelp.tableau.com/current/pro/desktop/en-us/calculations_bins.htm
