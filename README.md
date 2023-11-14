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

Plotted categorical variables of a dataframe which shows higher Phi-K correlation coefficient. 
Following are the plots used for plotting categorical variable:
- Count Plot: Percentage Count plot of variable categories
-	Stacked Bar Plot: Binary classification perentage plot of each category of categorical variable
-	Pie Chart: Percentage count of defaulters in dataframe variable

Plotted continuous variables of a dataframe which shows higher Pearson correlation coefficient. Along with from plot sometimes understood percentile distribution of dataframe variable. 
Following are the plots used for plotting continuous variable:
-	Density Plot
-	Box Plot
-	Pair Plot

Target distribution on the data has shown that the data is imbalanced with around 8% defaulters. Defaulters are client with payment difficulties: he/she had late payment more than X days on at least one of the first Y installments of the loan in our sample while non defaulters are rest of the clients.

![Target Distribution Image](https://github.com/Swapnil-Ransing/PredictingClientsLoanRepaymentAbility/blob/main/07_ReferenceImages/TargetDistribution.JPG)

### Following is a sample of NAME_INCOME_TYPE categorical variable EDA:

![Target Distribution Image](https://github.com/Swapnil-Ransing/PredictingClientsLoanRepaymentAbility/blob/main/07_ReferenceImages/NameIncomeType1.JPG)
![Target Distribution Image](https://github.com/Swapnil-Ransing/PredictingClientsLoanRepaymentAbility/blob/main/07_ReferenceImages/NameIncomeType2.JPG)
![Target Distribution Image](https://github.com/Swapnil-Ransing/PredictingClientsLoanRepaymentAbility/blob/main/07_ReferenceImages/NameIncomeType3.JPG)

**Observations and conclusions:**
1. For income type categorical variable total categories are 8 and all the samples are categorized i.e. none of the sample have NaN value.
2. Working is the largest income type (51.6%) of apllicant followed by comercial associate (23.3%) , pensioners (18%) and so on.
3. Similar distribution of defaulters is also observed from the pie chart as observed in the count bar plot. Working constitute 61% of total defaulted applicant. For commercial associate this number is 22% and for pensioners it is 12%.
4. If we look for percentage of defaulters for each category, maternity leave and unemployed income type shows the highest defaulters. However for this categories count of applicant is almost 0. For students, there is not a single defaulters and applicant are almot 0%.
5. For smalller count, decision can be taken to combine these categories while featurizing.

### Following is a sample of EXT_SOURCE_3 continuous variable EDA:

![Target Distribution Image](https://github.com/Swapnil-Ransing/PredictingClientsLoanRepaymentAbility/blob/main/07_ReferenceImages/EXT_SOURCE_3.JPG)

**Observations and Conclusions :**
1. This variable represents the normalized score of application from external data source.
1. Row samples of EXT_SOURCE_3 continuous variable have 19.83% have NaN values. Out of these NaN valued row samples, around 90.69% are non defaulters while 9.31% are defaulters.
2. Probability density plot of non defaulters is left skewed while that of defaulters is right skewed.Range of values for both of these categories lies between 0 to 0.9 .
3. Intequertile range of non defaulters box plot is from 0.39 to 0.68 while that of defaulters is from 0.22 to 0.52. This is an evident that for smaller values of this variable, there are more chances of defaulting the loan.
4. Median values of non defaulters is higher than the defaulter. This value can be used for imputing the NaN samples.

### CONCLUSIONS FROM EDA:
EDA of data gave a good insight into the available data. Conclusions from the descriptive statistics, correlation coefficients and variable plots observations are as follows:

1. Data is imbalanced. There are almost 92% of non defaulters and 8% defaulters. We will have to come up with sampling techniques while building the model.
2. Week correlation is observed for most of the variables and target label. This represent that the data is not linearly correlated. High order featurization can be used to improve model performance for classification.
2. Few of the variables have shown a good linear correlation wrt target label data. These features will be important for the classification task.
3. Few of the variables have number of days samples. Value range of these samples is high.(Eg. 0 to 1,00,000). Such variables can be transformed into years or month data for efficient computation. This transformation is also used in EDA for better visualization and observations.
4. Few of the continuous variables shows anamolies. (Eg. Count of days wrt application date should be negative, however few percentile of data has these values as positive. Also for some samples count of days does not make sense.) These anamolies should be dealt either by saturating the values to the nearest possible values or by imputation.
5. It is also been onserved that, for some single categorical variables, there are multiple categories with 0% count. Such categories can be grouped together for dimensionality reduction.
6. For variables where we observ a different median values of defaulter and non defaulter categoris, NaN values of such variables can be imputed by corresponding median values.
7. Observations from percentile statistics has shown anomolies in data for small percentile value. Such data can be saturated or imputed.
8. There are few variables which have almost 99% NaN values. Such variables can be discarded. A threshold of percentage of NaN values can be decided to discard the variables from the dataframe.
9. As the data is relational, data needs to be merged in an smart way. For visualization in this EDA study left merge on the Application Train dataframe is used.

## Featurization 
[Featuriztion iPyhton Notebook](https://github.com/Swapnil-Ransing/PredictingClientsLoanRepaymentAbility/blob/main/03_Featuriztion/Featurization.ipynb)

Each of the provided dataframes data was cleaned first and then featurized. EDA observations and conclusions were used for featurization.

For features were created for each of the individual dataframes:
1. Count variables : Explains the count of considered variable wrt applicant. Eg. Count of previous applications by current client
2. Age variables : Explains the time difference between current application and previous application. Multiple combinations such as last application, first appllication, last closed application, last defaulted application wrt current application etc. were created
3. Velocity variables : Explains the count/ amount sum  wrt time with or without additional conditions by a client. Eg. Sum of previous loan amount not defaulted by client in last 3 years, count of various jobs performed by client in last 1 year, count of various jobs performed by client in last 1 year where loan is not defaulted etc.
4. Decay Variables : Exponential decay of variables such as amount paid in each of last 1 year per month This gives higher imortance to the recent information.
5. Categorical variable encoding : Various encoding methods were used such as label encoding, one hot encoding and frequency encoding.
6. Aggregation techniques : Various combinations of continous variables such as simple moving average, exponential moving averae, weighted moving average and their combination wrt min, max, last, first etc. were used.

Once the individual dataframes were featurized, they were left joined on the Application Train and Application Test dataframes. **1770** different variables were created in the featurization.
- Application train merged dataframe shape was  (307511, 1772)
- Application test merged dataframe shape was  (48744, 1771)

### EDA of Merged Dataframe:
Correlation between variable and Target variable sorted in descending order were :

| Variable | Correaltion With Target |
| --- | ---|
| TARGET |                              1.000000 |
| EXT_SOURCE_2  |                       0.159029 |
| EXT_SOURCE_3    |                     0.119572 |
| EXT_SOURCE_MUL   |                    0.088886 |
| DAYS_CREDIT_MEAN   |                  0.083960 |
| EXT_SOURCE_MIN      |                 0.081264 |
| DAYS_BIRTH          |                 0.078239 |
| CREDIT_ACTIVE_CLOSED_MEAN  |          0.076500 |
| NAME_CONTRACT_STATUS_MEAN_ALL  |      0.073213 |
| DAYS_CREDIT_MAX                  |    0.072869 |
| NAME_CONTRACT_STATUS_MEAN_LAST_5  |   0.070471 |

Variables of merged application train dataframe did not shown a linear behavior with the Target. There were large number of variables which shown a very less correlation (<0.06) wrt Target.

