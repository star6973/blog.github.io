---
layout: post
title:  Deep Learning [Day 7]
date:   2020-07-15 10:00:00-19:00:00
categories: DeepLearning
tag: DeepLearning
use_math: true
---

## Exercise
### 1. MNIST
```python
# -*- coding: utf-8 -*-
import tensorflow as tf
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense, Conv2D, Flatten, Dropout, MaxPooling2D
from tensorflow.keras.preprocessing.image import ImageDataGenerator
import os
import numpy as np
import matplotlib.pyplot as plt

_URL = 'https://storage.googleapis.com/mledu-datasets/cats_and_dogs_filtered.zip'
path_to_zip = tf.keras.utils.get_file('cats_and_dogs.zip', origin=_URL, extract=True)
PATH = os.path.join(os.path.dirname(path_to_zip), 'cats_and_dogs_filtered')

train_dir = os.path.join(PATH, 'train')
validation_dir = os.path.join(PATH, 'validation')

train_cats_dir = os.path.join(train_dir, 'cats')
train_dogs_dir = os.path.join(train_dir, 'dogs')
validation_cats_dir = os.path.join(validation_dir, 'cats')
validation_dogs_dir = os.path.join(validation_dir, 'dogs')

num_cats_tr = len(os.listdir(train_cats_dir))
num_dogs_tr = len(os.listdir(train_dogs_dir))

num_cats_val = len(os.listdir(validation_cats_dir))
num_dogs_val = len(os.listdir(validation_dogs_dir))

total_train = num_cats_tr + num_dogs_tr
total_val = num_cats_val + num_dogs_val

print('total training cat images: ', num_cats_tr)
print('total training dog images: ', num_dogs_tr)
print('total validation cat images: ', num_cats_val)
print('total validation dog images: ', num_dogs_val)
print('--')
print('Total training images: ', total_train)
print('Total validation images: ', total_val)

batch_size = 128
epochs = 15
IMG_HEIGHT = 150
IMG_WIDTH = 150

train_image_generator = ImageDataGenerator(rescale=1./255)
validation_image_generator = ImageDataGenerator(rescale=1./255)

train_data_gen = train_image_generator.flow_from_directory(batch_size=batch_size,
                                                           directory=train_dir,
                                                           shuffle=True,
                                                           target_size=(IMG_HEIGHT, IMG_WIDTH),
                                                           class_mode='binary')

val_data_gen = validation_image_generator.flow_from_directory(batch_size=batch_size,
                                                              directory=validation_dir,
                                                              target_size=(IMG_HEIGHT, IMG_WIDTH),
                                                              class_mode='binary')

sample_training_images, _ = next(train_data_gen)

# This function will plot images in the form of a grid
def plotImages(images_arr):
  fig, axes = plt.subplots(1, 5, figsize=(20, 20))
  axes = axes.flatten()
  for img, ax in zip(images_arr, axes):
    ax.imshow(img)
    ax.axis('off')
  
  plt.tight_layout()
  plt.show()

model = Sequential([
  Conv2D(16, 3, padding='same', activation='relu', input_shape=(IMG_HEIGHT, IMG_WIDTH, 3)),
  MaxPooling2D(),
  Conv2D(32, 3, padding='same', activation='relu'),
  MaxPooling2D(),
  Conv2D(64, 3, padding='same', activation='relu'),
  MaxPooling2D(),
  Flatten(),
  Dense(512, activation='relu'),
  Dense(1)
])

model.compile(optimizer='adam',
              loss=tf.keras.losses.BinaryCrossentropy(from_logits=True),
              metrics=['accuracy'])

model.summary()

history = model.fit_generator(
    train_data_gen,
    steps_per_epoch=total_train // batch_size,
    epochs=epochs,
    validation_data=val_data_gen,
    validation_steps=total_val // batch_size
)

acc = history.history['accuracy']
val_acc = history.history['val_accuracy']

loss = history.history['loss']
val_loss = history.history['val_loss']

epochs_range = range(epochs)

plt.figure(figsize=(8, 8))
plt.subplot(1, 2, 1)
plt.plot(epochs_range, acc, label='Training Accuracy')
plt.plot(epochs_range, val_acc, label='Validation Accuracy')
plt.legend(loc='lower right')
plt.title('Training and Validation Accuracy')

plt.subplot(1, 2, 2)
plt.plot(epochs_range, loss, label='Training Loss')
plt.plot(epochs_range, val_loss, label='Validation Loss')
plt.legend(loc='upper right')
plt.title('Training and Validation Loss')
plt.show()

image_gen = ImageDataGenerator(rescale=1./255, horizontal_flip=True)
train_data_gen = image_gen.flow_from_directory(batch_size=batch_size,
                                               directory=train_dir,
                                               shuffle=True,
                                               target_size=(IMG_HEIGHT, IMG_WIDTH))

augmented_images = [train_data_gen[0][0][0] for i in range(5)]
plotImages(augmented_images)

image_gen = ImageDataGenerator(rescale=1./255, rotation_range=45)
train_data_gen = image_gen.flow_from_directory(batch_size=batch_size,
                                               directory=train_dir,
                                               shuffle=True,
                                               target_size=(IMG_HEIGHT, IMG_WIDTH))

augmented_images = [train_data_gen[0][0][0] for i in range(5)]
plotImages(augmented_images)

image_gen_train = ImageDataGenerator(rescale=1./255, 
                               rotation_range=45,
                               width_shift_range=.15,
                               height_shift_range=.15,
                               horizontal_flip=True,
                               zoom_range=0.5)

train_data_gen = image_gen_train.flow_from_directory(batch_size=batch_size,
                                                     directory=train_dir,
                                                     shuffle=True,
                                                     target_size=(IMG_HEIGHT, IMG_WIDTH),
                                                     class_mode='binary')

augmented_images = [train_data_gen[0][0][0] for i in range(5)]
plotImages(augmented_images)

image_gen_val = ImageDataGenerator(rescale=1./255)
val_data_gen = image_gen_val.flow_from_directory(batch_size=batch_size,
                                                 directory=validation_dir,
                                                 target_size=(IMG_HEIGHT, IMG_WIDTH),
                                                 class_mode='binary')

model_new = Sequential([
  Conv2D(16, 3, padding='same', activation='relu', input_shape=(IMG_HEIGHT, IMG_WIDTH, 3)),
  MaxPooling2D(),
  Dropout(0.2),
  Conv2D(32, 3, padding='same', activation='relu'),
  MaxPooling2D(),
  Dropout(0.2),
  Flatten(),
  Dense(512, activation='relu'),
  Dense(1)
])

model_new.compile(optimizer='adam',
                  loss=tf.keras.losses.BinaryCrossentropy(from_logits=True),
                  metrics=['accuracy'])
model_new.summary()

history = model_new.fit_generator(
    train_data_gen,
    steps_per_epoch=total_train // batch_size,
    epochs=epochs,
    validation_data=val_data_gen,
    validation_steps=total_val // batch_size
)

acc = history.history['accuracy']
val_acc = history.history['val_accuracy']

loss = history.history['loss']
val_loss = history.history['val_loss']

epochs_range = range(epochs)

plt.figure(figsize=(8, 8))
plt.subplot(1, 2, 1)
plt.plot(epochs_range, acc, label='Training Accuracy')
plt.plot(epochs_range, val_acc, label='Validation Accuracy')
plt.legend(loc='lower right')
plt.title('Training and Validation Accuracy')

plt.subplot(1, 2, 2)
plt.plot(epochs_range, loss, label='Training Loss')
plt.plot(epochs_range, val_loss, label='Validation Loss')
plt.legend(loc='upper right')
plt.title('Training and Validation Loss')
plt.show()
```


