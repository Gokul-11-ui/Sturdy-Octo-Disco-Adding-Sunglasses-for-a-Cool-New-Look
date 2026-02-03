# Sturdy-Octo-Disco-Adding-Sunglasses-for-a-Cool-New-Look

## Date - 03/2/26
## Reg no - 212224040091

Sturdy Octo Disco is a fun project that adds sunglasses to photos using image processing.

Welcome to Sturdy Octo Disco, a fun and creative project designed to overlay sunglasses on individual passport photos! This repository demonstrates how to use image processing techniques to create a playful transformation, making ordinary photos look extraordinary. Whether you're a beginner exploring computer vision or just looking for a quirky project to try, this is for you!

## Features:
- Detects the face in an image.
- Places a stylish sunglass overlay perfectly on the face.
- Works seamlessly with individual passport-size photos.
- Customizable for different sunglasses styles or photo types.

## Technologies Used:
- Python
- OpenCV for image processing
- Numpy for array manipulations

## How to Use:
1. Clone this repository.
2. Add your passport-sized photo to the `images` folder.
3. Run the script to see your "cool" transformation!

## Applications:
- Learning basic image processing techniques.
- Adding flair to your photos for fun.
- Practicing computer vision workflows.

Feel free to fork, contribute, or customize this project for your creative needs!

##PROGRAM AND OUTPUT:

```
# Import libraries
import cv2
import numpy as np
import matplotlib.pyplot as plt

# Load the Face Image
import cv2
import matplotlib.pyplot as plt
faceImage = cv2.imread('IMG-20250507-WA0015.jpg')
plt.imshow(faceImage[:,:,::-1]);plt.title("Face")


```

<img width="594" height="590" alt="image" src="https://github.com/user-attachments/assets/8d507c94-65f5-4927-8c3c-410456157ea3" />

```

faceImage.shape
```


(1014, 713, 3)

```
# Load the Sunglass image with Alpha channel
# (http://pluspng.com/sunglass-png-1104.html)
glassPNG = cv2.imread('Image-Source-PlusPNG.com (1).png',-1)
plt.imshow(glassPNG[:,:,::-1]);plt.title("glassPNG")
```
<img width="552" height="280" alt="download" src="https://github.com/user-attachments/assets/061414a7-40e7-4805-b714-e3133b636e99" />


```
# Resize the image to fit over the eye region
glassPNG = cv2.resize(glassPNG,(190,50))
print("image Dimension ={}".format(glassPNG.shape))

# Separate the Color and alpha channels
glassBGR = glassPNG[:,:,0:3]
glassMask1 = glassPNG[:,:,3]

# Display the images for clarity
plt.figure(figsize=[15,15])
plt.subplot(121);plt.imshow(glassBGR[:,:,::-1]);plt.title('Sunglass Color channels');
plt.subplot(122);plt.imshow(glassMask1,cmap='gray');plt.title('Sunglass Alpha channel');

```
<img width="1209" height="203" alt="download" src="https://github.com/user-attachments/assets/4740b53c-62a7-4d14-8a3c-24ffa5d573ca" />


```
# Make a copy 
#faceWithGlassesNaive = resized_faceImage.copy()
faceWithGlassesNaive = faceImage.copy()

# Replace the eye region with the sunglass image
faceWithGlassesNaive[135:185,110:300]=glassBGR

plt.imshow(faceWithGlassesNaive[...,::-1])
```
<img width="325" height="418" alt="download" src="https://github.com/user-attachments/assets/306b0992-0cfd-4078-8022-897b1796d146" />

```
# Make the dimensions of the mask same as the input image.
# Since Face Image is a 3-channel image, we create a 3 channel image for the mask
glassMask = cv2.merge((glassMask1,glassMask1,glassMask1))

# Make the values [0,1] since we are using arithmetic operations
glassMask = np.uint8(glassMask/255)

# Make a copy
faceWithGlassesArithmetic = faceImage.copy()

# Get the eye region from the face image
eyeROI= faceWithGlassesArithmetic[135:185,110:300]

# Use the mask to create the masked eye region
maskedEye = cv2.multiply(eyeROI,(1-  glassMask ))

# Use the mask to create the masked sunglass region
maskedGlass = cv2.multiply(glassBGR,glassMask)

# Combine the Sunglass in the Eye Region to get the augmented image
eyeRoiFinal = cv2.add(maskedEye, maskedGlass)

# Display the intermediate results
plt.figure(figsize=[20,20])
plt.subplot(131);plt.imshow(maskedEye[...,::-1]);plt.title("Masked Eye Region")
plt.subplot(132);plt.imshow(maskedGlass[...,::-1]);plt.title("Masked Sunglass Region")
plt.subplot(133);plt.imshow(eyeRoiFinal[...,::-1]);plt.title("Augmented Eye and Sunglass")
```
<img width="1597" height="140" alt="download" src="https://github.com/user-attachments/assets/773aa9d4-8e2a-4202-ae62-bcd88d987783" />


```

# Replace the eye ROI with the output from the previous section
faceWithGlassesArithmetic[135:185,110:300]=eyeRoiFinal

# Display the final result
plt.figure(figsize=[20,20]);
plt.subplot(121);plt.imshow(faceImage[:,:,::-1]); plt.title("Original Image");
plt.subplot(122);plt.imshow(faceWithGlassesArithmetic[:,:,::-1]);plt.title("With Sunglasses");

```
<img width="1616" height="1066" alt="download" src="https://github.com/user-attachments/assets/4b1833e6-2443-4a7e-a8c4-6de60f774ff9" />

