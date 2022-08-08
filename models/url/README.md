# URL Analysis

![AI](https://img.shields.io/badge/AI-Machine--Learning-brightgreen)
![Model](https://img.shields.io/badge/Model-XGB-brightgreen)
![Type](https://img.shields.io/badge/Type-Classifier-brightgreen)
![Aim](https://img.shields.io/badge/Aim-decision-brightgreen)
![stage](https://img.shields.io/badge/stage-1st-brightgreen)

## URL Filteration Machine learing MODEL 

Detection of Malicious URLs by XGB Model Classifier.

### Agenda !
- [Problem Statment.](#problem-statement)
- [Data Set.](#data-set)
- [Importing libraries.](#importing-libraries)
- [Feature Engineering.](#feature-engineering).
- [Split Data.](#split-data)
- [XGBoster Classifier Model.](#XGBoster-classifier-model)
	- Hyperparameters tunning. 
	- Model training & prediction.
	- Ploting Results (features importance, Accuracy , confusion matrix , ROC curve).
- [References.](#references)


### Problem Statement

In this case study, we address the detection of malicious URLs as a multi-class classification problem.
Classify the raw URLs into different class types such as benign or safe URL, defacement URL, phishing URL or malware URL.

As we know machine learning algorithms only support numeric inputs so we will create lexical numeric features from input URLs. So the input to machine learning algorithms will be the numeric lexical features rather than actual raw URLs. If you don't know about lexical features you can refer to this discussion about a lexical feature in StackOverflow.

So, in this case study, we will be using well-known boosting machine learning classifiers namely XGBoost.
That decision came up after testing many other known boosting machine learning classifiers like Light GBM, Gradient Boosting Machines.
XGBoost was the most sufficient and accurate model with best performance parameters.

### Data Set

As known one of the most crucial tasks is to curate the dataset for a machine learning project.
We have searched and looked for a clear and defined dataset curated different sources for more details check the [references](#references).


For training and testing machine learning algorithms, we have used a huge dataset of 651,191 URLs.

![dataset](/assets/url_XGB/data_set.JPG).


Out of which 428103 benign or safe URLs, 96457 defacement URLs, 94111 phishing URLs, and 32520 malware URLs.


Depicts their distribution in terms of percentage.


![dataset_percentage](/assets/url_XGB/dataset_perc.JPG)



### Importing libraries
- pandas
- itertools
- sklearn
	- metrics:
		classification_report, mean_squared_error, confusion_matrix, precision_score, recall_score, auc,roc_curve
	- model_selection:
		train_test_split
- numpy
- random
- math
- matplotlib
- xgboost
- os
- socket
- tld
- urllib
- GridSearchCV
- seaborn

### Feature Engineering

lexical features: whole word, prefix/suffix (various lengths possible), stemmed word, lemmatized word

- Export Dataset with Features (optional)

### Split Data


![dataset_split](/assets/url_XGB/Dataset_split.JPG)


### XGBoster Classifier Model

#### Model parameters

#### Model training.

#### Results ploting 
features importance, Accuracy , confusion matrix


![Confusion matrix](/assets/url_XGB/Confusion_matrix.jpg)

![Features_Importance](/assets/url_XGB/Features_Importance.jpg)


![Multiclass_ROC](/assets/url_XGB/Multiclass_ROC.png)


### References

- For collecting benign, phishing, malware and defacement URLs we have used URL dataset [(ISCX-URL-2016)](https://www.unb.ca/cic/datasets/url-2016.html) and here is the full research [paper](https://www.springerprofessional.de/en/detecting-malicious-urls-using-lexical-analysis/10722098) outlining the details of the dataset and its underlying principles.
- For increasing phishing and malware URLs, we have used Malware domain black list dataset. We have increased benign URLs using faizan git repo At last, we have increased more number of phishing URLs using [Phishtank dataset](https://www.phishtank.com/developer_info.php) and [PhishStorm](https://research.aalto.fi/en/datasets/phishstorm-phishing-legitimate-url-dataset) dataset As we have told you that dataset is collected from different sources. 
- So firstly, we have collected the URLs from different sources into separate dataframe and finally merge them to retain only URLs and their class type.

- The final pre-processed dataset is available at [kaggle](https://www.kaggle.com/code/sid321axn/malicious-url-detection-using-ml-feat-engg/data)