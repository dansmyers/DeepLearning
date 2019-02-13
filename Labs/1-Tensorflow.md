# Tensorflow

## Overview

This lab will let you install and experiment with neural network implementations using Keras and Tensorflow, two popular frameworks for deep learning. Along the way, we'll learn more about visualization in Python.

Technically, Tensorflow is a library that implements numerical computation methods for machine learning. It includes, among other things, support for processing on GPUs and multiserver systems. Keras a Python front-end library that provides a simpler interface to the Tensorflow routines.

## Install Tensorflow and Keras

### Miniconda

I heard you like installing things, so step 1 is to install Miniconda, a package manager that will help you install eveything else that needs to be installed.

Fetch the Miniconda package from its location with the `wget` command. Throughout this lab, I'm using `$` to refer to the command prompt:

```
$ wget https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh
```

Run the installer:

```
$ bash Miniconda3-latest-Linux-x86_64.sh
```

When the installer shows you the license, press `q` to leave it and then enter `yes` to accept it. You can install the library into the default location (probably /home/ubuntu/miniconda3). Agree to add miniconda to your system path.

When the installer finishes, close your current terminal and open a new one.

### Everything Else

Run the following commands to install all of the Python packages you'll need:

```
$ conda install numpy scipy mkl
$ conda install tensorflow
$ conda install keras
```

## Setup GUI Output

### X Server

**UPDATE: This didn't work well, so we've changed to saving our plots as PDFs. The rest of the lab has been updated.***

Codio can display graphical output from Python programs, but you have to do a few extra steps to get it configured.

Go to Tools --> Install Software and scroll down to X Server. Click the install icon to add it to your workspace.

X is a virtual desktop that can run in a tab in your Codio environment. You can use it to visualize the output of graphical programs, like the plotting code we'll use in this lab.

Once the X Server install finishes, it will prompt you to go to the Project menu and reset your box. Do that.

When your window restarts, go up to **second tab to the right of Help** at the top of your window. It will probably say either "Preview" or "Virtual Desktop". Click on the small arrow and go down to "Configure" on the menu. Verify that the JSON file that comes up looks like this:

```
{
    "preview": {
        "Virtual Desktop": "https://{{domain3000}}/"
    }
}
```

### Matplotlib

Let's get a Python plotting library.

```
$ conda install matplotlib
```

Create a test file:

```
$ mkdir Lab-1-Tensorflow
$ cd Lab-1-Tensorflow
$ touch test_plot.py
```

Put this code in `test_plot.py`:

```
# Example plot
# CMS 495, Spring 2019

import matplotlib
matplotlib.use('Agg')  # Use to save output as PDF

from matplotlib import pyplot as plt
from math import sin, pi

# Make a list that goes from 0 to 2 * pi
x = [i * .01 * 2 * pi for i in range(100)]

# sin(x)
y = [sin(t) for t in x]

# Plot with Matplotlib
# The first call has to be plt.figure() to initialize the window
plt.figure()
plt.plot(x, y)
plt.xlabel('x')
plt.ylabel('sin(x)')

# Save as PDF
plt.savefig('test_plot.pdf', bbox_inches = 'tight')
```

Run the program using

```
$ python test_plot.py
```

This will save the figure to a file named `test_plot.py`. The `bbox_inches` argument tells `matplotlib` to put the minimum amount of whitespace around the figure.


## Fashion Forward

The MNIST dataset is a set of 28x28 images of handwritten characters. It's the same dataset that was used in the video we watched.

MNIST stands for "Modified National Institute of Standards and Technology". The set contains 60000 training images and 10000 test images.

However, classifying digits is boring, so the Keras documentation includes an alternate "Fashion MNIST" dataset, containing 60000 images of clothing divided into 10 classes: ankle boots, sandles, t-shirts, etc.

The goal of this section is to write a program that uses a neural network trained by Tensorflow to classify clothing images from the Fashion MNIST dataset. The code is modified from the Tensorflow intro tutorial.

### Start

Make a new file named `fashion.py`. Add this code:

```
from __future__ import absolute_import, division, print_function

# TensorFlow and tf.keras
import tensorflow as tf
from tensorflow import keras

# Helper libraries
import numpy as np

import matplotlib
matplotlib.use('Agg')
import matplotlib.pyplot as plt

print(tf.__version__)
```

Run this program and verify that it prints a version of Tensorflow.

Next, add the following lines to import the dataset and display some example images:

