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

  
### Segmentation Code Snippet  
##### Lodading Data  
```
import nibabel as nib
image_path = "BraTS-Data/imagesTr/BRATS_001.nii.gz"
image_obj = nib.load(image_path)

# Extract data as numpy ndarray
image_data = image_obj.get_fdata()

# The image object has the following dimensions: height: 240, width:240, depth:155, channels:4
```       
##### Loading Labels  
```
label_path = "./BraTS-Data/labelsTr/BRATS_001.nii.gz"
label_obj = nib.load(label_path)

# Extract data labels as np array
label_array = label_obj.get_fdata()
```  
##### Interactive Data Visualization  
```
import itkwidgets
from ipywidgets import interact, interactive, IntSlider, ToggleButtons

# Section to visualize the data
def explore_3dimage(layer):
    plt.figure(figsize=(10, 5))
    channel = 3
    plt.imshow(image_data[:, :, layer, channel], cmap='gray');
    plt.title('Explore Layers of Brain MRI', fontsize=20)
    plt.axis('off')
    return layer

# Run the ipywidgets interact() function to explore the data
interact(explore_3dimage, layer=(0, image_data.shape[2] - 1));

# Section to Visualize Label
# Create button values
select_class = ToggleButtons(
    options=['Normal','Edema', 'Non-enhancing tumor', 'Enhancing tumor'],
    description='Select Class:',
    disabled=False,
    button_style='info', 
    
)
# Create layer slider
select_layer = IntSlider(min=0, max=154, description='Select Layer', continuous_update=False)

    
# Define a function for plotting images
def plot_image(seg_class, layer):
    print(f"Plotting {layer} Layer Label: {seg_class}")
    img_label = classes_dict[seg_class]
    mask = np.where(label_array[:,:,layer] == img_label, 255, 0)
    plt.figure(figsize=(10,5))
    plt.imshow(mask, cmap='gray')
    plt.axis('off');

# Use the interactive() tool to create the visualization
interactive(plot_image, seg_class=select_class, layer=select_layer)
```  

##### Code to extract the Patch
```
# Compute valid start indices, note the range() function excludes the upper bound
valid_start_i = [i for i in range(image_length - patch_length + 1)]
print(valid_start_i)
```  

##### Calculating the filter sizes  
filters_{i} = 32 x (2^i) x 2^j ==> i is the depth and j is the layer  
  
> Most Commercial MRI scanners gives output as DICOM format. This type of data can be processed using the pydicom Python library.   
> 4 different channels are not RGBA format.   


##### Standardizing Image  
We need to standardize the image in Channel and Z plane   
```
 # initialize to array of zeros, with same shape as the image
    standardized_image = np.zeros(image.shape)

    # iterate over channels
    for c in range(image.shape[0]):
        # iterate over the `z` dimension
        for z in range(image.shape[3]):
            # get a slice of the image 
            # at channel c and z-th dimension `z`
            image_slice = image[c,:,:,z]

            # subtract the mean
            centered = image_slice - np.mean(image_slice)

            # divide by the standard deviation
            if np.std(centered) != 0:
                centered_scaled = centered / np.std(centered)

            # update  the slice of standardized image
            # with the scaled centered and scaled image
            standardized_image[c, :, :, z] = centered_scaled

    ### END CODE HERE ###

    return standardized_image
```  

> Cross entropy loss is not suitable in segmentation due heavy class imbalance. So Dice Similarity Coefficient  
> Soft Dice Loss is used for continuous values. Dice Coeffcient is used as metrics. Because we predict the 0 or 1 value. 
