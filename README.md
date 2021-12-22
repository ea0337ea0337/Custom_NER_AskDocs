## Named-Entity Recognition for Patient Queries on r/AskDocs using a custom NER model in spaCy

### Contents
- [Problem Statement](#Problem-Statement)
- [Datasets](#Datasets)
- [Data Dictionary](#Data-Dictionary)
- [Predictive Modeling](#Data-Processing-and-Modeling)
- [Observations](#Observations-from-EDA-and-Prediction-Error-Analysis)
- [Conclusions and Recommendations](#Conclusions-and-Recommendations)
- [Next Steps](#Future-Work)

#### Problem Statement
People increasingly use online forums, subreddits to post their symptoms and seek advice from qualified medical professionals. This voluntary exchange between the patient and medical professionals can be made more efficient.
Extracting meaningful information from patient queries can help in better decision making for all parties involved. This data can also be used to show the viability of the patient - caregiver exchange as a business model. Thus encouraging wider participation by other stakeholders in the healthcare domain.

[Back to Top](#Contents)

#### Datasets
##### Training + Validation dataset
* [`Posts from r/AskDocs for February 2021`](./Data/AskDocs_Feb_2021.csv)
* [`Posts from r/Anxiety for July 2020`](./Data/Anxiety_July_2020.csv)
* [`Posts from r/Anxiety for August 2020`](./Data/Anxiety_Aug_2020.csv)
##### Testing dataset
* [`Posts from r/AskDocs for March 2021`](./Data/AskDocs_Mar_2021.csv) 
* [`Posts from r/Anxiety for September 2020`](./Data/Anxiety_Sep_2020.csv)
* [`Posts from r/Anxiety for October 2020`](./Data/Anxiety_Oct_2020.csv)

[Back to Top](#Contents)

#### Annotation Workflow
1. Manually annotate a seed list of text examples
2. Extract annotated labels from Step 1 and store as keyword patterns
3. Use keyword patterns to pre-annotate the next batch of text data
4. Import pre-annotated text into a annotation software (Doccano). Check pre-annotations and perform manual annotation
5. Export annotated text for model training, evaluation
6. Go to Step 2 for annotating next batch of text



[Back to Top](#Contents)

#### Observations from EDA and Prediction Error Analysis
* Text annotation can be an iterative process. It can done efficiently if labeling guidelines are objectively defined beforehand.

* In general, more annotated data leads to better model performance. Thus, any insights about the data from annotating a previous batch of text become very useful for future annotations.

* The annotator has to pay special attention to label a word given its context.

* Using pre-trained ML models should generally be helpful for modeling.


[Back to Top](#Contents)

#### Data Processing and Modeling
* [`Data Cleaning`](./code/1_Data_Cleaning.ipynb): Clean text data from csv files and save as json files for annotation
* [`Pre-processing Annotated Data`](./code/2_Preprocessing.ipynb): Read-in annotated data and check for alignment. Save in a spaCy readable format.
* [`Model Training`](./code/3_Model_training.ipynb): Build Rule-Based and ML models
* [`Model Evaluation`](./code/4_Evaluation.ipynb): Evaluate model predictions and compare differences across models
* [`Pseudo Rehearsal Data`](./code/generate_rehearsal_data.ipynb): Generate sample data to perform pseudo rehearsal as described below
[Source](https://explosion.ai/blog/pseudo-rehearsal-catastrophic-forgetting)<br>


[Back to Top](#Contents)

#### Conclusions and Recommendations
- A predictive model is built using Logistic Regression with a Test accuracy of 93%,  
- The current model shows reasonable performance scores to be used in a semi-automated manner i.e. if deployed, a human review of model predictions is needed
- Healthcare providers who have a lot of patient data should use this exercise as a Proof-of-Concept and provide this service to people in genuine need of help

[Back to Top](#Contents)

#### Future Work
- Further data cleaning to remove for e.g. case descriptors mandated by forum rules such as  ‘existing relevant medical issue’ in r/AskDocs etc. This will be helpful in finding more meaningful features to understand the dataset
- Try to understand the impact of bi- and tri-grams on model performance. These features occur less frequently than uni-grams but are better at summarizing the context and hence, have more predictive power for the model
- Incorporate Part Of Speech (POS) tags as additional features to improve performance
- Try using word counts, sentiment analysis as features to improve model performance

[Back to Top](#Contents)