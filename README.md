### Table of Contents

1. [Installation](#installation)
2. [Project Motivation](#motivation)
3. [File Descriptions](#files)
4. [Results](#results)
5. [Licensing, Authors, and Acknowledgements](#licensing)

## Installation
There should be no necessary libraries to run the code here beyond the Anaconda distribution of Python.  The code should run with no issues using:
 - Python 3.6
 - Spark 2.4

 I have used the [IBM Watson Studio](https://www.ibm.com/cloud/watson-studio) to set up a Spark Cluster and notebook. The selected environment was "Default Spark 2.4 & R 3.6" With the free plan you can use this service for about 30 hours per month.
 
### Data
Here ist an overview about the available sizes of the event data. Because of free usage limitation I was only able to use the medium_sparkify_event_data on Watson Studio.
| Dataset | size |active customer | churned customer | churn rate |
| ------ | ------ | ------ | ------ | ----- |
| [mini_sparkify_event_data](https://github.com/m444x/churn-prediction-sparkify/raw/master/medium-sparkify-event-data.zip)| 128MB| 173 | 52 | 23,1% |
| [medium_sparkify_event_data](https://video.udacity-data.com/topher/2018/December/5c1d6681_medium-sparkify-event-data/medium-sparkify-event-data.json) | 250MB| 349 | 99 | 22,1%|
|Full dataset on S3 Bucket*|12GB|N/A|N/A|N/A|

*s3n://udacity-dsnd/sparkify/sparkify_event_data.json

### Structure
```sh
root
 |-- artist: string (nullable = true)
 |-- auth: string (nullable = true)
 |-- firstName: string (nullable = true)
 |-- gender: string (nullable = true)
 |-- itemInSession: long (nullable = true)
 |-- lastName: string (nullable = true)
 |-- length: double (nullable = true)
 |-- level: string (nullable = true)
 |-- location: string (nullable = true)
 |-- method: string (nullable = true)
 |-- page: string (nullable = true)
 |-- registration: long (nullable = true)
 |-- sessionId: long (nullable = true)
 |-- song: string (nullable = true)
 |-- status: long (nullable = true)
 |-- ts: long (nullable = true)
 |-- userAgent: string (nullable = true)
 |-- userId: string (nullable = true)
```

## Project Motivation<a name="motivation"></a>
This project is the capstone of the Udacity Machine Learning with PyTorch and Data Scientist Nanodegree Program. For me, it was important to learn how to manipulate large and realistic datasets with Spark to engineer relevant features for predicting churn. 

To deal with this large amount of data, I was also able to learn how to use Spark MLlib to build machine learning models with large datasets.  Far beyond what could be done with non-distributed technologies like scikit-learn.

**Career Relevance**

Predicting churn rates is a challenging and common problem that data scientists and analysts regularly encounter in any customer-facing business. Additionally, the ability to efficiently manipulate large datasets with Spark is one of the highest-demand skills in the field of data.

**Developing skills**
- Loading large datasets into Spark and manipulating them using Spark SQL and Spark Dataframes
- Using the machine learning APIs within Spark ML to build and tune models
- Integrating the skills I've learned in the Spark course and the Data Scientist Nanodegree program

## File Descriptions <a name="files"></a>

There is one notebook available [here](https://github.com/m444x/churn-prediction-sparkify/blob/master/Sparkify.ipynb) to showcase work related to the above questions.  This notebook is exploratory in searching through the data pertaining to the questions showcased by the notebook title.  


## Results<a name="results"></a>
After investigating the unique values of the columns I could transform the categorical values for machine learning. Here is an overview for the colums used in this model: 

```sh
root
 |-- artist: string (nullable = true)
 |-- auth: string (nullable = true)
        |-- Cancelled
        |-- Logged In
 |-- firstName: string (nullable = true)
 |-- gender: string (nullable = true)
        |-- F
        |-- M
 |-- itemInSession: long (nullable = true)
 |-- lastName: string (nullable = true)
 |-- length: double (nullable = true)
 |-- level: string (nullable = true)
        |-- free
        |-- paid
 |-- location: string (nullable = true)
        |-- Akron, OH
        |-- Albany, OR
        |-- Albany-Schenectad...
        |-- Albemarle, NC
        |-- Alexandria, LA
        |-- ......
 |-- method: string (nullable = true)
 |-- page: string (nullable = true)
        |-- About
        |-- Add Friend
        |-- Add to Playlist
        |-- Cancel
        |-- Cancellation Conf..
        |-- Downgrade
        |-- Error
        |-- Help
        |-- Home
        |-- Logout
        |-- NextSong
        |-- Roll Advert
        |-- Save Settings
        |-- Settings
        |-- Submit Downgrade
        |-- Submit Upgrade
        |-- Thumbs Down
        |-- Thumbs Up
        |-- Upgrade
 |-- registration: long (nullable = true)
 |-- sessionId: long (nullable = true)
 |-- song: string (nullable = true)
 |-- status: long (nullable = true)
        |-- 200
        |-- 307
        |-- 404
 |-- ts: long (nullable = true)
 |-- userAgent: string (nullable = true)
        |-- Mozilla/5.0 (Mac...
        |-- Mozilla/5.0 (Mac...
        |-- Mozilla/5.0 (Mac...
        |-- .....
 |-- userId: string (nullable = true)
```

### Full list of features
```sh
root
 |-- userId: string (nullable = true)
 |-- num_session: integer (nullable = true)
 |-- avg_actions_session: float (nullable = false)
 |-- sum_songs: integer (nullable = true)
 |-- avg_songs_session: float (nullable = false)
 |-- sum_session_min: integer (nullable = true)
 |-- avg_session_min: float (nullable = false)
 |-- sum_days_free: float (nullable = false)
 |-- avg_days_free: float (nullable = false)
 |-- sum_days_paid: float (nullable = false)
 |-- avg_days_paid: float (nullable = false)
 |-- count_404: integer (nullable = true)
 |-- num_downgrades: integer (nullable = true)
 |-- count_Upgrade: integer (nullable = true)
 |-- count_Thumbs_Up: integer (nullable = true)
 |-- count_Thumbs_Down: integer (nullable = true)
 |-- count_Submit_Upgrade: integer (nullable = true)
 |-- count_Submit_Downgrade: integer (nullable = true)
 |-- count_Settings: integer (nullable = true)
 |-- count_Save_Settings: integer (nullable = true)
 |-- count_Roll_Advert: integer (nullable = true)
 |-- count_NextSong: integer (nullable = true)
 |-- count_Logout: integer (nullable = true)
 |-- count_Home: integer (nullable = true)
 |-- count_Help: integer (nullable = true)
 |-- count_Error: integer (nullable = true)
 |-- count_Downgrade: integer (nullable = true)
 |-- count_Add_to_Playlist: integer (nullable = true)
 |-- count_Add_Friend: integer (nullable = true)
 |-- count_About: integer (nullable = true)
 |-- gender: integer (nullable = true)
 |-- num_actions: integer (nullable = false)
 |-- registered_days: integer (nullable = true)
```
### Results on the Train set
|Parameter|Logistic Regression| One Vs Rest | RandomForest Classifier | DecisionTree Classifier | Gradient Boost |
| ----- | ----- | ----- | ----- | ----- | ----- |
| Test Error|0.166197|0.166197| 0.0112676 | 0.132394 | **0.0056338** |
| Accuracy | 0.833803|0.833803| 0.988732 | 0.867606 | **0.994366** | 
| F1 | 0.820551 | 0.820551|0.988627| 0.858031 | **0.99434** |
| WeightedPrecision | 0.821194 | 0.821194 | 0.988893| 0.861683 | **0.994407** |
| Weighted Recall | 0.833803 | 0.833803| 0.988732| 0.867606 | **0.99436**6 |

### Results on the Test set

|Parameter|Logistic Regression| One Vs Rest | RandomForest Classifier | DecisionTree Classifier | Gradient Boost |
| ----- | ----- | ----- | ----- | ----- | ----- |
| Test Error|**0.129032**|**0.129032**| 0.16129 | 0.182796 | 0.16129 |
| Accuracy | **0.870968**|**0.870968**| 0.83871 | 0.817204 | 0.83871 | 
| F1 | **0.862129** | **0.862129** |0.833795| 0.811634 | 0.846659 |
| WeightedPrecision | 0.864493| 0.864493 | 0.831145| 0.808166 | **0.864813** |
| Weighted Recall | **0.870968** | **0.870968**| 0.83871| 0.817204 | 0.83871 |

As we can see Gradient Boost can predict the Training Data perfectly. It seems the model is overfitted to the Training set.  In comparison with the Test set Logistic Regression and One vs. rest do have the best result in the metrics. But the value of Weighted Precision is remarkable.

Let us check the results deeper in a Confusion Matrix:

### Confusion Matrix Logistic Regression vs. GradientBoost
|Churned|Logistic: 0|Logistic: 1|Gradient: 0|Gradient: 1|
|--------|---|---|---|---|
|1|9|11|4|16|
|0|70|3|62|11|

Although the metrics are better overall in the Logistic Regression, and not churning customers were predicted correctly. But customers who want to leave us have identified more accurately in the GradientBoost model. 

The main goal of our model ist to find users they will leave our services and prevent churning. So in our case, it is important to find as many true-positives as possible and reduce false-negative values. 
The impact of false-positive values could be a financial impact on the company. Users who do not want to churn will receive a special discount. Such measures can also lead to higher customer satisfaction 

A detailed description can be found at the post available [here](https://medium.com/@bluebullet/what-coding-skills-are-expected-in-germany-in-2020-e19281dddf6b).

## Licensing, Authors, Acknowledgements<a name="licensing"></a>

Must give credit to Udacity for the Sparkify Event data. 

Spark MLlib: Main Guide
- [ML Tuning: model selection and hyperparameter tuning](https://spark.apache.org/docs/2.4.0/ml-tuning.html#ml-tuning-model-selection-and-hyperparameter-tuning)
- [Classification and regression](https://spark.apache.org/docs/2.4.0/ml-classification-regression.html#classification-and-regression)

Otherwise, feel free to use the code here as you would like! 
