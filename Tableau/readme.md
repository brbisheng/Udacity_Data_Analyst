## Data 

Baseball data containing 1157 players. The following features are included
- `Avg`: Batting Average
- `Height`
- `Weight`
- `HR`: Number of Home runs
- `Handeness`


## Report
1. First version:
    - Feedback:
        1. 
        2. 
2. Second version: 



### quick summary
1. `HR` is increasing in both `Weight` and `Height`. 
2. `Avg` is decreasing in both `Weight` and `Height`.
3. The slopes of the regression lines do not differ significantly when I compare the 3 handeness groups 'L', 'B', and 'R'.

### design 设计： 解释你所做的任何设计选择，包括收集反馈后对可视化进行的更改

#### Univariate analysis

1. `Weight` and `Height` are whole numbers. So the bin sizes of these two distributions in histogram are chosen as 1 
2. I created a new feature `BMI`.

Creat new variable: BMI

#### Bivariate analysis


#### 


### feedback 反馈： 包含从第一份草图到最终可视化期间，你从他人那里获得的针对你的可视化的所有反馈

First feedback:
- It seems that the `Weight` and `Height` are whole numbers. 
- The variable `BMI` you created does not seem informational, because `Weight` and `Height` are whole numnbers.
- The linear relationship does not seem convincing, because there are many v
- The path graph is inaccurate, because the groups have different number of data points.

### resource. 资源： 列出你创建可视化时参考的任何来源

References:
1. Calculation of BMI
  - https://www.cdc.gov/healthyweight/assessing/bmi/childrens_bmi/childrens_bmi_formula.html
2. Create Bins from continuous measures
  - https://onlinehelp.tableau.com/current/pro/desktop/en-us/calculations_bins.htm
3. 



Link: 包含你的 Tableau Public 工作簿的链接，在线发布以及一个包含四部分的报告。参见此处，获取发布 Tableau Public 工作簿方面的帮助。

总结： 不超过四句话，简要介绍你的数据可视化，并添加可帮助读者理解的上下文信息
