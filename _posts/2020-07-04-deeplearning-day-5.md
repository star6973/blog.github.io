---
layout: post
title:  Deep Learning [Day 5]
date:   2020-07-04 10:00:00-19:00:00
categories: DeepLearning
tag: DeepLearning
use_math: true
---

## Exercise
### 문제
keras에서 cifar10 불러오기  
이미지데이터를 0과 1사이로 보정  
레이블데이터를 one-hot encoding으로 변경  
<br>

문제 1) CNN으로 구성(138, 139)  
1번째 conv2d레이어 3by3 필터, 90개  
2번째 conv2d레이어 3by3 필터, 90개  
3번재 레이어 softmax의 output레이어  
<br>

문제 2) MLP로 구성(이미지 데이터를 1차원으로 변형)  
multi-layer perception으로 구성  
1번째 히든레이어 unit 개수 2000개  
2번째 히든레이어 unit 개수 3000개  
3번째 히든레이어 unit 개수 2000개  
output레이어 unit 개수 10개  
<br>

힌트) MNIST 경우 mnist.train.next_batch() 등의 함수로 데이터를 가져왔으나 cifar10은 이를 지원하지 않음. 따라서 139페이지처럼 구현  

## *Tensorflow*
CNN으로 구성

```python
from keras.utils import np_utils
from keras.datasets import cifar10
import tensorflow as tf
import time

(train_images, train_labels), (test_images, test_labels) = cifar10.load_data()
train_images = train_images.reshape(train_images.shape[0], 32, 32, 3).astype('float32')/255.0
test_images = test_images.reshape(test_images.shape[0], 32, 32, 3).astype('float32')/255.0
train_labels = np_utils.to_categorical(train_labels)
test_labels = np_utils.to_categorical(test_labels)

training_epochs = 15
batch_size = 100

X = tf.placeholder(tf.float32, [None, 32, 32, 3])
Y = tf.placeholder(tf.float32, [None, 10])
X_img = tf.reshape(X, [-1, 32, 32, 3])

# Convolution Layer 1
W1 = tf.Variable(tf.random_normal([3, 3, 3, 90], stddev=0.01))
CL1 = tf.nn.conv2d(X_img, W1, strides=[1, 1, 1, 1], padding='SAME')
CL1 = tf.nn.relu(CL1)

# Convolution Layer 2
W2 = tf.Variable(tf.random_normal([3, 3, 90, 90], stddev=0.01))
CL2 = tf.nn.conv2d(CL1, W2, strides=[1, 1, 1, 1], padding='SAME')
CL2 = tf.nn.relu(CL2)

# Fully Connected Layer
L_flat = tf.reshape(CL2, [-1, 32*32*90])
W3 = tf.Variable(tf.random_normal([32*32*90, 10], stddev=0.01))
b3 = tf.Variable(tf.random_normal([10]))

# Model, Cost, Train
model_LC = tf.matmul(L_flat, W3) + b3
model = tf.nn.softmax(model_LC)
cost = tf.reduce_mean(tf.nn.softmax_cross_entropy_with_logits_v2(logits=model_LC, labels=Y))
train = tf.train.AdamOptimizer(0.01).minimize(cost)

# Accuracy
accuracy = tf.reduce_mean(tf.cast(tf.equal(tf.argmax(model, 1), tf.argmax(Y, 1)), tf.float32))

# Session
with tf.Session() as sess:
  sess.run(tf.global_variables_initializer())

  # Training
  t1 = time.time()
  for epoch in range(training_epochs):
    total_batch = int(train_images.shape[0] / batch_size)
    for i in range(total_batch):
      batch_train_images = train_images[i*batch_size : (i+1)*batch_size]
      batch_train_labels = train_labels[i*batch_size : (i+1)*batch_size]
      c, _ = sess.run([cost, train], feed_dict={X: batch_train_images, Y: batch_train_labels})

      if i % 10 == 0:
        print('epoch: ', epoch, ', batch number: ', i)

  t2 = time.time()
  
  # Testing
  print('Training Time (Seconds): ', t2 - t1)

  total_batch = int(test_images.shape[0] / batch_size)
  for i in range(total_batch):
    batch_test_images = test_images[i*batch_size : (i+1)*batch_size]
    batch_test_labels = test_labels[i*batch_size : (i+1)*batch_size]

  print('Accuracy: ', sess.run(accurcay, feed_dict={X: batch_test_images, Y: batch_test_labels}))
```
<br>

ANN으로 구성

