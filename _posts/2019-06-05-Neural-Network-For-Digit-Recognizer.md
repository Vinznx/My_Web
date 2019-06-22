---
layout: post
title:  "Kaggle: Neural Network For Digit Recognizer"
date:   2019-06-25 08:00:00 -0400
categories: Interesting-projects
author: Ningxiao Zhang
permalink: "/posts/neural-network-for-digit-recognizer/"
---

Neural Network has been successfully used in digit recognizer. And it is very easy to just apply some packages like tensorflow. However, it requires some tips in analyzing the data to get a better result. Here I am writing a demonstration of how to improve the accuracy of my neural network code from 70% to 99%.

# Include packages
The TensorFlow version is 1.13.1
```css
%matplotlib inline
import matplotlib.pyplot as plt
import tensorflow as tf
import pandas as pd
from sklearn.model_selection import train_test_split
```

# Download data
The data used in this demonstration is from Kaggle  <https://www.kaggle.com/c/digit-recognizer/data>. The training sample is saved as 'train.csv' under the data folder.
```css
df_train = pd.read_csv('data/train.csv')
df_tr_data = df_train.iloc[:,1:]
df_tr_label = df_train.iloc[:,0]
```
The size of this training sample is 42,000.

# Data details
Each image is a 28 x 28 pixels image, the number of colour channels is 1 (gray-scale), and there are 10 classes represent digit 0 to 9. Here are three images from this data set with label of digit 1, digit 0 and digit 4.

<img src="../../assets/img/nn_img1.png" alt="nn_img1" width="200"/>
<img src="../../assets/img/nn_img2.png" alt="nn_img2" width="200"/>
<img src="../../assets/img/nn_img3.png" alt="nn_img3" width="200"/>

# Split data for training and testing
```css
x_train,x_test,y_train,y_test =  train_test_split(df_tr_data,df_tr_label,test_size=0.33, random_stat=40)
```

# A simple 2-layers neural network
This simple neural network's core only has 7 lines in total and it can help your model to reach an accuracy of 70%. This part is the core of the whole neural network with three components. First, you need to write the flow of the model. Here I just choose two layers with 256 and 128 nodes, respectively, and use relu function as the activation. Second, you need to set the optimizer and loss function. The last one is just fit the model.
```css
model = tf.keras.models.Sequential()
model.add(tf.keras.layers.Flatten())
model.add(tf.keras.layers.Dense(256,activation=tf.nn.relu))
model.add(tf.keras.layers.Dense(128,activation=tf.nn.relu))
model.add(tf.keras.layers.Dense(10,activation=tf.nn.softmax))

model.compile(optimizer='adam',loss='sparse_categorical_crossentropy',metrics=['accuracy'])

model.fit(x_train,y_train,epochs=10)
```

# Evaluate the result
The result of this method is evaluated by an accuracy.
```css
predictions = model.predict([x_test])
correct = np.equal(np.argmax(predictions,axis=1),y_test)
accuracy = np.sum(correct)/len(predictions)
print ('Accuracy of the model on test sample is:', accuracy)
```
