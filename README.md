# ![](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png) Project 4: West Nile Virus Classification


### Contents:
- [Problem Statement](#Problem-Statement)
- [Datasets](#Datasets)
- [Proposed Model](#Proposed-Model)
- [Summary of Analysis](#Summary-of-Analysis)
- [Recommendations](#Recommendations)
- [Future Works](#Future-Works)
- [Sources](#Sources)

---

### Problem Statement


---

### Datasets


* [`train.csv`](datasets/train.csv): Provided train dataset
* [`train_cleaned.csv`](datasets/train_cleaned.csv): Cleaned train dataset 
* [`train_modeling.csv`](datasets/train_modeling.csv): Test dataset ready to be used for modeling  
* [`test.csv`](datasets/test.csv): Provided test dataset 
* [`test_cleaned.csv`](datasets/test_cleaned.csv): Cleaned test dataset
* [`test_modeling.csv`](datasets/test_modeling.csv): Test dataset ready to be used for modeling 
* [`kaggle_submission_sujayjangam.csv`](datasets/kaggle_submission_sujayjangam.csv): Kaggle Submission with predicted values of `SalePrice`

---
### Proposed Model
- Our final model is a `LogisticRegression` model with the following scores.
    * Recall Score: 68.5%
    * Test_AUC Score: 85%

---

### Summary of Analysis

#### Cleaning:
1. First of all, I explored the data files provided. It was clear that there were many missing data, 9,822 to be exact. In a file with 2051 rows, this was an alarming number of missing data.
2. Most of the missing data was interpreted from the Data Dictionary, and could be interpreted to 'None' values. However for `Lot Frontage` we had to impute the values using the mean Square Footage in each neighborhood.


#### Encoding, EDA and Feature Selection:
1. Once the files were all clean, we set about carrying out manual encoding or one-hot encoding. This needs to be done so that we may feed features from our data set into our model.
2. Along the way, we dropped columns where we saw little to no variance. Columns where there was indication of multicollinearity were also dropped, keeping one of them.
3. Upon investigation of trends in the data, we saw that there were strong indicators of correlation in 4 main groups with our target, `SalePrice`.
    - Square Footage
    - Quality of the Property
    - Location
    - Others, e.g. Age of property
4. We isolated features that fell into these groups, and eventually begain the modeling process.

#### Feature Engineering:
1. We noticed that our test dataset did not have the `nummosquitos` feature. 

#### Modeling:
1. We began with Linear Regression models, where we iterated through starting with a small number of features, and eventually including all the features that were in our 4 groups above.
2. We move to using Regularisation and found that the Lasso model had the best Root Mean Squared Error of 27,904.
3. After creating our final model, we fed all our remaining data into our final model and generate the predictions for Kaggle.
4. Afterwhich, we dove back into our final model to gain insights towards our problem statement.

---

### Conclusions and Recommendations:

- Factors such as months, number of mosquitoes, dewpoint (humidity), location and temperature play an important role in predicting West nile Virus Infections.

- We propose to conduct sprays on identified hotspots during the months of July, August and September

**Summary of Spray costs illustrated on 2013 dataset:**

|Item|Cost|
|---|---|
|Total Savings for 3 human infections per WNV incidence|+ \$ 3,401,700|
|Spray cost for 6 weeks| - \$ 2,858,536|
|Total cost savings| + \$ 543,163|
|Cost saving per additional human infection after 3 infections| + \$ 1,133,900|

#### Recommendations

- Optical sensing based counting system to ensure consistent sampling

- Do greater checks on mosquitos on banks of Lake Michigan from Jul - Sep

#### Future Works

- Finetune Linear Regression model to better predict number of mosquitos
- Further hyper parameter tuning, or even use of other blackbox models such as Neural Networks.


---

### Sources

1. https://academic.oup.com/jme/article/56/6/1456/5572378

- Learnt about the healthcare issues arising from WNV.

2. https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3322011/

- Reference for case study to calculate associated costs of WNV

3. https://academic.oup.com/jme/article/56/6/1456/5572378

- Learnt about the worst time periods for WNV

4. https://link.springer.com/article/10.1007/s00340-019-7361-2

- Learnt about counting mosquitos through optical sensing
