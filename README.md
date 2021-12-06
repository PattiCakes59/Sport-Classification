![Baseball.png](attachment:Baseball.png)

# Business Problem

Trying to retrieve information automatically is always better achieved when the data has been previously classified into categories. However, given the current amount of of available information, and more constantly being produced, it is very hard to manually classify all of them. That is why it is essential to establish a methodology that would be able to not only retrieve the data, but also to present the data in the correct class. One way to accomplish this later goal is to use a machine learning algorithm for classification. For this project in particular, we tackle the automatic classification of images related to different sports. Given the increasing success of of deep learning in classification tasks it only makes sense to use it for this classification.

This <a rhef=https://www.sciencedirect.com/science/article/pii/S1877050920307560> article</a> goes more in depth about why sports image classification is important. In case the need for further reading.

# Project Goals

There are a number of project goals that need to be accomplished in order to consider this a successful endeavor.
They are as followed:
1. First and most importantly, the goal of any classification project is to use the raw visual data contained within an image to assign a certain conceptual label, or “class” to that image.
2. This comes down to using a computer vision algorithm that will return a single choice out of a number of possible choices.
3. I would like to run a variety of notebooks with different types of models in order to get to the most appropriate structure that provides the best results.
4. Finally, I would like my model to return an accuracy score greater than or equal to 85%.


The reason I would like the accuracy score at this point or higher is because anything lower means the model is not doing well. This in turn means I am not classifying the images at a high enough clip.

## Where to Find the Data
The data is a big part of any classification problem. I look around and found a GitHub repository that had all of the things I needed. There are 23 classes of images. This is plenty of images to create a good dataset.
- The data repository on GitHub can be found <a rhef=https://github.com/jurjsorinliviu/Sports-Type-Classifier>here.</a>
- To navigate to the the actual data used from the repository click on the file titled "data". 
 - This folder will open up and give you a list of 23 folders that include images of the folders label.
 - Included in the data is also a txt file that includes the url location of each image.
- You can fork and clone the whole repository and get the data that way. 
- Another way to get the data is through this zip file that I created. Either click on the link or copy and paste the link and the data will automatically be downloaded in the form of a zip file.
 - https://minhaskamal.github.io/DownGit/#/home?url=https://github.com/jurjsorinliviu/Sports-Type-Classifier/tree/master/data
 



## Loading up the Data

Loading up images can sometimes be an issue. So, to make this easier on whomever is reading this and interested in trying the project here is a list of procedures:
- The first thing to know is that you will need pathlib to import Path. 
 - You can find out more about this process <a href=https://docs.python.org/3/library/pathlib.html#basic-use>here.</a>
- The next thing that needs to be done is to create a dataset from the images. That is done with the following process:
 - First let's load the images using the helpful tf.keras.utils.image_dataset_from_directory utility. 
  - This will take you from a directory of images on disk to a tf.data.Dataset in just a couple lines of code.
 - After creating a dataset it is good practice to use a validation split when developing your model. I will use 80% of the images for training, and 20% for validation.
 - To learn more about the process and the libraries associated you can find the info <a rhef=https://www.tensorflow.org/api_docs/python/tf/keras/utils/image_dataset_from_directory> here.</a>

## Modeling

In terms of modeling the first thing that needs to be done is to prepare the data. In brief, my first step was to standardize the data. This was done by making the values fit in the range of [0, 1] by using tf.keras.layers.Rescaling (originally the data was between [0, 255]). This was done outside of the actual architecture of the model but can be done within it. The rest of the steps are incorporated in the model. For image classification the best model to use is a convolutional neural network. The reason for this is that a convolutional neural network is composed of multiple building block layers that receive input and transform the data from the image and pass it as input to the next layer. It is also designed to automatically and adaptively learn spatial hierarchies of features through a backpropagation algorithm.


```python

```
