# Traffic Sign Recognition from Images
### Project Group 2
#### Nujhat Tasneem, Mohammad Adnaan, Daniel P Neelappa, Md. Shariful Islam

#### [Video Link](https://www.youtube.com/watch?v=kOIKduAOD5k)

## 1. Introduction

<object data="./Images/Supervised_Learning/1.pdf" type="application/pdf" width="700px" height="700px">
    <embed src="./Images/Supervised_Learning/1.pdf">
        <p>This browser does not support PDFs. Please download the PDF to view it: <a href="./Images/Supervised_Learning/1.pdf">Download PDF</a>.</p>
    </embed>
</object>

Traffic sign recognition system is an indispensable part of autonomous self-driving car technologyand advanced driver assistance system(ADAS). Different categories of algorithms like histogram oforiented gradients (HOG), scale-invariant feature transform (SIFT), convolutional neural networks(CNN) have been used to enhance the performance of this detection system.  Besides the role of algorithm  used  in  detection,  several  other  practical  case  scenarios  like  weather  condition,  colorfading, disorientation of signs make the task a challenging one.  In our project we have used a standard dataset provided by GTSRB (German traffic sign recognition benchmark)[1] which contains more than 50000 images organized in 43 classes for training purpose and few thousands ofrandomized images to test the performance of detection algorithm.  We have used convoultional neural network (CNN)[2] for supervised learning and for unsupervised learning Kmeans based clustering, gaussian mixture model (GMM) with principal component analysis (PCA) has been used.

## 2. Data Description
The images we use for image classification have already been reduced to 32 by 32 pixels with the training set preconfigured to 34799 examples and 12630 photos dedicated to the testing segment. A small amount of 4410 images are used as validation for each epoch of training that we perform. The entire datset has 43 unqiue classes of image labels. Here are a few of the representations of images from the dataset in their 32px by 32px representations:

<p align="center">
<img src="./Images/Supervised_Learning/train_children_crossing.png" /> <img src="./Images/Supervised_Learning/test_roundabout_mandatory.png" /> <img src="./Images/Supervised_Learning/valid_wild_animal_crossing.png" /> 
</p>

<p align="center">Fig. 1 Image Example </p>

When reviewing other work and in our class lectures, we saw the use of histograms to visualize our data. Below are three histograms that show the distribution of the given training, validation, and test datasets.

As shown in the figures 24, the dataset provided for us have a relatively similar distribution across training, testing, and validation. Our models will experience a similar range of images during training and validation.

<p align="center">
<img src="./Images/Supervised_Learning/hist_train.png" /> <img src="./Images/Supervised_Learning/hist_test.png" /> <img src="./Images/Supervised_Learning/hist_valid.png"/> 
</p>

<p align="center">Fig. 2 Distribution of training, test and validation datasets </p>

## 3. Data Processing

## 4.Supervised Learning

