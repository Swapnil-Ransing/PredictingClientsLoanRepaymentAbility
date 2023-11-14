# Predicting Clients Loan Repayment Ability
[Kaggle Problem Statemant Link](https://www.kaggle.com/competitions/home-credit-default-risk/overview)
## Overview
Many people struggle to get loans due to insufficient or non-existent credit histories. Unfortunately, this population is often taken advantage of by untrustworthy lenders. Home credit accesses repayment ability of this unbanked population by using variety of data including telco and transactional information. Doing so will ensure that clients capable of repayment are not rejected and that loans are given.

This problem statement of predicting clients loan repayment ability is a Kaggle competition happened in year 2018. This competition was conducted by Home credit group. Home credit is an international consumer finance provider company.

In this case study data provided by home credit to predict client’s repayment capability is as follows:

![Summary Table Image](https://github.com/Swapnil-Ransing/PredictingClientsLoanRepaymentAbility/blob/main/07_ReferenceImages/DataTableSummary.JPG)

- Business Constraints:
1. No strict latency requirements
2. Prediction probability is important
3. Results interpretability is important
- Performance metrics:
1. Area under the ROC curve 
2. F1 score
3. Confusion matrix

Extensive literature survey was done on the problem statement, domain, solution approaches and [can be found here](https://github.com/Swapnil-Ransing/PredictingClientsLoanRepaymentAbility/blob/main/01_Literature_Survey/AbstractAndSummary.pdf).

## Exploratory Data Analysis 
[EDA iPyhton Notebook](https://github.com/Swapnil-Ransing/PredictingClientsLoanRepaymentAbility/blob/main/02_EDA/EDA.ipynb)
Each of the provided dataframe was analyzed one by one. From these dataframes categorical and continuous variables were found.

Plotted categorical variables of a dataframe which shows higher Phi-K correlation coefficient. Following are the plots used for plotting categorical variable.
•	Count Plot: Percentage Count plot of variable categories
•	Stacked Bar Plot: Binary classification perentage plot of each category of categorical variable
•	Pie Chart: Percentage count of defaulters in dataframe variable

Plotted continuous variables of a dataframe which shows higher Pearson correlation coefficient. Along with from plot sometimes understood percentile distribution of dataframe variable. Following are the plots used for plotting continuous variable.
•	Density Plot
•	Box Plot
•	Pair Plot

Target distribution on the data has shown that the data is imbalanced with around 8% defaulters. Defaulters are client with payment difficulties: he/she had late payment more than X days on at least one of the first Y installments of the loan in our sample while non defaulters are rest of the clients.

![Target Distribution Image](https://github.com/Swapnil-Ransing/PredictingClientsLoanRepaymentAbility/blob/main/07_ReferenceImages/TargetDistribution.JPG)

-Following is a sample of NAME_INCOME_TYPE categorical variable EDA:

![Target Distribution Image](https://github.com/Swapnil-Ransing/PredictingClientsLoanRepaymentAbility/blob/main/07_ReferenceImages/NameIncomeType1.JPG)
![Target Distribution Image](https://github.com/Swapnil-Ransing/PredictingClientsLoanRepaymentAbility/blob/main/07_ReferenceImages/NameIncomeType2.JPG)
![Target Distribution Image](https://github.com/Swapnil-Ransing/PredictingClientsLoanRepaymentAbility/blob/main/07_ReferenceImages/NameIncomeType2.JPG)

** Observations and conclusions :**

For income type categorical variable total categories are 8 and all the samples are categorized i.e. none of the sample have NaN value.
Working is the largest income type (51.6%) of apllicant followed by comercial associate (23.3%) , pensioners (18%) and so on.
Similar distribution of defaulters is also observed from the pie chart as observed in the count bar plot. Working constitute 61% of total defaulted applicant. For commercial associate this number is 22% and for pensioners it is 12%.
If we look for percentage of defaulters for each category, maternity leave and unemployed income type shows the highest defaulters. However for this categories count of applicant is almost 0. For students, there is not a single defaulters and applicant are almot 0%.
For smalller count, decision can be taken to combine these categories while featurizing.
