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


* [`train.csv`](../data/train.csv): Provided train dataset from kaggle
* [`train_clean.csv`](../data/train_clean.csv): Cleaned train dataset 
* [`weather_train_merged.csv`](../data/weather_train_merged.csv): Merged dataframe between weather and train 
* [`merged_train_multi.csv`](../data/merged_train_multi.csv): Merged dataframe with features having multicollinearity for model testing
* [`merged_train_final.csv`](../data/merged_train_final.csv): Merged dataframe with minimal multicollinearity for model testing
* [`final_train.csv`](../data/final_train.csv): Final merged dataframe used for model training
* [`test.csv`](../data/test.csv): Provided test dataset from kaggle 
* [`test_clean.csv`](../data/test_clean.csv): Cleaned test dataset
* [`weather_test_merged.csv`](../data/test_modeling.csv): Merged dataframe between weather and test
* [`merged_tets_multi.csv`](../data/test_modeling.csv): Merged dataframe with features having multicollinearity for model testing
* [`merged_test_final.csv`](../data/test_modeling.csv): Merged dataframe with minimal multicollinearity for model testing
* [`final_test.csv`](../data/test_modeling.csv): Final merged dataframe used for model predictions for kaggle submissions
* [`weather.csv`](../data/test_modeling.csv): Provided weather dataset from kaggle
* [`weather_clean.csv`](../data/test_modeling.csv): Cleaned weather dataset
* [`spray.csv`](../data/test_modeling.csv): Provided spray dataset from kaggle
* [`spray_clean.csv`](../data/test_modeling.csv): Cleaned spray dataset
* [`kaggle_submission.csv`](../data/kaggle_submission.csv): Kaggle Submission with predicted values of `wnvpresent`
* [`base_model_scores.csv`](../data/kaggle_submission.csv): base_model_scores saved from jupyter notebook
* [`nummosquitos_preds.csv`](../data/kaggle_submission.csv): nummosquitos prediction from our `LinearModel` that is used in our final model
* [`sample_Submission.csv`](../data/kaggle_submission.csv): Sample of a submission to kaggle


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


#### EDA:
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
2. However upon exploration of the feature, it turns out that this feature has the highest correlation with our target. Upon testing, we decided to create another `LinearRegression` model to predict the `nummosquitos` feature for our kaggle test dataset.
3. We also made use of Polynomial Features to add different combinations of features that improved our overall correlation with our target. 
4. Considering that mosquitos take 7-14 days to hatch, we added in a time lag feature for `tavg`, `dewpoint` and `preciptotal` as we felt that may potentially have an impact on our model's performance with the target.
5. We decided to experiment with two different datasets, one with multicollinearity between features, and one with minimal multicollinearity.

#### Modeling:
1. We began with Classification models, where we iterated through each dataset and ran base models.
2. Afterwhich we identified the dataset with multicollinearity as the dataset that produced better model scores.
3. Next we Tuned our top models which turned out to be `LogisticRegression`, `AdaBoost`, and `GradientBoost`. 
4. Once tuning was completed, we discussed and made the decision to try modeling with our `nummosquitos` variable. This ended up improving our model score by 4% - 6% which we felt was significant
5. As such we decided to create a `LinearRegression` model to predict the `nummosquitos` feature for our kaggle test dataset. We tested out different models such as `LinearRegression`, `Ridge`, `Lasso`, and `ElasticNet`. We kept this part fairly simple as we found that the model seemed to perform well.
6. Armed with all the features we felt were necessary, we ran our final model again and ended up with the scores mentioned above for our final model, afterwhich we proceeded for Kaggle submission.

---

#### Conclusions and Recommendations:

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
