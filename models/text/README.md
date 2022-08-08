# TEXT Analysis

![AI](https://img.shields.io/badge/AI-Machine--Learning-brightgreen)
![Model](https://img.shields.io/badge/Model-MultinomialNB-brightgreen)
![Type](https://img.shields.io/badge/Type-NPL-brightgreen)
![Aim](https://img.shields.io/badge/Aim-decision-brightgreen)
![stage](https://img.shields.io/badge/stage-2nd-brightgreen)

## TEXT Filteration Machine learing MODEL 

Detection of suspected Emails by MultinomialNB Model Classifier.

### Agenda !
- [Problem Statment.](#problem-statement)
- [Data Set.](#data-set)
- [Importing libraries.](#importing-libraries)
- [Feature Engineering.](#feature-engineering)
	- Word Cloud
- [Split Data.](#split-data)
- [MultinomialNB Classifier Model.](#MultinomialNB-classifier-model)
	- Hyperparameters tunning.
	- Model training & prediction.
	- Ploting Results (features importance, Accuracy , confusion matrix , ROC curve).
- [References.](#references)

### Data Set

#### About Data

For training and testing machine learning algorithms, we have used a huge dataset of 651,191 URLs, out of which 428103 benign or safe URLs, 96457 defacement URLs, 94111 phishing URLs, and 32520 malware URLs.
Figure 2 depicts their distribution in terms of percentage. 
As we know one of the most crucial tasks is to curate the dataset for a machine learning project. We have curated this dataset from five different sources.

For collecting benign, phishing, malware and defacement URLs we have used URL dataset [(ISCX-URL-2016)](https://www.unb.ca/cic/datasets/url-2016.html) and here is the full research [paper](https://www.springerprofessional.de/en/detecting-malicious-urls-using-lexical-analysis/10722098) outlining the details of the dataset and its underlying principles.
For increasing phishing and malware URLs, we have used Malware domain black list dataset. We have increased benign URLs using faizan git repo At last, we have increased more number of phishing URLs using [Phishtank dataset](https://www.phishtank.com/developer_info.php) and [PhishStorm](https://research.aalto.fi/en/datasets/phishstorm-phishing-legitimate-url-dataset) dataset As we have told you that dataset is collected from different sources. 
So firstly, we have collected the URLs from different sources into separate dataframe and finally merge them to retain only URLs and their class type.

The final pre-processed dataset is available at [kaggle](https://www.kaggle.com/code/sid321axn/malicious-url-detection-using-ml-feat-engg/data)

#### Problem Statement

In this case study, we address the detection of malicious URLs as a multi-class classification problem. In this case study, we classify the raw URLs into different class types such as benign or safe URL, phishing URL, malware URL, or defacement URL.

As we know machine learning algorithms only support numeric inputs so we will create lexical numeric features from input URLs. So the input to machine learning algorithms will be the numeric lexical features rather than actual raw URLs. If you don't know about lexical features you can refer to this discussion about a lexical feature in StackOverflow.

So, in this case study, we will be using three well-known boosting machine learning classifiers namely XGBoost, Light GBM, Gradient Boosting Machines.


### Feature Engineering.

lexical features: whole word, prefix/suffix (various lengths possible), stemmed word, lemmatized word

Visiualization for Words weight in HAM emails in our dataset
![dataset](/assets/text_MultinomialNB/HAM_WORDs_TEXT.jpg).

Visiualization for Words weight in Spam emails in our dataset

![dataset](/assets/text_MultinomialNB/SPAM_WORDs_TEXT.jpg).

ROC-CURVE 

![dataset](/assets/text_MultinomialNB/ROC_CURVE_TEXT.jpg).


### Export Dataset with Features (optional)

### Split Data.

### MultinomialNB Classifier Model

#### Model parameters

#### Model training.

#### Results ploting 
features importance, Accuracy , confusion matrix 
