### Bio Medical Evaluation Metrics

Accuracy is not the right metrics for evaluating the bio-medical models. A model with accuracy of 80% may be achieved by predicting all as negative in a dataset with 20% positive example.
  
### **Sensitivity** and **Specificity** 
-[ ] **Sensitivity(True Positive):** The probability of a model predicting Positive given the sample as Positive.

-[ ] **Specificity(True Negative):** If the patient is normal what is the probability that the model predicts as Normal.

> **Accuracy = P(correct)  
>Accuracy = Sensitivity x prevalence + Specificity x (1-prevalence)**  


  
  
- **Prevalence:** P(diseases). The presence of the positive data in the dataset. 0.2 if 20% data is positive sample.

### **Positive Predictive Value(PPV)** and **Negative Predictive Value(NPV)**    

-[ ] **Positive Predictive Value(PPV):** The probability that a person is predicted Positive has the the disease.

-[ ] **Negative Predictive Value(NPV):** The probability that a person is predicted Normal has not the disease(Normal).

       
### Correlation of Confusion matrix with these Metrics
![correlation with conf metrics](/Images/bio_med_evaluation_metric.png)
![correlation with conf metrics](/Images/bio_eval_metric.png)
  
  
### ROC (Region Over Curve) Curve:
ROC curve allows to visually plot the Sensitivity of a model against the Specificity of the model at different decision threshold.

> if threshold=0 then sensitivity=0, specificity=1  
> if threshold=1 then sensitivity=1, specificity=0 
  
### Sampling from the total population
It is not feasible to test the model performance on a  hospital with 5000 patients. What we do is take a random sample and calculate the accuracy of the model on that random sample. 
With this accuracy we can calculate the ```range of the accuracy for the total population```. The more sample size we increase the more strict the range is (small interval).
  
**Example:** With 95% confidence, P is in the interval [0.72, 0.88]._      
  This doesn't mean:  
  x => 95% possibility, P lies in the interval []  
  x => 95% of the sample accuracy lies on the interval []. 
  