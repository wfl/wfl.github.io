
## Project Mcnulty: Stroke Prediction


### Motivation: 

According to the [CDC](https://www.cdc.gov/stroke/index.htm), about 795,000 Americans have a stroke every year, making stroke the fifth leading cause of death and the major cause of long-term serious disability. An effective way of lowering stroke risk is healthy living. Thus, [as much as eighty percent](https://www.cdc.gov/stroke/healthy_living.htm) of the 795,000 stoke cases could be prevented if these patients were informed of their risk early enough for them to have a chance to make healthy changes in their lifestyle. 

This project’s goal is to a model or models that can predict whether a patient has high risk of having a stroke.
 

### Data

The dataset train_2v.csv, published on the [Kaggle webpage]( https://www.kaggle.com/asaumya/healthcare-dataset-stroke-data), was provided by the McKinsey & Company for a 1 day hackathon event held by Analytics Vidhya. It contains 43400 patients’ information that are represented by the variables listed in the table below:

| Feature or Target | Variable | Type | Brief description |
| :---------------: | :--------:  | :--------:  | :--------------------------------------------------- |
| Neither | id | label | Patient ID |
| Feature | gender | nominal categorical | Gender of the patient  (male, female, other) |
| Feature | age | discrete numerical | Patient’s age |
| Feature | hypertension | nominal categorical | Suffering from hypertension 
(0 – No, 1 – Yes) |
| Feature | heart_disease | nominal categorical | Presence of heart disease 
(0 – No, 1 – Yes) |
| Feature | ever_married | nominal categorical | Yes or No |
| Feature | work_type | nominal categorical | Type of occupation
(private, self-employed, children, govt_job, & never_worked) |
| Feature | residence_type | nominal categorical | Area type of resident 
(urban or rural) |
| Feature | avg_glucose_level | continuous numerical | Average glucose level after a meal |
| Feature | bmi | continuous numerical | Body mass index |
| Feature | smoking_status | nominal categorical | Smoking status
(never smoked, Formerly smoked, Smokes) |
| Target | stroke | nominal categorical | Has suffered or is suffering from stroke (0 – No, 1 – Yes) |

There are missing data in the bmi and smoking_status variables. There is about 30.63% (i.e., 13292 of 43400) of missing smoking_status data, a large percentage that warrants exclusion of smoking_status variable. For this project, two datasets were generated from the original train_2v dataset. The first dataset, named as dataset 1, is the full train_2v dataset that its 40% (i.e., 5499 of 13292) of the missing observations in the smoking_status variable were replaced with “never smoke” because these 5499 missing observations (See Figure 1) are patients younger than 15 years old who are unlikely to smoke, according to the findings reported in [this article](https://www.cnn.com/2018/08/22/health/cigarette-smoking-teens-parent-curve-intl/index.html). The second dataset, named as dataset 2, only contains patients who have never smoked. Since there are only 3.37% of the missing bmi observations in the original train_2v dataset, all these missing data in two datasets (i.e., dataset 1 and dataset 2) were replaced with median value of the bmi data.

![Figure 1: Distribution of missing values for the variable Age](https://github.com/wfl/healthcare_stroke_classification/blob/master/figures/histogram_age_missing_data.png)



### Approach

Both datasets display a serious level of class imbalance (See Figure 2). 98.21% of the data are labeled with “No”, which is the majority class that represents patients who haven’t suffered a stroke yet; while the 1.79% of the data with the “Yes” label is the minority class that represents patients who have had a stroke. This disproportionate ratio of class labels is a problem for this case because the machine learning algorithms are designed to minimize the cost function that results in biasing towards the majority class. It is an undesirable outcome where the classifiers incorrectly predict patients who have high risk of having a stroke to no risk at all. 

![Figure 2: Distribution of patients who have never and had suffered from stroke](https://github.com/wfl/healthcare_stroke_classification/blob/master/figures/histogram_stroke.png)

To mitigate this issue, [weight adjustment in the cost function, and resampling strategies such as oversampling and synthesis of the minority class to restore class balance in the training dataset](http://www.svds.com/learning-imbalanced-classes/) were implemented with the four selected classifiers. The four selected classifiers are Logistic Regression (LR), Linear Support Vector Classifier (SVC) with approximate RBF kernel map, Random Forest Classifier (RF), and Gradient Boosting Classifier (GBC). The resampling strategies are SMOTE. ADASYN, and Random Over Sampler (ROS). 

Each of the two datasets was split into training and test sets with the 75:25 ratio. Then the classifiers were trained on the training data via using the stratified 10-fold cross-validation approach. At the same time, their hyperparameters were also optimized. To evaluate the classifiers’ performance, the average performance scores (i.e., precision, recall, and F1-scores) were calculated on the validation sets. Additionally, the same performance scores, confusion matrix, roc curves, and precision-recall curves were generated for the test scores. 


### Result Summary and Thoughts

• By implementing methods to handle the class imbalance problem, the precision score increases by 4% and the recall scores increases between 50% and 60% when comparing to the base models (i.e., the classifiers without implementation of any resampling strategy or weight penalty adjustment method).

• The confusion matrices of Logistic Regression+weight adjustment, Random Forest Classifier+weight adjustment, Support Vector Classifier+ROS, and Gradient Boosting Classifier+ROS have the most True Positives and the least False Negatives for test dataset 1 (i.e., full dataset with smoking status). For test dataset 2 (i.e. dataset with patients who have never smoked), the stroke results predicted by Logistic Regression+ROS, Support Vector Classifier+ROS,, Gradient Boosting Classifier+ROS, and Forest Classifier+weight adjustment were the best. These conclusions are supported by the high recall scores of at least 0.8, which were calculated for these test datasets.

• The ROC curves for most of the classifiers have at least an AUC value of 0.8. They are insensitive to the class distribution. The best models’ ROC curves for each dataset are shown in Figures 3 and 4 below.  

![Figure 3: Best models’ ROC for dataset 1 (with smoking status)](https://github.com/wfl/healthcare_stroke_classification/blob/master/figures/ROC_bestmodels_dataset1_plot.png)  ![Figure 4: Best models’ ROC for dataset 2 (with only patients who have never smoked)](https://github.com/wfl/healthcare_stroke_classification/blob/master/figures/ROC_bestmodels_dataset2_plot.png)

• The best models’ precision-recall curves (See Figures 5 and 6 below) indicated that the improvement of the sensitivity (recall) of the classifiers would reach to a point where it won’t contribute in improving the precision score, which remains a low constant value. One explanation would be that increased sensitivity in predicting the 2% minority class would result in predicting more false positives. High number of false positives and low number of true positives yield low precision score. For this project, ability to predict stroke correctly is the priority because it will save patients who are at high risk of having a stroke. The false positives are just the false alarms that don’t negatively impact the patients’ wellbeing by advising them to choose a healthy lifestyle choice.

![Figure 5: Best models’ precision-recall curves for dataset 1 (with smoking status)](https://github.com/wfl/healthcare_stroke_classification/blob/master/figures/PrecRec_bestmodels_dataset1_plot.png)  ![Figure 6: Best models’ precision-recall curves for dataset 2 (with only patients who have never smoked)](https://github.com/wfl/healthcare_stroke_classification/blob/master/figures/PrecRec_bestmodels_dataset2_plot.png)

• Finally, the feature importance that was generated by Random Forest and Gradient Boosting classifiers was examined for both datasets. In general, age is the primary attribute for predicting stroke. For the full dataset (Figure 7), both classifiers indicate that bmi and avg_glucose_level were the secondary important attributes; although Random Forest also indicates that patients’ marital status, specific worktype, heart disease and hypertension are the also the main feature for prediction. However, for dataset 2 (Figure 8), both classifiers indicate that patient's marital status, avg_glucose_level, heart disease, hypertension are the secondary features that indicate whether a patient who has never smoked would have a high risk of stroke.

![Figure 7: Feature importance for dataset 1 (with smoking status)](https://github.com/wfl/healthcare_stroke_classification/blob/master/figures/feature_importance_dataset1_plot.png)  ![Figure 8: Feature importance for dataset 2 (with only patients who have never smoked)](https://github.com/wfl/healthcare_stroke_classification/blob/master/figures/feature_importance_dataset2_plot.png)
 

Please visit my [github repository]( https://github.com/wfl/healthcare_stroke_classification) for the detail implementation of this project.  Thank you.  





