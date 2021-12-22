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
* Model is able to discern a lot of key descriptive words representative of each subreddit for e.g. panic, attack, blood, test
* As expected, keywords for r/Anxiety lean towards emotionally descriptive words
* Keywords for r/AskDocs lean more towards health issues and conecerns
* Word counts for each submission are lower on average for posts in r/AskDocs
* Reviewing posts for False Positives, False Negatives gives some clues about model behavior

[Back to Top](#Contents)

#### Data Processing and Modeling
* [`Pushshift API Wrapper`](./Code/pushshift_get_data_clean.ipynb): Functions to use the Pushshift API to query and gather reddit posts from subreddits
* [`Data Cleaning, Modeling and Evaluation`](./Code/P3_09Oct2021_clean.ipynb): End-to-End modeling workflow including text data clean-up and tokenizing
* [`Expand Word Contractions`](./Code/contractions.py): Expand word contractions to complete words for easier text processing
[Source](https://towardsdatascience.com/a-practitioners-guide-to-natural-language-processing-part-i-processing-understanding-text-9f4abfd13e72)<br>

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