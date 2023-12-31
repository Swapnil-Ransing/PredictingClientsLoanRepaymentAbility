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

## Feature Selection: 
[Feature Selection iPyhton Notebook](https://github.com/Swapnil-Ransing/PredictingClientsLoanRepaymentAbility/blob/main/04_Feature_Selection/FeatureSelection.ipynb)

Featurization produced 1770 features, however all of these features should not be used in the machine learning model. It was necessary to understand the most important features amongst these 1770 features which would be useful for better predictions. Following steps were taken to get the importatnt 15 features for classification task:

### 1. Data cleaning of merged dataframe:
Removed 352 columns from merged dataframe which has >99% frequency of a single value. After this, train merged dataframe splitted into train, test and oot. 

### 2. Top 40 feature selection using Weight of Evidence and Information Value:
The weight of evidence (WoE) tells the predictive power of a single feature concerning its independent feature. If any of the categories/bins of a feature has a large proportion of events compared to the proportion of non-events, we will get a high value of WoE which in turn says that that class of the feature separates the events from non-events. WoE value tells us the predictive power of each bin of a feature. However, information value (IV) represents the entire feature’s predictive power. IV values of each feature were manually analyzed and top 40 different features were selected.

### 3. Min max scaling and SMOTE data:
Train data is scaleed between 0 to 1 and its transformation is applied on the test and oot data. Also since the target label is imbalanced, synthetic defaulter client target points were created using SMOTE technique for train.

### 4. Top 15 feature selection using count of votes:
From th top 40 selected features further 15 features were selected by count of votes method. In this for each of the machine learning algorithm top 15 features were given a vote by each algorithm based on  feature importance and finally votes for each variable is counted and top 15 voted variables were selected. Following were the algorithms used for voting:
1. Recursive feature elimination with Logisticregression
2. Correlation
3. Lasso with logistic regression
4. Recursive feature elimination with decision tree
5. Recursive feature elimination with random forest
6. Recursive feature elimination with gradient boosting classifier
7. Recursive feature elimination with Perceptron
8. Extra tree classifier
9. Permutation importance

### Following top 15 features were selected :

| Variable Name | Information Value | Count of Votes | Variable Description |
| --- | --- | --- | --- |
| EXT_SOURCE_2 |0.318135 | 9 | Normalized score from external data source |
| EXT_SOURCE_3 |0.246297	 | 9 | Normalized score from external data source |
| DAYS_PAYMENT_RATIO_MAX_Latest_year |0.076490 | 8 | Maximum value from ratio of previous credit pay date to the actual payment date wrt current application for last one year |
| AMT_GOODS_PRICE |0.095468 | 8 | Goods price of good that client asked for (if applicable) on the previous application|
| CNT_DRAWINGS_ATM_CURRENT_MAX_Latest_year |0.044054 | 8 | Max value from count of ATM drawings during a last year month on the previous credit |
| EXT_SOURCE_1 |0.110553 | 8 | Normalized score from external data source |
| DAYS_DETAILS_CHANGE_MUL |0.052973	 | 7 | Multiplication of how many days before the application did client (changed the phone, changed the registration, change the identity document) with which he applied for the loan |       
| AMT_CREDIT |0.052588 | 7 | Credit amount of the loan|
| DAYS_BIRTH |	0.083650	 | 7 | Client's age in days at the time of application |
| DEF_60_CREDIT_RATIO |0.055447 | 7 | Ratio of Credit amount of the loan by how many observation of client's social surroundings defaulted on 60 (days past due) DPD |
| AMT_PAYMENT_DIFF_MAX_Latest_year |0.050351	 | 6 | Max difference between the prescribed installment amount of previous credit and the actual amount paid over the last year |
| INTEREST_SHARE_MEAN_LAST_5 |0.053174 | 6 | mean ratio of (Credit amount* Term* annuity) of previous loan by credit amount of the loan for last 5 month |                         
| REGION_POPULATION_RELATIVE |0.042427	 | 6 | Normalized population of region where client lives (higher number means the client lives in more populated region) |
  | DAYS_LAST_PHONE_CHANGE | 0.042783 | 6 | How many days before application did client change phone |
| EMPLOYED_TO_AGE_RATIO | 0.083327 | 6 | Ratio of how many days before the application the person started current employment by client's age in days wrt current application |

## Modeling and Results:
[Modeling iPyhton Notebooks Folder](https://github.com/Swapnil-Ransing/PredictingClientsLoanRepaymentAbility/tree/main/05_Modeling)

Various models including machine learning and deep learning models were implemented on the final set of variables. These models hyperparameters were tuned to achieve the best performance of ROC AUC. Following is the summary result of these models:
| Model Number |	Model Name |	OOT ROC AUC	| OOT Defaulter Capture at 20% Operating range	| Kaggle Private Score	| Kaggle Public Score	| Predicion Latency	| Probability Distribution |
| --- | --- | --- | --- | --- | --- | --- | --- |
|1	|Logistic Regression |	71.95% |	45.96% |	70.28% |	70.49% |	Low	 | Good |
|2	|SGD |	71.93% |	45.83% |	70.16% |	70.34% |	Low |	Good
|3	|Adaboost |	75.68% |	52.52% |	74.02% |	74.65% |	Low |	Ok |
|4	|XGBoost |	75.43% |	51.90% |	73.29% |	73.39% |	Low |	Poor |
|5	|LightGbm |	75.97% |	52.56% |	74.47% |	74.20% |	Low |	Ok |
|6	|MLP |	73.85% |	50.33% |	72.28% |	72.67% |	Low |	Poor |

**Observations and Conclusions:**
1. Logistic regression and SGD model gave a good probability distribution across the deciles. This will be helpful in developing scorecard as scores will also be well distributed helping developing the strategies. 
2. Adaboost, XGBoost and LightGbm model gave a high performance on OOT data, shown a good defaulter capture rate at 20% operating range. These also gave a high predictions on the kaggle datasets.
3. MLP models performance is lower than the Adaboost, XGbost and Lightgbm models.
4. All the models prediction latency is low. As the model uses only 15 selected features for prediction and a single model.

## Risk Scorecard and Risk Strategy Development:
[Risk Scorecard example iPyhton Notebook](https://github.com/Swapnil-Ransing/PredictingClientsLoanRepaymentAbility/blob/main/06_RiskScorecard/RiskScorecardDevelopment_SampleExample.ipynb)

[Risk Scorecard iPyhton Notebook](https://github.com/Swapnil-Ransing/PredictingClientsLoanRepaymentAbility/blob/main/06_RiskScorecard/LoanDefaulter_RiskScorecardDevelopment_Adaboost.ipynb)

A risk score is a numerical score that is derived from the statistical analysis and represents transactions riskiness. Higher risk score corresponds to high default probability. Defining threshold for loan applicants acceptance decision, based on the developed models prediction probabilities will be challenging as transactions predicted probabilities will be tightly placed between small scale. So, it necessitates the scaling methodology which can scale the predicted probabilities to a large range of scores. 

The mentioned notebook demonstrate the procedure to develope a risk scorecard and also provides an interactive user interface to tune the PDO calibration values.

As the Adaboost model shown a good probability distribution and high deafulter capture rate at 20% operating range of the oot data, Adaboost model was used for for developing the scorecard. After tuning the PDO calibration parameters, aligned score equation obtained for this model was :

**Aligned Score= 719.948 - -1531.484*score**

where, score is log(odds).

Following distribution of various metrics in each aligned score bucket is obtained for train, test and oot:

![RiskScoreStrategy Image](https://github.com/Swapnil-Ransing/PredictingClientsLoanRepaymentAbility/blob/main/07_ReferenceImages/RiskScoreStrategy.JPG)

### Observations from risk scores are :

1. Obtained well distribution of cummulative percentage of total transactions and cummulative percentage of bad transactions throught the score range (300-999).
2. Scorecard shows similar distribution for train, test and oot data.
3. Percentage of total applicants in each bucket is not greater than 10%. Also this graph is left skewed indicating higher applicants with lower scores.
4. Cumulative percentage of total applicants are 40% for score around 550.
5. Cumulative bad rate is 80% for score around 550.
6. Good by bad ratio is higher for lower scores indicating higher good transactions at lower score values.

### Risk Strategy:
A simple risk strategy can be developed which can aim at reducing giving loans to defaulters, giving loan to the good population based on the risk scorecard. 

Eg.

| Decision | Score Range | % Total Applicants | % Bad/Total Applicant | %  Bad/Total Bad Applicant |
| --- | --- | --- | --- | --- |
| Direct Approval | 300< Score <= 500 | 40% | 1.5% |  20% |
| Manual Review | 500 <Score <= 560 | 40% | 2.9% | 36.4% |
| Direct Decline | Score > 560 | 20% | 3.5% | 43.6% | 

This strategy can be refined further and approval percentage can further be increased by clubing the scores with other variables (applicants attribute) like amount of loan, age of applicant etc.

## Scope for Improvement :
1. Improving F1 score and confusion matrix.
2. Ensemble modelig techniques can be tried out in the modeling to achieve the higher ROC AUC.[Kaggle Third Place Solution](https://www.kaggle.com/c/home-credit-default-risk/discussion/64596)
3. Modeling technique such as: Clustering the data by using clustering algorithm and then using the machine learning algorithm on clusters to achieve the higher ROC AUC. [Research paper link](https://cs229.stanford.edu/proj2019aut/data/assignment_308832_raw/26644913.pdf)
4. Refining the stragy using combination of aligned score and applicants attribute.
