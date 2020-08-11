## Image Segmentation on (3D) MRI Images
Image segmentation plays a vital role in numerous medical imaging applications such as Quantification of the size of tissues, the localization of diseases and treatment planning.
  
#### MRI Data:  
MRI data are normally volumetric or 3D. This can be represented as 2D image with many channels.     

A data sample can be a combination of multiple sequences. These sequences are generated when multiple MRI is done.  
  
##### Image Registration:   
To combine the sequences in a single axis it is required all the sequences are aligned. To ensure this Image Registration is done to make all the sequences aligned.   
![registration](/Images/image_registration.jpg)  
#### Voxels:  
The points in 2D space is called pixel and 3D space is called voxels.   

  
### Segmentation Approaches  
Segmenting MRI images can be approached with 2D and 3D. 
  
- **2D Approach**  
In this way each slice of the image is passed to the segmentation model and the segmentation for each slice is combined together to generate the final output.   
> Drawback:  Missing the important 3D context  
  
- **3D Approach**  
In this way ideally we want to pass the whole 3D volume but it requires a lot of memory and computation which makes it impossible.   
Rather we break the 3D MRI volume into smaller 3D volume to provide context information to the model. In this way though we are missing the spatial information.   
After passing each of the sub volume we aggregate them at the end to form segmentation map.   
  
  
### UNet Architecture   
One of the most popular segmentation architecture. 
- A contracting path followed by a expanding path  
- Concatenates the upsampling representations at each with corresponding feature map at contraction pathway.  
 
> Cool thing of UNet: Can achieve relatively good results even with hundreds of examples.  


We need to replace 2D operations with its corresponding 3D counter parts. 
  
![3D UNet](/Images/3DUnet.jpg)  

### Data Augmentation  
Data augmentation in the segmentation needs to be performed both in Input and Output. We need to rotate the output at the same angle with input.   

### Loss Function  
  
**Soft Dice Loss**   
This loss function calculates the overlap between the predictions and ground truth.   
![Loss Function](/Images/dice%20loss.jpg)  

Taking 1 minus represent larger loss for smaller overlap. The fraction represent the overlap.   


### Challenges of AI in Medical to bring in practice  
- **Generalization:** Model trained with collected dataset from one region might not look similar from other region.   
For example MRI images varies with time and place as its scanner changes.  

#### External Validation   
External validation means when we test the model with the data of a new population from the similar distribution of the training data. If we are not generalizing we need to fine tune the model.   

- To analyze the performance of AI model in real world we need to test it with the Prospective or real world data.  
Often the model is trained with retrospective data which is clean and processed.   
  
  
- **Randomized control Trial**  
We want to analyze the effect of the model with different ages, sex and socioeconomic classes. Skin diseases model might vary with black and white people.         