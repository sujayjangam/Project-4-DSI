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
West Nile Virus is one of the most prominent vector-borne diseases affecting Chicago, with 225 human fatalities occurring in the year following its first detection in the local avian species in 2001<sup> 1 </sup>. 

In the years since, the Chicago Department of Public Health and the Centers for Disease Control and Prevention have been managing the prevalence of the disease well. However, the recent epidemic (2013) has begun to call into question if the money spent on control measures is being used efficiently.

To this end, our team at (DATA-SCIENCE) has been tasked to use past mosquito collection and weather data of the city of Chicago to create a classifying model that is able to predict the presence of West Nile Virus based on location and weather so as to effectively assign resources to control the spread of the West Nile Virus.

Additionally, the Chicago City Council has also requested for a cost benefit analysis to be conducted on the effects of its insecticide spraying to see if its application is a valuable use of their budget.

**Source:**
<br>
Chicago 2013 West Nile Virus Report
<br>
https://www.chicago.gov/content/dam/city/depts/cdph/statistics_and_reports/CDInfo_2013_JULY_WNV.pdf

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
The most notable point of the EDA was that the data sampling was very inconsistent both time-wise and location-wise. With this in mind, we did not expect our model to be of high quality.

#### Cleaning:
1. Change the date on every dataframe to datetime
2. Create 'year', 'month', 'week', and 'weekday' columns from the date
3. Change the column names on every dataframe to lowercase
4. Drop duplicates from training data
5. Drop 'Time' column from spray data
6. Replace missing values ('-' and 'M') in weather data with NA
7. Fill 'tavg' NA values with average temperature calculated from 'tmax' and 'tmin' in weather data
8. Drop 'water1' column for weather data
9. Drop 'depth' column in weather data since it is just a linear transformation of 'station'
10.Drop 'snowfall' column in weather data since no snow during months present in our training data
11.Drop 'depart' column from weather data
12.ffill for sunset and sunrise NA values
13.Reformat time in 'sunrise' and 'sunset' columns and convert to datetime
14.Replace erroneous values in 'sunset' based on educated guesses
15.Replace blank 'codesum' value with 'moderate' according to noaa_weather_qclcd_documentation
16.Separate codesum values
17.Fill remaining NA values with median of their respective columns
18.Save all files as clean versions

#### EDA:
The EDA indicated that in terms of weather, temperature and humidity had a positive correlation with the number of mosquitoes and, through that, the presence of WNV. Additionally, we could also see particularly in the 2013 data, that insecticide spraying was effective in reducing the total and WNV number of mosquitoes. However, we also realised that the spraying done in both years was not really targeted and could have been improved by using the metric of rate of infection per trap, and targeting these particular locations instead. 

Through this metric, we could identify on the map of Chicago the neigbourhoods that had the highest proportions of WNV mosquitoes, noting that they were close to areas that were easily identifiable as ideal breeding grounds for both the reservoir and the vector species of WNV e.g. LaBagh Woods, River Park, and Lincoln Zoo; basically any large area of land where it would be unlikely to have sufficiently thorough groundskeeping to remove mosquito breeding spots.

From this insight, we would expect that further location-based features to arise if we were to keep using the 'WNV mosquito infection rate' to identify hotspots.

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

Spray Calculatively
- By spraying in locations that are known to have high rate of WNV mosquitoes, we expect to more efficient with our spraying, exterminating more WNV-infected mosquitoes than otherwise

Alternate Insecticides
- Using stronger insecticides such as Malathion may prove to be more effective

Alternate Insecticide Dispersal Methods
- Aerial spraying or community-based larvicide application has been shown to be effective in managing the mosquitoes better

Improve Data Collection
- Using a sensor that is capable of identifying and differentiating mosquitoes could help our poor data sampling issue and reduce inconsistencies in the sample collection

#### Future Works

- Finetune Linear Regression model to better predict number of mosquitos
- Further hyper parameter tuning, or even use of other blackbox models such as Neural Networks.

---

### External Research

1. https://academic.oup.com/jme/article/56/6/1456/5572378

- Learnt about the healthcare issues arising from WNV.

2. https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3322011/

- Reference for case study to calculate associated costs of WNV

3. https://academic.oup.com/jme/article/56/6/1456/5572378

- Learnt about the worst time periods for WNV

4. https://www.chicago.gov/city/en/depts/cdph/provdrs/healthy_living/news/2021/august/city-to-spray-insecticide-wednesday-to-kill-mosquitoes.html

- 2021 Chicago Zenivex Spraying

5. https://www.centralmosquitocontrol.com/-/media/files/centralmosquitocontrol-na/us/specimen%20labels/zenivex%20e20%20specimen%20label.pdf

- Zenivex Product Label

6. https://pubmed.ncbi.nlm.nih.gov/23883850/

- Larvicide Effectiveness

7. https://www.ncbi.nlm.nih.gov/pmc/articles/PMC2661378/

- Efficacy of Community-based Larvicide Application

8. https://link.springer.com/article/10.1007/s00340-019-7361-2

- Optical Sensor-based Counting System

9. https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0179673

- Insecticide Resistance to Permethrin and Malathion

10. https://www.science.org/content/article/after-40-years-most-important-weapon-against-mosquitoes-may-be-failing

- Mosquito Resistance to Pyrethroid Insecticides

11. https://www.cmmcp.org/pesticide-information/pages/zenivex-e4-etofenprox

- Zenivex Information

12. http://npic.orst.edu/factsheets/archive/malatech.html

- Malathion Information

