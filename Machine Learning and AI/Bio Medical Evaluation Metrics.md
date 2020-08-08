### Bio Medical Evaluation Metrics

Accuracy is not the right metrics for evaluating the bio-medical models. A model with accuracy of 80% may be achieved by predicting all as negative in a dataset with 20% positive example.
  
**Sensitivity** and **Specificity** are two terms commonly used in medical application of ML. 

- **Sensitivity(True Positive):** The probability of a model predicting Positive given the sample as Positive.

- **Specificity(True Negative):** If the patient is normal what is the probability that the model predicts as Normal.

> **Accuracy = P(correct)  
>Accuracy = Sensitivity x prevalence + Specificity x (1-prevalence)**  

- **Prevalence:** P(diseases). The presence of the positive data in the dataset. 0.2 if 20% data is positive sample.

#### **Positive Predictive Value(PPV):** 
The probability that a person is predicted Positive has the the disease.

### **Negative Predictive Value(NPV)**    
The probability that a person is predicted Normal has not the disease(Normal).


### Correlation of Confusion matrix with these Metrics
![correlation](/Images/bio_med_evaluation_metric.png)