```
# Download the Fashion MNIST dataset
fashion_mnist = keras.datasets.fashion_mnist

# Extract the training and test images and their labels
(train_images, train_labels), (test_images, test_labels) = fashion_mnist.load_data()

class_names = ['T-shirt/top', 'Trouser', 'Pullover', 'Dress', 'Coat', 
               'Sandal', 'Shirt', 'Sneaker', 'Bag', 'Ankle boot']

# Scale image pixels to (0.0, 1.0) range
train_images = train_images / 255.0
test_images = test_images / 255.0

# Plot examples
plt.figure(figsize=(5,5))
for i in range(9):
    plt.subplot(3,3,i+1)
    plt.xticks([])
    plt.yticks([])
    plt.grid(False)
    plt.imshow(train_images[i], cmap=plt.cm.binary)
    plt.xlabel(class_names[train_labels[i]])
plt.savefig('images.pdf', bbox_inches = 'tight')
```

The code is straightforward. Notice that loading the dataset loads both a set of training images and their labels (this is the 60000 image set) and the separate test set (10000 images). The test labels are available to evaluate the quality of the model once it's been trained.


### Training Montage

Add these lines to the end of your program:

```
# Build a model
model = keras.Sequential([
    keras.layers.Flatten(input_shape=(28, 28)),
    keras.layers.Dense(128, activation=tf.nn.relu),
    keras.layers.Dense(10, activation=tf.nn.softmax)
])

# Compile the model
model.compile(optimizer='adam', 
              loss='sparse_categorical_crossentropy',
              metrics=['accuracy'])

# Train the model
model.fit(train_images, train_labels, epochs=5)

test_loss, test_acc = model.evaluate(test_images, test_labels)
print('Test accuracy:', test_acc)
```

The model creates a 3-layer network

- an input layer, with the 28x28 image automatically unpacked into a 784 element input vector

- a 128 node hidden layer

- a 10 node output layer, with each node corresponding to one of the 10 fashion classes

The `fit` function actually trains the model on the labeled training data. `epochs` controls the number of training iterations.

The last two lines evaluate the trained model against the 10000 element test set. None of the test images were used in the training process, so the test data provides an way to evaluate how well the trained model generalizes outside the data used to create it.

Before you run the code, go back and comment out the `for` loop from the previous section that plots the example images. You don't need to see them now.

A couple of notes on running this code:

- You will see training output for each of the five epochs. I've seen a few instances where it appears to hang, but wait a moment and the training will get moving again.

- Each training band spits out a few numbers, the important one is accuracy, which is the fraction of training images that are classified successfully.

- When you evaluate on the test set, note how the test accuracy compares to the training accuracy. Is it lower? What does that mean?

### Visualize the Results

The following large block of code will allow you to see the results of the training and test process:

```
# Helper functions for viewing results
def plot_image(i, predictions_array, true_label, img):
  predictions_array, true_label, img = predictions_array[i], true_label[i], img[i]
  plt.grid(False)
  plt.xticks([])
  plt.yticks([])
  
  plt.imshow(img, cmap=plt.cm.binary)

  predicted_label = np.argmax(predictions_array)
  if predicted_label == true_label:
    color = 'blue'
  else:
    color = 'red'
  
  plt.xlabel("{} {:2.0f}% ({})".format(class_names[predicted_label],
                                100*np.max(predictions_array),
                                class_names[true_label]),
                                color=color)

def plot_value_array(i, predictions_array, true_label):
  predictions_array, true_label = predictions_array[i], true_label[i]
  plt.grid(False)
  plt.xticks([])
  plt.yticks([])
  thisplot = plt.bar(range(10), predictions_array, color="#777777")
  plt.ylim([0, 1]) 
  predicted_label = np.argmax(predictions_array)
 
  thisplot[predicted_label].set_color('red')
  thisplot[true_label].set_color('blue')
  
predictions = model.predict(test_images)

# Plot a training image and the output of each final layer neuron
i = 0
plt.figure(figsize=(6,3))
plt.subplot(1,2,1)
plot_image(i, predictions, test_labels, test_images)
plt.subplot(1,2,2)
plot_value_array(i, predictions,  test_labels)

plt.savefig('training_image.pdf', bbox_inches = 'tight')


# Plot the first X test images, their predicted label, and the true label
# Color correct predictions in blue, incorrect predictions in red
num_rows = 5
num_cols = 5
num_images = num_rows * num_cols
plt.figure(figsize=(2 * 2 * num_cols, num_rows))
for i in range(num_images):
  plt.subplot(num_rows, 2 * num_cols, 2*i + 1)
  plot_image(i, predictions, test_labels, test_images)
  plt.subplot(num_rows, 2 * num_cols, 2*i + 2)
  plot_value_array(i, predictions, test_labels)
  
plt.savefig('test_results.pdf', bbox_inches = 'tight')
```

### Jam

When you have the entire system running, experiment with making some changes and see if you can drive the test accuracy into the mid-90's. Some things to try:

- Increase the number of training epochs

- Change the number of nodes in the hidden layer

- Add an additional layer to the sequential model
