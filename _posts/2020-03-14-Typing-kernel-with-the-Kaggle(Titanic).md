---
layout: post
title:  Typing kernel with the Kaggle(Titanic)
date:   2020-03-14 00:51:20
categories: typing kernel
tags: Typing Kernel
---


```python

#%% md

## 노트북 목차
---
### 파트1: 탐색적 데이터 분석(EDA)
1) 특성 분석  
2) 여러 특성을 고려한 관계 또는 트렌드 찾기   

### 파트2: Feature Engineering 및 데이터 정제
1) 새로운 특성 추가하기  
2) 중복 특성 제거하기  
3) 모델링에 적합한 형태로 특성 변환하기  

### 파트3: 모델링 예측
1) 기본적인 알고리즘  
2) 교차 검증  
3) 앙상블 기법  
4) 중요한 특징 추출  

#%% md

## 파트1: 탐색적 데이터 분석(EDA)

#%%

import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
plt.style.use('fivethirtyeight')
import warnings
warnings.filterwarnings('ignore')
%matplotlib inline

#%%

data = pd.read_csv('C:/Users/battl/PycharmProjects/cse_project/coding practice/Kaggle/Titanic/train.csv')

#%%

data.head()
    
```

A blog is a Web site that enables you or your organization to quickly share ideas and information. Blogs contain posts that are dated and listed in reverse chronological order. People can comment on your posts, as well as provide links to interesting sites, photos, and related blogs.

Posts are an essential part of a blog. They are typically journal-like entries that contain information, ideas, and opinions. They are displayed in chronological order, starting with the most recent posts.

You can create a post on a SharePoint blog by using a Web browser, if you have permission to contribute to the Posts list. You don't need additional tools or programs to create content, add pictures, apply formatting, and insert hyperlinks. To post comments to a blog, you must have permission to contribute to the Comments list. In some cases, comments must be approved before they will appear on the site.

 Note   If your Web browser doesn't support ActiveX controls, then the formatting toolbar may not be available. However, you can use basic HTML tags to format the text in your blog posts.

[![This is a test](http://lorempixel.com/400/200/sports/1/ "A title!"){:class="img-responsive image-center thumbnail " height="200px" width="400px"}](http://lorempixel.com/400/200/sports/1/){:class="ltbox"}

You can also post content to a blog by doing the following:

    Creating and submitting a post in an e-mail message. In order to submit a post in an e-mail message, your blog must be enabled to receive content in e-mail messages.
    Creating a post by using a blog creation and publishing tool that is compatible with a SharePoint blogs.

By default, blogs are set up so that approval is required before posts are published. This can be helpful if numerous people are publishing content, or if a post could contain sensitive content. By default, people who have permission to manage lists can approve blog posts, but you can customize those settings.