### 2. YOLO - Image
```python
# -*- coding: utf-8 -*-
!git clone https://github.com/eriklindernoren/PyTorch-YOLOv3

cd PyTorch-YOLOv3/

!pip3 install -r requirements.txt

cd weights

cat /content/PyTorch-YOLOv3/weights/download_weights.sh

ls

cd ..

!python3 detect.py --image_folder data/samples

!pip install keras==2.1.5

!pip install tensorflow==1.5.0

!pip install imageai

from google.colab import drive
drive.mount('/content/drive')

import numpy as np
import io
import matplotlib.pyplot as plt
from matplotlib.image import imread
from imageai.Detection import ObjectDetection
import os

detector = ObjectDetection()

# detector.setModelTypeAsRetinaNet()
detector.setModelTypeAsYOLOv3()
# detector.setModelTypeAsTinyYOLOv3()
path = "/content/drive/My Drive/"
detector.setModelPath(path + "yolo.h5")

detector.loadModel()

detections = detector.detectObjectsFromImage(input_image=path + "image/tt0084058.jpg",
                                             output_image_path=path + "image/tt0084058_output.jpg")

for eachObject in detections:
    print(eachObject["name"] , " : " , eachObject["percentage_probability"] )

img = imread(path + 'image/tt0084058_output.jpg')
plt.imshow(img)
```
<center><img src="/assets/images/deeplearning/83.PNG" width="50%"></center><br>

### 3. YOLO - Video
```python
# -*- coding: utf-8 -*-
# Object Detection with Colab (fear.imageai)

우선 파일을 불러올 수 있도록 드라이브에서 가져올 수 있게, 권한을 받습니다.
링크를 클릭해서 권한을 입력한뒤, Enter를 눌러 드라이브 접근 권한을 얻습니다.

Colab Notebooks에서 imageai_video폴더를 생성후 폴더에 다음 파일들을 위치시켜줍니다.
*   yolo.h5
*   sample_video2.mp4
*   (detected_video2.avi)

detected_video2.avi는 이미 처리가 끝난 파일입니다.
훈련과정에서 오랜 시간이 걸리므로 결과를 확인하기 싶다면 해당 파일을 참고하시면 됩니다.
"""

!pip install imageai

!pip install keras==2.1.5

!pip install tensorflow==1.5.0

from google.colab import drive
drive.mount('/content/drive')

from imageai.Detection import VideoObjectDetection
import os

detector = VideoObjectDetection()

path = "/content/drive/My Drive/Colab Notebooks/imageai_video/"

detector.setModelTypeAsYOLOv3()
detector.setModelPath(path + "yolo.h5")
detector.loadModel()

video_path = detector.detectObjectsFromVideo(input_file_path=path + "sample_video1.mp4",
                                             output_file_path=path + "detected_video1",
                                             frames_per_second=20, log_progress=True)
print(video_path)
# It takes long long time

"""1초당 30프레임씩 분석하기 때문에, 나의 동영상은 13초 길이이므로 총 13 x 30 = 390 프레임을 분석해야 한다."""
```