For our project, we used tensorflow to implement deep learning models to perform image classification in Google Colab. Convolutional Neural Networks (CNN's) are a popular method to perform image classification and can achieve high accuracy for supervised learning. We implemented both a vanilla neural network, single layer CNN, and double layer CNN for this project. Lastly, we wanted to see how susceptible our model was for changes in the images. For this reason, we added noise to our testing set to examine the models susscesability to changes in the image quality.

### 4.1 Vanilla Fully Connected Neural Network

Below is an implementation of a fully connected neural network. The image which is 32px by 32px is flattened into an array of 1024 values. The network architecture consists of two hidden layers with 512 and 256 nodes each with a relu activation. Since there are 43 class labels and we begin with 1024 values in an image, we need to reduce the dimensionality from 1024 to 43. We decided to halve the number of layer nodes as an arbitrary way to reduce dimensionality. If we had additional time, we could expand the hidden layers of the network to a large number and slowly reduce the dimensionality as long as the accuracy isn't significantly affected. With our current implementation, we were able to achieve 93% in-sample accuracy and a 78% accuracy for out of sample testing.

<p align="center">
<img src="./Images/Supervised_Learning/vanilla_training_accuracy_loss.png" /> 
</p>

<p align="center">Fig. 3 Accuracy and loss plots </p>

We also experimented with a single fully connected hidden layer with 512 nodes and a relu activation. In-sample accuracy was 95%, but out of sample accuracy was lower at 75%. With the small accuracy gain by doubling the hidden layers of the model, we decided to move on to using convolutional neural networks.

<p align="center">
<img src="./Images/Supervised_Learning/vanilla_training_accuracy_loss_hidden_layer.png" /> 
</p>

<p align="center">Fig. 4 Accuracy and loss plots of hidden layer</p>

### 4.2 Convolutional Neural Networks (CNN's)

CNN's differ from fully connected neural networks because we do not flatten the image into an array like in a fully connected neural network. CNN's have kernels which are filters that slide over the image capturing features for distinguishing labels. The last layer of a CNN network is a fully connected layer for selecting a given label.

Below are two CNN models. The first uses a double layer CNN network with relu activations. The first CNN has a stride of 1 and a kernel size of 6 by 6. We used a stride of one as we want to have a maximum of overlap with our small images as we slide the kernel. The filter size was slowly incremented from 2 to 7 by 7. A filter size of 6 by 6 was experimentally shown to provide the best accuracy given our other hyperparameters. The 2nd layer was set to a kernel size of 5 by 5. The CNN layer is then flattened with one hidden layer of 200 nodes before the 43 node layer for classification with a softmax activation. We always use a softmax activation as this limits the output from 0 to 1 which can be used to represent probabilities.

Both architectures performed very well at generalizing the dataset. The single CNN model was able to obtain a 87% accuracy during training and a 86% accuracy for the test set. The double layer CNN was able to achieve 92% in training and 90% in the testing set. Both of these were significantly better than the fully connected neural network and were able to achieve high accuracy with a limited number of layers.

<p align="center">
<img src="./Images/Supervised_Learning/single_layer_cnn_training_accuracy_loss.png" /> <img src="./Images/Supervised_Learning/double_layer_cnn_training_accuracy_loss.png" />
</p>

<p align="center">Fig. 5 Accuracy and loss plots of single and double layer CNN </p>

### 4.3 Noise Addition

Below shows a series of photos with a percentage of noise added to each. 100% noise is an value added to the normalized grayscale image from 0 to 1. A noise percentage is mupltiplied to the random value to scale the max value down from the original 1. We used our highest accuracy model which was the double layer CNN architecture.

The photos below show the accuracy and the visual depiction of several levels of noise percentage. With 15% noise, we can see a clear speed limit sign and the model has a 88% accuracy. At 75% noise, the model can obtain an almost 70% accuracy while the sign is hardly visible to the human eye.

<p align="center">
<img src="./Images/Supervised_Learning/noise_15.png" /> <img src="./Images/Supervised_Learning/noise_30.png" /> <img src="./Images/Supervised_Learning/noise_60.png" /> <img src="./Images/Supervised_Learning/noise_75.png" />
</p>

<p align="center">Fig. 6 Photo with noise </p>

### 4.4 Graphical User Interface (GUI)
Finally, we have made a GUI to facilitate image classification testing. In this GUI we can choose a particular image from test set and in the backend the trained supervised model will run and show the class of image that we have selected.

<p align="center">
<img src="./Images/Supervised_Learning/gui1.JPG" width="400" /> <img src="./Images/Supervised_Learning/gui2.JPG"  width="400" /> 
</p>

<p align="center">Fig. 7 GUI for testing </p>

## 5. Unsupervised Learning
### 5.1 Methods 
### 5.2 Results


## 6. Conclusion

## 7. References

[1] J. Stallkamp, M. Schlipsing, J. Salmen, and C. Igel. The German Traffic Sign Recognition Benchmark: A multi-class classification competition. In Proceedings of the IEEE International Joint Conference on Neural Networks, pages 1453–1460. 2011 \
[2] O'Shea, Keiron, and Ryan Nash. "An introduction to convolutional neural networks." arXiv preprint arXiv:1511.08458 (2015).











