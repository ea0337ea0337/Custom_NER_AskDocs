## Named-Entity Recognition for Patient Queries on r/AskDocs using a custom NER model in spaCy

### Contents
- [Problem Statement](#Problem-Statement)
- [Datasets](#Datasets)
- [Custom NER Model](#Data-Processing-and-Modeling)
- [Observations](#Observations-from-EDA-and-Prediction-Error-Analysis)
- [Results and Use Cases](#Results-and-Use-Cases)
- [Next Steps](#Future-Work)

#### Problem Statement
People increasingly use online forums, subreddits to post their symptoms and seek advice from qualified medical professionals. This voluntary exchange between the patient and medical professionals can be made more efficient.
Extracting meaningful information from patient queries can help in better decision making for all parties involved. This data can also be used to show the viability of the patient - caregiver exchange as a business model. Thus encouraging wider participation by other stakeholders in the healthcare domain.

[Back to Top](#Contents)

#### Datasets
##### Training + Validation files
* [`Annotated Training Data`](./data/json/nsamples_480_v2_2021_6m_doccano.jsonl)
* [`Annotated Validation Data`](./data/json/val_nsamples_240_v2_doccano.jsonl)
* [`Patterns from first batch of annotation`](./data/patterns/old_patterns_240_NS.csv)
* [`Patterns from latest Rule-Based Model`](./data/patterns/old_patterns_480_NS_v2.csv) 
* [`Training data for Pseudo Rehearsal`](./data/json/nlp_rehearsal_1000.json)
* [`Validation data for Pseudo Rehearsal`](./data/json/test_nlp_rehearsal_1000.json)

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

#### Results and Use Cases
- Customer NER models were trained in spaCy as a proof-of-concept
- For the limited training data, the Rule-Based model performed similar to the ML models.
- Models need to perform better to be deployable in a useful way
- A robust NER model can help in characterizing patient queries better
- Correctly identified health conditions can prompt the patient towards immediate medical attention is needed.
- Patient demographic data, if extracted properly, can be used to get broad insights into people’s health by gender / age / race / location etc.


[Back to Top](#Contents)

#### Future Work
- Annotate more data for model training, validation
- Add frequently occurring and easily identifiable labels such as drug dosage
- Include the word doctor / Dr. / etc. which was accidentally omitted during clean-up
- Use domain specific pre-trained spacy models, if available
- Identify similar, but large annotated datasets and use as an intermediate step in model training; fine-tune model using custom dataset
- Use data augmentation to make the model more robust to misspelling etc.


[Back to Top](#Contents)