```python
from keras.utils import np_utils
import tensorflow as tf
from keras.datasets import cifar10
from keras.models import Sequential
from keras.layers import Dense, Flatten

# cifar10 데이터 가져오기
(train_images, train_labels), (test_images, test_labels) = cifar10.load_data()
train_images = train_images.reshape(train_images.shape[0], 32, 32, 3).astype('float32')/255.0
test_images = test_images.reshape(test_images.shape[0], 32, 32, 3).astype('float32')/255.0
train_labels = np_utils.to_categorical(train_labels)
test_labels = np_utils.to_categorical(test_labels)
print(train_images.shape, train_labels.shape, test_images.shape, test_labels.shape)

training_epochs = 5
batch_size = 100

nH1 = 2000
nH2 = 3000
nH3 = 2000

X = tf.placeholder(tf.float32, [None, 32, 32, 3])
Y = tf.placeholder(tf.float32, [None, 10])
X_img = tf.reshape(X, [-1, 32*32*3])

def mlp_LC(img):
  HL1 = tf.nn.relu(tf.add(tf.matmul(img, W['HL1']), b['HL1']))
  HL2 = tf.nn.relu(tf.add(tf.matmul(HL1, W['HL2']), b['HL2']))
  HL3 = tf.nn.relu(tf.add(tf.matmul(HL2, W['HL3']), b['HL3']))
  Out = tf.matmul(HL3, W['Out']) + b['Out']
  return Out

W = {
    'HL1': tf.Variable(tf.random_normal([32*32*3, nH1])),
    'HL2': tf.Variable(tf.random_normal([nH1, nH2])),
    'HL3': tf.Variable(tf.random_normal([nH2, nH3])),
    'Out': tf.Variable(tf.random_normal([nH3, 10]))
}

b = {
    'HL1': tf.Variable(tf.random_normal([nH1])),
    'HL2': tf.Variable(tf.random_normal([nH2])),
    'HL3': tf.Variable(tf.random_normal([nH3])),
    'Out': tf.Variable(tf.random_normal([10]))
}

# Model, Cost, Train
model_LC = mlp_LC(X_img)
model = tf.nn.softmax(model_LC)
cost = tf.reduce_mean(tf.nn.softmax_cross_entropy_with_logits_v2(logits=model_LC, labels=Y))
train = tf.train.AdamOptimizer(0.01).minimize(cost)

# Accuracy
accuracy = tf.reduce_mean(tf.cast(tf.equal(tf.argmax(model, 1), tf.argmax(Y, 1)), tf.float32))

# Session
with tf.Session() as sess:
  sess.run(tf.global_variables_initializer())

  # Training
  for epoch in range(training_epochs):
    total_batch = int(train_images.shape[0] / batch_size)

    # batch_size가 너무 크면 메모리가 잡아먹어, 시간이 오래 걸리는 문제가 있지만,
    # 반대로, batch_size가 너무 작으면 학습이 잘 이루어지지 않아, overfitting 문제가 있을 수 있다.
    for i in range(total_batch):
      batch_train_images = train_images[i*batch_size : (i+1)*batch_size]
      batch_train_labels = train_labels[i*batch_size : (i+1)*batch_size]
      c, _ = sess.run([cost, train], feed_dict={X: batch_train_images, Y: batch_train_labels})

      if i % 100 == 0:
        print('epoch: ', epoch, ', batch number: ', i)

  # Testing
  total_batch = int(test_images.shape[0] / batch_size)
  for i in range(total_batch):
    batch_test_images = test_images[i*batch_size : (i+1)*batch_size]
    batch_test_labels = test_labels[i*batch_size : (i+1)*batch_size]

    print('Accuracy: ', sess.run(accuracy, feed_dict={X: batch_test_images, Y: batch_test_labels}))
```
<br>

## *Keras*
CNN으로 구성

```python
# 138p에 6번 7번 사이에 train_images test_images 255로 나눈 것 추가
from keras.utils import np_utils
from keras.datasets import cifar10
from keras.models import Sequential
from keras.layers import Conv2D, pooling, Flatten, Dense

(train_images, train_labels), (test_images, test_labels) = cifar10.load_data()
print(train_images.shape, train_labels.shape, test_images.shape, test_labels.shape)

train_images = train_images.reshape(train_images.shape[0], 32, 32, 3).astype('float32') / 255.0
test_images = test_images.reshape(test_images.shape[0], 32, 32, 3).astype('float32') / 255.0

train_labels = np_utils.to_categorical(train_labels)
test_labels = np_utils.to_categorical(test_labels)

model = Sequential()
model.add(Conv2D(90, (3, 3), padding='same', strides=(1, 1), activation='relu'))
model.add(pooling.MaxPooling2D(pool_size=(2, 2)))
model.add(Conv2D(90, (3, 3), padding='same', strides=(1, 1), activation='relu'))
model.add(pooling.MaxPooling2D(pool_size=(2, 2)))
model.add(Flatten())
model.add(Dense(10, activation='softmax'))
model.compile(loss='categorical_crossentropy', optimizer='sgd', metrics=['accuracy'])

model.fit(train_images, train_labels, epochs=5, batch_size=100, verbose=1)
_, accuracy = model.evaluate(test_images, test_labels)
print('Accuracy: ', accuracy)
model.summary()
```
<br>

ANN으로 구성

```python
from keras.utils import np_utils
from keras.datasets import cifar10
from keras.models import Sequential
from keras.layers import Dense

(train_images, train_labels), (test_images, test_labels) = cifar10.load_data()
train_images = train_images.reshape(train_images.shape[0], 32*32*3).astype('float32')/255.0
test_images = test_images.reshape(test_images.shape[0], 32*32*3).astype('float32')/255.0

train_labels = np_utils.to_categorical(train_labels)
test_labels = np_utils.to_categorical(test_labels)

model = Sequential()
model.add(Dense(2000, activation='relu'))
model.add(Dense(3000, activation='relu'))
model.add(Dense(2000, activation='relu'))
model.add(Dense(10, activation='softmax'))
model.compile(loss='categorical_crossentropy', optimizer='sgd', metrics=['accuracy'])

# Training
model.fit(train_images, train_labels, epochs=5, batch_size=100, verbose=1)

# Testing
_, accuracy = model.evaluate(test_images, test_labels)

print('Accuracy: ', accuracy)
model.summary()
```
