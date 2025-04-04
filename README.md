# 🧠🧬 Brain Tumor Deep Learning Detection (CNN, VGG, ResNet, EfficientNet, ConvNeXt) and Segmentation (Segment Anything Model (SAM), UNet-like Architecture, K-Means)

![Harvard_University_logo svg](https://github.com/user-attachments/assets/0ea18127-d8c2-46ec-9f3e-10f2dc01d4d7)

![Harvard-Extension-School](https://github.com/user-attachments/assets/7de8c00d-6d74-456f-9b18-abb3174e83d5)

## **Master of Liberal Arts, Data Science**

## CSCI E-25 Computer Vision in Python

## Timeline: January 6th - May 16th, 2025 (In Progress)

## Professor: Stephen Elston

## Author: Dai-Phuong Ngo (Liam)

## First Words

In my **Brain Tumor Detection** project, I chose this dataset from Kaggle with 115 brain imges with tumor and 98 without (clearly showing imbalance): 

https://www.kaggle.com/datasets/navoneel/brain-mri-images-for-brain-tumor-detection

First, I will start with **Data Exploration** and **Image Preprocessing**:

For **Data Exploration**, I will try out the CDF and histogram distributions on 3 standard colors or predefined colors (3 to 5) to examine the color distribution by objects in typical MRI images: black background, skull layer, brain tissues, fluids, brain cord, tumor (if any) based on different brightness, contrast, sharpness, quality, angle of MRI scanning slice, unnecessary objects (eyes, neck, etc).

For **Image Preprocessing**, this phase serves 2 purposes of my 2 end goals:

1/ Prepare images for binary classification:

I will apply different techniques for image cropping (background might be too wide and unnecessary), skull layer removal (fully or partially or none removed by **Morphological Snakes** and/or **Morphological Filtering** for largest contour detection and removal), depending on image quality and close-to-global techniques), contrast enhancement, sharpness adjustment, noise surpression in predefined order. 

https://scikit-image.org/docs/0.24.x/auto_examples/segmentation/plot_morphsnakes.html

https://scikit-image.org/docs/stable/auto_examples/applications/plot_morphology.html

Then I will try out further Watershed Segmentation to highlight different regions in the brain images (with/without tumor) in colors for data enrichment before moving to data augmentation and class rebalancing techniques to diversify and balance data.
https://scikit-image.org/docs/0.24.x/auto_examples/segmentation/plot_watershed.html

2/ Prepare images with segmented tumor:

For those images with tumor, I will apply this first half part of this technique **Segment Anything Model (SAM) with Transformers** to generate color-highlighted tumor regions and build a separate annotated dataset for fine-tuning segmentation models.
https://keras.io/examples/vision/sam/

My project has 2 predictive end goals: 

1/ **Binary Classification** - Predict whether a brain MRI image shows a tumor:

I will start with a custom **CNN** and then apply **Transfer Learning** with models such as **VGG16, ResNet50, EfficientNetV2, and ConvNeXt** (more preferrable) variants to compare performance across metrics beyond accuracy.
https://keras.io/api/applications/

2/ **Tumor Segmentation** - Highlight the tumor region in MRI images where present: 

With the #2 annotated data thanks to SAM as mentioned above, I will fine-tune the pretrained model, such as **SAM** and/or **Image segmentation with a U-Net-like architecture**.

https://keras.io/examples/vision/oxford_pets_image_segmentation/

https://keras.io/examples/vision/deeplabv3_plus/

https://keras.io/examples/vision/basnet_segmentation/

https://keras.io/examples/vision/fully_convolutional_network/

This dataset is manageable in size, and I’ll rely on Keras, scikit-image and pretrained models to optimize both performance and training time.

## Computer Vision Application for Image Preprocessing

For my project, I plan to work on brain tumor detection in grayscale MRI images using deep learning techniques such as segmentation, augmentation, edge sharpening and if possible, masking and/or segmentation. MRI images are grayscale and vary as they depend on scanners, capturing different slices of the brain. These challenges arise when working with MRI data that could lower model performance. An issue is a lack of high-quality MRI images, especially annotated ones, causing an imbalance in the dataset. In addition, there are variations in brain size, tumor size and the angle of MRI scan which also lower model accuracy. To improve image quality and, hopefully, consistency, some preprocessing techniques might help as I try to increase model accuracy. The first ones can be normalization and standardization as pixel intensity values can be adjusted to a common scale. Enhancing contrast by histogram equalization can highlight tumor areas more effectively. Then, applying filters like Gaussian can denoise and reduce artifacts. As the image data is not abundant for training, data augmentation like rotation, flipping or even shifts in intensity can diversify the data and enhance the generalization of models. Edge detection and sharpening can also improve tumor boundaries. Before that, removing unnecessary areas of the images can help crop the image by covering the skull only.

## Major Concern - Bias

Bias in the medical imaging powered by machine learning, particularly brain tumor detection is a major concern. One common issue is because of the quality of the images, which are used for training purposes, that can affect the precision and reliability of classification, segmentation and even object detection models.

With that being mentioned, I find the key problem that is the most challenging for image processing phase is image quality variation. The scanner's images can experience low resolution (the most obvious behavior), noise, scanner artifacts (they can show a character like A, B, C at the image corner) and contrast inconsistencies for the whole brain and skull artifacts. As the images vary in different aspects, which I consider them as exceptional cases from standard and common quality brain images: brightness, resolution and angles (from top to bottom, or from back to front, with or without eyes and neck, which adds up more artifacts than usual), that accumulates with more challenges and struggles when dealing with feature extraction, causing inconsistent predictions. 

Another major issue is class imbalance in the given images. In my dataset, there are 98 images of brains without tumors versus 155 images of brains with tumors. This imbalance causes the models to be trained with and observe more examples of tumors, which sounds attractive, but unfortunately, might be more prone to and biased toward detecting tumors, even when there is no tumor exists, which is false positive. On the other hand, underrepresented images without tumors might cause tumors undetected, which is false negative, causing more serious risk.

To rectify the problems, I've been thinking about some techniques to apply for and test the ending results when put into the models.

Data augmentation can help to apply certain tasks like rotation, flipping, contrast adjustment and noise surpression to have more diverse data for model training.

Class rebalancing might offer an effective fight against bias with oversampling or undersampling. I have to think which one makes more sense for this project.

Transfer learning can reduce much of model development by using pretrained models that were trained on large-scale medical datasets to improve performance on my limited data.

Not relying on a single metric, accuracy, is a must as there are other metrics like precision, recall and their curves, AUC ROC, F1 to consider about.

I've been thinking about finding more images to merge into my dataset or find a brand new dataset but their load are very heavy for limited cloud storage and model consumption within limited project timeline for this course if not running on Kaggle notebook but on Google Colab Pro (plus) and better version of Google Drive.

## Major Preparation - Feature Extraction

Brain tumor detection from MRI scan images is my project for this course to explore and apply feature extraction techniques to identify tumors more efficiently and accurately. These images include different sizes and shapes of tumors and normal brain scans for a balanced dataset. Some features for tumor detection that I have been thinking about, such as:

Tumors have different textures than normal brain tissue and asymmetry location. Extracting features based on entropy can differentiate these areas. 

Tumors have unusual shapes, and boundaries. Using edge detection techniques like Sobel filters and Canny edge detection can help to capture them.

Tumors can be displayed in bright or dark areas. Using intensity histograms and threshold techniques can highlight abnormal areas.

Some key properties that I will use in these applications are as follows:

Asymmetry and unusual boundaries: tumor objects tend to exist on one side, causing an asymmetry appearance.

Contrast differences: tumors can be displayed with contrast variations which is not observed in health brain image.

Different scale analysis: Tumors can exhibit in varying sizes so features should be extracted at different scales when analyzing multi-scale features. 

## Enhancement Concepts

1. Multiscale Image Enhancement (Laplacian Pyramid approach)

Decompose image into low- and high-frequency components (Gaussian + detail).

Amplify detail (high-frequency) layers.

Reconstruct the enhanced image via synthesis.

2. MUSICA (Multiscale Image Contrast Amplification)

Essentially: Apply the above with fine-tuned contrast amplification at each level.

![Cropped images](https://github.com/user-attachments/assets/ef1f6f52-51d8-434c-8200-fb23f6c381f2)

![download (17)](https://github.com/user-attachments/assets/63f1bc9f-5c3d-419c-98fc-0794edb9d5be)

![download (12)](https://github.com/user-attachments/assets/e389182f-c5e0-40e9-8431-8df5cdfd0507)

![download (11)](https://github.com/user-attachments/assets/0c65ccb8-08b0-4182-bb4f-6a1b9ed4947b)

![download (13)](https://github.com/user-attachments/assets/87a05431-6101-4578-bd02-3b967bf1b72d)


## K Means for Tumor Mask in colors

### Select a K based on basic domain-based approach (tumor, skull, brain tissue, background)

![download (14)](https://github.com/user-attachments/assets/5557cd08-41ed-4029-806b-9887c3ee306d)

![download (15)](https://github.com/user-attachments/assets/405ef50d-f565-4d3d-ad18-655efc59b36d)

![download (16)](https://github.com/user-attachments/assets/81394d7a-f58d-468c-9754-e7638bd0a569)

### Find the best K with revised clustering plots

![download (39)](https://github.com/user-attachments/assets/bd30daed-fc7c-47e6-bb03-6945921885d0)

Based on the **WCSS (Inertia)**, WCSS decreases significantly from k = 2 to k = 4, then tapers off, which is the elbow method suggesting k ≈ 4.

As per the **BCSS**, BCSS remains consistent increment with k, but the rate of increase slows after k = 4.

Regarding the **Silhouette Score**, the highest score at k = 2, but that is often too coarse. There is a noteworthy drop after k = 4, suggesting diminishing structure.

In terms of the **Calinski-Harabasz Index**, it increases linearly, which is useful but doesn't suggest the elbow like the WCSS or the Silhouette.

From this observation of all signals, the best should be **4**. which offers a balance of natural elbow in WCSS, a decent SIlhouette score and a fair trade-off of detail and interpretability for medical segmentation, such as: background, skull layer, brain tissue, tumor.

### Revise the MUSICA on non-flattened images before K Means and apply strict threshold for black pixels (enhance blackness for black/dark background)

![download (31)](https://github.com/user-attachments/assets/40dc64e1-68e2-44bf-9995-d2defb70f2cd)

![download (32)](https://github.com/user-attachments/assets/a8d28fab-e781-4dfb-ade4-2a26476c7bd1)

![download (33)](https://github.com/user-attachments/assets/9fdfb972-59fb-4795-b204-458edaac358a)

The surrounding background with some blue regions are now surpressed to become black before applying colorized K Means with suggested 4 colors.

### Find Better K based on new clustering plots with extended range up to 20

![download (45)](https://github.com/user-attachments/assets/080dbb69-fca8-4b53-bae9-c4dd8d66251a)

![download (46)](https://github.com/user-attachments/assets/9e14a174-b334-422f-bc58-f6f23689948b)

![download (47)](https://github.com/user-attachments/assets/a2cccff4-7148-4527-81cb-327fd3ccd2b1)

![download (48)](https://github.com/user-attachments/assets/332ccbbd-5b69-4f8b-b012-46610789fbda)

![download (49)](https://github.com/user-attachments/assets/ba20c226-ec46-4c68-b59e-75605d6f7263)

![download (50)](https://github.com/user-attachments/assets/9072aeea-9efe-4bd5-aaf2-3486cae01ddc)

![download (51)](https://github.com/user-attachments/assets/6c907a80-cae2-4cf6-8c95-96861bc5bfb3)

Based on the plots of 7 images, the best K is 9. This is a consistent balancing among high Silhouette Score, high Calinski-Harabasz Index, reasonable BCSS gain and WCSS drop-off.

### Colorized Clusters with K = 9

![download (52)](https://github.com/user-attachments/assets/bfa979b4-88aa-4a10-a106-146a87178900)

![download (53)](https://github.com/user-attachments/assets/c4d22175-0d2c-4587-9243-18a19c9465fe)

![download (54)](https://github.com/user-attachments/assets/7964aff1-81c1-436f-9983-6376b837b88b)


# Deep Learning (to be continued)
---

## Detection (to be continued)
---

## Segmentation (to be continued)

--- 

# References

### **Medical Imaging, CAD, and AI**

> **Pianykh, O.S.** (2024). *Computer-Aided Diagnosis: From Feature Detection to AI in Radiology*. Lecture slides, CSCI E-87: Big Data and Machine Learning in Healthcare Applications, **Harvard University Medical School**.  
> ➤ Key topics: CAD pipeline (feature extraction → classifier → validation), edge & line detection for fractures, convolution in LoG filters, multiscale resolution, PyRadiomics, CNNs for medical imaging.

Use this reference to support:
- Why ML is used in radiological images
- KMeans + morphological segmentation logic
- Convolutional methods (LoG, Gaussian, etc.)
- Discussion of PyRadiomics as an alternative or extension

> “Pathologies manifest as deviations from normal patterns; by extracting numeric features like shape, density, and texture, we can quantify abnormality — the essence of CAD.” — *Pianykh (2024)*

---

### **Multiscale Image Enhancement & Multiscale Image Contrast Amplification (MUSICA)**

> **Pianykh, O.S.** (2024). *Image Enhancement Techniques in Medical Imaging: From Denoising to CNNs*. Lecture slides, CSCI E-87: Big Data and Machine Learning in Healthcare Applications, **Harvard University Medical School**.  
> ➤ Key topics: noise vs edges, bilateral filtering, Gaussian pyramids, Laplacian pyramids, multiscale decomposition and synthesis, MUSICA-style amplification.

Use this reference to support:
- Why using multiscale decomposition
- The basis for MUSICA-style image contrast enhancement
- Limitations of averaging vs adaptive multiscale amplification

> “By decomposing an image into low- and high-frequency components, and rebalancing with detail amplification, MUSICA enhances the contrast and diagnostic utility of the image.” — *Pianykh (2024)*


---

### **Clustering & Dimensionality Reduction**

> Elston, S. F. (2024). *Introduction to Clustering Models – Part 1* [Lecture slides]. CSCI E-108: Data Mining, Discovery, and Exploration, **Harvard University Extension School**.  
   - Topics: Unsupervised learning, K-Means clustering, distance and similarity metrics, cluster properties (compactness & separation), evaluation metrics (WCSS, BCSS, Silhouette score, Calinski-Harabasz index).

> Elston, S. F. (2024). *Introduction to Clustering Models – Part 2* [Lecture slides]. CSCI E-108: Data Mining, Discovery, and Exploration, **Harvard University Extension School**.  
   - Topics: Curse of dimensionality in clustering, hierarchical clustering, DBSCAN, OPTICS, affinity propagation, spectral clustering, and graph-based methods.

> Elston, S. F. (2024). *Dimensionality Reduction – Part 1* [Lecture slides]. CSCI E-108: Data Mining, Discovery, and Exploration, Harvard University Extension School.  
   - Topics: PCA, feature projection, manifold learning (e.g., UMAP), and visualization strategies for high-dimensional data clustering.

---

### 🧪 **Image Segmentation & Morphological Techniques**
1. **Morphological Snakes (Active Contour Models)**  
   *scikit-image team.*  
   _Morphological Snakes: Active contours without edges._  
   https://scikit-image.org/docs/0.24.x/auto_examples/segmentation/plot_morphsnakes.html

2. **Watershed Segmentation**  
   *scikit-image team.*  
   _Image segmentation using the watershed algorithm._  
   https://scikit-image.org/docs/0.24.x/auto_examples/segmentation/plot_watershed.html

3. **Morphological Operations in Image Processing**  
   *scikit-image team.*  
   _Denoising, sharpening and edge detection using kernel convolution._  
   https://scikit-image.org/docs/stable/auto_examples/applications/plot_morphology.html

---

### 🤖 **Deep Learning with Keras**

4. **Segment Anything with SAM & Keras**  
   *Keras Team, 2024.*  
   _Segment Anything: Integrate SAM models for high-quality image segmentation._  
   https://keras.io/examples/vision/sam/

5. **Keras Applications (Pretrained CNNs)**  
   *Keras Documentation.*  
   _Keras Applications - Pretrained models like ResNet, VGG, Inception._  
   https://keras.io/api/applications/

6. **U-Net for Oxford Pets Dataset (Semantic Segmentation)**  
   *Keras Team, 2023.*  
   _Image segmentation of Oxford Pets using U-Net._  
   https://keras.io/examples/vision/oxford_pets_image_segmentation/

---

### 🧠🧬 **Medical Image Dataset**

7. **Brain MRI Dataset for Tumor Detection (Kaggle)**  
   *Navoneel Chakrabarty, Kaggle.*  
   _Brain MRI Images for Brain Tumor Detection (Yes/No classification)._  
   https://www.kaggle.com/datasets/navoneel/brain-mri-images-for-brain-tumor-detection


