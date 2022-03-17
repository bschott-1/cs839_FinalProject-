# Out-of-Distribution (OOD) Detection for liver Tumors on Computed Tomography Images (CT)

The purpose of this work is to implement out-of-distribution (OOD) detection methods for a convolutional neural network (CNN) trained to segment liver tumors on Computed Tomography (CT) images. When training a CNN for semantic segmentation, the model learns the feature representations that transform each pixel (or voxel) in the input image into any one of N classes. The underlying image features in the training dataset make up the in-training data distribution that a network acts on. This distribution should be broad enough to encompass all possible test images. If a test image is dissimilar to the distribution of training samples, then it is considered OOD. A deep CNN will likely segment such an OOD test image incorrectly but with high confidence because modern neural networks are known to be miscalibrated [[1]](#1). Therefore, an additional measure indicative of distance from the training distribution is necessary to generate trustworthy results at test time. 

 

In our approach, we assert an OOD detection algorithm requires two main components: 1) feature dimensionality reduction and 2) some margin to determine distance from the training distribution. Previous work has been done in [[2]](#2), which reduces features from an intermediate layer in the network via singular value decomposition. This feature reduction is applied at test time, where the test image features are compared to the reduced features present in the entire training set by the Euclidean distance. An alternative approach to OOD detection is to train an autoencoder to encode an image into a finite feature representation (bottle-neck layer) which is then decoded to reconstruct the input image [[3,4]](#3). In theory, a trained autoencoder will reconstruct images within its training distribution well and poorly reconstruct images outside the training distribution. Reconstruction error can be quantified using image similarity metrics such as mean squared error, which can subsequently serve as a margin for OOD distance. OOD autoencoder implementations in the literature are limited to classification tasks. We will investigate the utility of using an autoencoder for OOD detection in medial image segmentation tasks.  

 

For this work, we will train two separate networks. We will utilize the nnUNet repository for the segmentation CNN [[5]](#5). This library optimizes the network hyperparameters through a series of heuristics which considers the input “data fingerprint” and the available compute infrastructure. For the autoencoder CNN, we will follow identical architecture as in the segmentation CNN, except skip connections will be removed and the loss function will be changed to mean squared error (as opposed to the sum of Dice and cross-entropy loss). We will rely on publicly available medical imaging and manual contour data available through [[6]](#6). Available datasets span different imaging modalities (e.g., CT and MRI) and disease types (e.g., liver tumors and brain tumors). We will train the networks using the liver tumor data which will be split into train, validation, and test sets. As a “proof-of-concept”, we will test the OOD method by introducing images that obviously differ from the training set (e.g., cardiac MRI images) and expect these images to be very OOD. We will also assess the OOD distances for the CT liver tumor testing images and hypothesize that images with larger OOD distances will have poorer segmentation performance which will be tested by linearly correlating these two variables. If a strong correlation is present, then we will conclude that OOD distance can serve as a surrogate for segmentation performance, thus offering an added level of trust in the network’s decision making. 

 
## References

<a id="1">[1]</a> C. Guo et al. “On Calibration of Modern Neural Networks”. arXiv. 2017. 

<a id="2">[2]</a>  D. Karimi and A. Gholipour. “Improving Calibration and Out-of-Distribution Detection in Medical Image Segmentation with Convolutional Neural Networks”. 2020. 

<a id="3">[3]</a>  M. Sabokrou et al. “Adversarially Learned One-Class Classifier for Novelty Detection”. arXiv. 2018. 

<a id="4">[4]</a>  N. Shvetsova et al. “Anomaly Detection in medical Imaging With Deep Perceptual Autoencoders”. IEEE. 9. 2021.  

<a id="5">[5]</a>  F. Isensee et al. “Automated Design of Deep Learning Methods for Biomedical Image Segmentation”. arXiv. 2020.  

<a id="6">[6]</a> M. Antonelli et al. “The Medical Segmentation Decathlon”. arXiv. 2021. 