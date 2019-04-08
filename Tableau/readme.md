First version:

Final version: 



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
We want to study to the interactions between the physical features of baseball players and their performances. I have the following discoveries:
1. The histogram of the physical features can vary to a large extent when we adjust the bin size. It is because
    - I created a new feature `BMI`, but it does not appear to bring us additional insight.
2. When I use `Handedness` to explain `Avg` and `HR`, I find that both handed players have most stable performance.
1. `HR` is increasing in both `Weight` and `Height`. 
2. `Avg` is decreasing in both `Weight` and `Height`.
3. The slopes of the regression lines do not differ significantly when I compare the 3 handeness groups 'L', 'B', and 'R'.

### design 设计： 解释你所做的任何设计选择，包括收集反馈后对可视化进行的更改

1. Univariate analysis

I start with univariate analysis. I find that the histogram of physical features depend crucially on the bin size we choose. This is partly because `Weight` and `Height` are both whole numbers.

We mostly used histograms to visualize the distributions of the variables. Namely we notice that the
height and weight of the players have normal distributions.
We use a bar chart to see the distribution of home runs based on handedness. The reason for using a
bar chart in this case is because handedness is a categorical variable with 3 classes: left, right, and both.
We also use scatterplots to see the relationships between weight, height and home runs.



Since `Weight`, `Height` and `HR` are all whole numbers. The chosen bin size should be also a whole number.
1. `Weight` and `Height` are whole numbers. So the bin sizes of these two distributions in histogram are chosen as 1.
    - For `Weight`, the values are concentrated at multiples of 5, such as 155, 160, 165, 170, etc.
2. I created a new feature `BMI`.

Creat new variable: BMI

#### Bivariate analysis


### feedback 反馈： 包含从第一份草图到最终可视化期间，你从他人那里获得的针对你的可视化的所有反馈

- It seems that `Weight` and `Height` are whole numbers. Maybe you could add a parameter to allow users to adjust bin size.
- You can try to create a new feature `BMI` from `Weight` and `Height`.
- The linear relationship does not seem convincing, because there are many v
- The path graph is inaccurate, because the groups have different number of data points.

### resource. 资源： 列出你创建可视化时参考的任何来源

References:
1. Calculation of BMI
  - https://www.cdc.gov/healthyweight/assessing/bmi/childrens_bmi/childrens_bmi_formula.html
2. Create Bins from continuous measures
  - https://onlinehelp.tableau.com/current/pro/desktop/en-us/calculations_bins.htm


Link: 包含你的 Tableau Public 工作簿的链接，在线发布以及一个包含四部分的报告。参见此处，获取发布 Tableau Public 工作簿方面的帮助。

总结： 不超过四句话，简要介绍你的数据可视化，并添加可帮助读者理解的上下文信息
