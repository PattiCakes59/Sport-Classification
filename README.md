![Hogan%20copy.jpg](attachment:Hogan%20copy.jpg)

## Initial Data Exploration/Preparation
The data comes from a repository on GitHub that can be found <a href=https://github.com/jurjsorinliviu/Sports-Type-Classifier>here.</a>
- This data is composed of 23 different classes of sport images. Seven of these 23 will be chosen to classify. The main reason behind this decision was that some of the files did not work. The format they were in was not compatible with this project.
- I will use the library pathlib in order to load up the data.
- There are two types of paths in pathlib:
>1. Relative path: The path relative to the folder we are currently working in.
 2. Absolute path: The path that is relative to the operating system (this is the one I am working with).
- There is a minor limit to the data. The images are of different aspects of the sport. For example, just a picture of a ball. Not necessarily the people playing with it.
- Importing relevant libraries to operate on the data.

## Download the Data
- How many images are there?
- Exploring a few images to see that we are loading up the right things.
- It doesn't do us any good to use data if it isn't the right data.
- Always good to measure twice and cut once.

## Exploring Some of the Images
- Loading up a couple of selections.
- What do the images look like?

![download.png](attachment:download.png)

## Parameters
- Preparing some parameters for the loader.
- The batch refers to the number of training examples utilized in one iteration.
- The dimension of the images we are going to define are 180x180.
- A lower dimension size with greater batch size is one of the options to try.

## Creating a Dataset
- Let's load these images off disk using the helpful tf.keras.utils.image_dataset_from_directory utility.
- This will take you from a directory of images on disk to a tf.data.Dataset in just a couple lines of code.
- It's good practice to use a validation split when developing your model. I will use 80% of the images for training, and 20% for validation.

## Visualize the Data
- This is a simple preview of some of the train_ds images.
- There are nine images to preview.
- It is good to get an idea if they are being filed under the right label.
- It is good practice to see your data.

## Configure the Dataset for Performance
- I will use buffered prefetching so I can yield data from the disk without having an input or output blocking. 
- These are two important methods you should use when loading data:
 1. Dataset.cache - keeps the images in memory after they're loaded off disk during the first epoch. This will ensure the dataset does not become a bottleneck while training your model. If your dataset is too large to fit into memory, you can also use this method to create a performant on-disk cache.
 2. Dataset.prefetch - overlaps data preprocessing and model execution while training.
(Interested readers can learn more about both methods, as well as how to cache data to disk in the Prefetching section of the Better performance with the tf.data API guide.)

## Standardize the data
- The RGB channel values are in the [0, 255] range. This is not ideal for a neural network.
- In general you should seek to make your input values small.
- I will standardize the values to be in the [0, 1] range by using tf.keras.layers.Rescaling.
- This can be done within the cnn architecture but I chose to do it outside of it so as to provide a good example.
- It is written up in the cnn architecture just hashed out.

## CNN
- CNN architecture is based on layers of convolution.
- The convolution layers receive input and transform the data from the image and pass it as input to the next layer.
- The transformation is known as the operation of convolution.
- It is necessary to define the number of filters for each convolution layer.
- These filters detect patterns such as edges, shapes, curves, objects, textures, or even colors.
- The more sophisticated patterns or objects it detects are more deeply layered.
- The Sequential model consists of three convolution blocks (tf.keras.layers.Conv2D) with a max pooling layer (tf.keras.layers.MaxPooling2D) in each of them. 
- There's a fully-connected layer (tf.keras.layers.Dense) with 128 units on top of it that is activated by a ReLU activation function ('relu'). This model has not been tuned for high accuracy. It is just a baseline model.

## Building a Baseline Model
- Just exploring the data.
- Seeing what an initial model spits out.
- In order to view the training and validation accuracy for each training epoch I will pass the metrics argument through .compile.
- Tried out a few initializers just to see what would happen.

## Visualizing the Model
- Link for code and model found <a href=https://towardsdatascience.com/medical-x-ray-%EF%B8%8F-image-classification-using-convolutional-neural-network-9a6d33b1c2a>in this blog</a>.


```python
# !pip install pydot
# import graphviz
# from tensorflow.keras.utils import plot_model
# plot_model(model,show_shapes=True, show_layer_names=True, rankdir='TB', expand_nested=True)
```

![Layers.png](attachment:Layers.png)

## Visualize Training Results
- Let's create some plots of loss and accuracy for the training and validation sets!
![Visuals.png](attachment:Visuals.png)

- The plots above show that training accuracy and validation accuracy are off by large margins, and the model has achieved only around 60% accuracy on the validation set.

- Let's inspect what went wrong and try to increase the overall performance of the model.

## Overfitting
- In the plots above, the training accuracy is increasing linearly over time, whereas validation accuracy stalls around 60% in the training process. Also, the difference in accuracy between training and validation accuracy is noticeable.

- When there are a small number of training examples, the model sometimes learns from noises or unwanted details from training examples. This can negatively impact the performance of the model on new examples. This phenomenon is known as overfitting. It means that the model will have a difficult time generalizing on a new dataset.

- There are multiple ways to fight overfitting in the training process. Going forward I will use data augmentation and add Dropout on the model.

## Data augmentation
- Overfitting generally occurs when there are a small number of training examples. Data augmentation takes the approach of generating additional training data from your existing examples by augmenting them using random transformations that yield believable-looking images. This helps expose the model to more aspects of the data and generalize better.

- I will implement data augmentation using the following Keras preprocessing layers: 
 1. tf.keras.layers.RandomFlip 
 2. tf.keras.layers.RandomRotation
 3. tf.keras.layers.RandomZoom. 
 
These can be included inside your model like other layers, and run on the GPU.

## Dropout
- Another technique to reduce overfitting is to introduce dropout regularization to the network.

- When you apply dropout to a layer, it randomly drops out (by setting the activation to zero) a number of output units from the layer during the training process. Dropout takes a fractional number as its input value, in the form such as 0.1, 0.2, 0.4, etc. This means dropping out 10%, 20% or 40% of the output units randomly from the applied layer.

- Let's create a new neural network with tf.keras.layers.Dropout before training it using the augmented images.

## Tuning the Model
- Creating some hyperparameters.
 1. Conv2D parameter is the numbers of filters that convolutional layers will learn from. It is an integer value and also determines the number of output filters in the convolution.

## Fitting the Model
- EarlyStopping is called to stop the epochs based on some metric (monitor) and conditions (mode, patience).
- It helps to avoid overfitting the model.
- We are telling the model to stop based on val_loss metric, we need it to be a minimum.
- (patience) says that after a minimum val_loss is achieved during the next iterations if the val_loss increases in any of the 3 iterations then the the training will stop at that epoch.

## Visualize Final Model Results
- The plots show that training accuracy and validation accuracy are off by large margins...again.
- The model only achieved only around 65% accuracy on the validation set and then plummeted.
- More fine tuning for the model will be necessary to make this model better.

## Evaluating a Random Image
- I pulled an image of a baseball player to see if it would be predicted correctly.
- The model did a phenomenal job at predicting the image.
 - The model had 100% confidence in predicting the right sport for the image. This is excellent.

![randomsample.jpeg](attachment:randomsample.jpeg)
