### Challenges of Bio Medical Image Processing
1. **Class Imbalance:** Less number of positive example commonly. Need to use the weighted loss to handle this. Which will keep the same effect of the loss for both positive and negative example. Weights can be calculated as p/N and n/N.
2. **Data Leakage:** Same patient record in multiple distributions like Train, Test and Validation set. Make the distribution such that no patient is repeated. Taking a set of different patients for different distribution. 
3. **Multiple Class:** This is not convenient to use multiple model for multiple diseases prediction. The loss is calculated as the weighted loss of the individual class. 
4. **Small Dataset size:** Medical datasets are normaly (~10k-100k) size. Suitable Data augmentation can be used. 


- **AUC(Area Under Curve)** is used for evaluating model. More the cureve in top left more area under curve and better the model. 

- **GradCam (Gradient Class Activation Map** is a technique to see how the model is seeing the image for predicting. It is useful to verify weather the model is actually looking at the desired location to make the prediction.

 
