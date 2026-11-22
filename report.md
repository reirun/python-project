# Data exploration

We first have a look at the dataset and make sure al values are coherent

|                    |   count |     mean |     std |   min |      25% |     50% |      75% |   max |
|:-------------------|--------:|---------:|--------:|------:|---------:|--------:|---------:|------:|
| ID                 |    5000 |  2500.5  | 1443.52 |     1 |  1250.75 |  2500.5 |  3750.25 |  5000 |
| Age                |    5000 |    45.34 |   11.46 |    23 |    35    |    45   |    55    |    67 |
| Experience         |    5000 |    20.1  |   11.47 |    -3 |    10    |    20   |    30    |    43 |
| Income             |    5000 |    73.77 |   46.03 |     8 |    39    |    64   |    98    |   224 |
| ZIP Code           |    5000 | 93152.5  | 2121.85 |  9307 | 91911    | 93437   | 94608    | 96651 |
| Family             |    5000 |     2.4  |    1.15 |     1 |     1    |     2   |     3    |     4 |
| CCAvg              |    5000 |     1.94 |    1.75 |     0 |     0.7  |     1.5 |     2.5  |    10 |
| Education          |    5000 |     1.88 |    0.84 |     1 |     1    |     2   |     3    |     3 |
| Mortgage           |    5000 |    56.5  |  101.71 |     0 |     0    |     0   |   101    |   635 |
| Personal Loan      |    5000 |     0.1  |    0.29 |     0 |     0    |     0   |     0    |     1 |
| Securities Account |    5000 |     0.1  |    0.31 |     0 |     0    |     0   |     0    |     1 |
| CD Account         |    5000 |     0.06 |    0.24 |     0 |     0    |     0   |     0    |     1 |
| Online             |    5000 |     0.6  |    0.49 |     0 |     0    |     1   |     1    |     1 |
| CreditCard         |    5000 |     0.29 |    0.46 |     0 |     0    |     0   |     1    |     1 |


We notice that *Experience* has a minimum of -3, which should not be possible. We further investigate into this issue and look at the characteristics of such customers, to handle this in the best way possible. 


|                    |   count |     mean |     std |     min |      25% |     50% |      75% |     max |
|:-------------------|--------:|---------:|--------:|--------:|---------:|--------:|---------:|--------:|
| ID                 |      52 |  2427.35 | 1478.83 |    90   |   767.25 |  2783.5 |  3669.5  |  4958   |
| Age                |      52 |    24.52 |    1.48 |    23   |    24    |    24   |    25    |    29   |
| Experience         |      52 |    -1.44 |    0.64 |    -3   |    -2    |    -1   |    -1    |    -1   |
| Income             |      52 |    69.94 |   37.96 |    12   |    40.75 |    65.5 |    86.75 |   150   |
| ZIP Code           |      52 | 93241    | 1611.65 | 90065   | 92167.8  | 93060   | 94720    | 95842   |
| Family             |      52 |     2.87 |    0.97 |     1   |     2    |     3   |     4    |     4   |
| CCAvg              |      52 |     2.13 |    1.75 |     0.2 |     1    |     1.8 |     2.32 |     7.2 |
| Education          |      52 |     2.08 |    0.84 |     1   |     1    |     2   |     3    |     3   |
| Mortgage           |      52 |    43.6  |   90.03 |     0   |     0    |     0   |     0    |   314   |
| Personal Loan      |      52 |     0    |    0    |     0   |     0    |     0   |     0    |     0   |
| Securities Account |      52 |     0.12 |    0.32 |     0   |     0    |     0   |     0    |     1   |
| CD Account         |      52 |     0    |    0    |     0   |     0    |     0   |     0    |     0   |
| Online             |      52 |     0.58 |    0.5  |     0   |     0    |     1   |     1    |     1   |
| CreditCard         |      52 |     0.29 |    0.46 |     0   |     0    |     0   |     1    |     1   |


We see that the oldest customer whose Experience is encoded as negative is 29 years old, and the average age of such customers is 24, which suggest that this could be a deliberate choice by the company to encode negative experience. We leave this aside and will discuss later how to deal with this. Other than that, it looks like the dataset is coherent, though not well balanced in the target variable, since only 10% of customers have contracted a loan. We further investigate this in the question 1.



# Question 1

![Loan distribution](image-3.png)

**90.4%** of customers *did not* contract a Loan following the campaign. Anticipating on the final question, this means that using a naive prediction of always predicting "No" gives a 90% accuracy rate, which calls for careful interpretation of the predictive power of the model we'll develop. 


# Question 2

|       |     Age |
|:------|--------:|
| count | 5000    |
| mean  |   45.34 |
| std   |   11.46 |
| min   |   23    |
| 25%   |   35    |
| 50%   |   45    |
| 75%   |   55    |
| max   |   67    |



![Age/EXperience/Icome distr](image-6.png)

[Ecrire un truc]




![Binary variables](image-7.png)

The asymmetry in the distribution of the binary variables is striking.. As said above, less than 10% of customers accepted the personal loan, 10.4% hold a securities account and 6% only a certificate of deposit account. This suggests that most customers do not use the bank's savings and investment products, but they do use other services such as online banking. As said earlier, we need to keep this in mind during the modelisation, since  naive classifier that always predicts "No" will be accurate in 90% of times on average.

## Question 3 

First, we compute the correlation matrix to see which variables we could interact, keeping in mind that our target variable is Loan contracted after the campaign.

![corr_matrix](image-8.png)

First, note that Experience and Age are perfectly correlated, which suggests a quasi-deterministic relathionship. Furthermore, taking into consideration the fact that experience had negative values, we are confident we can drop Experience variable.


Also Income is negatively correlated with Family and Education. 
As expected, Income is strongly correlated with Personal Loan.
Having this in mind we'll interact EducationXIncome with Personal loan, as well as other variables. 