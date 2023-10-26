# Zalando
Automatic classification of fashion images (by [Zalando](https://github.com/zalandoresearch/fashion-mnist)) via Deep Learning

<p align="center">
         <img width="450" src="./media/fashion-mnist-sprite.png">
</p>

## Description and objective

The original Zalando dataset contains 10 different labels:

0. __T-shirt/top__
1. __Trouser__
2. __Pullover__
3. __Dress__
4. __Coat__
5. __Sandal__
6. __Shirt__
7. __Sneaker__
8. __Bag__
9. __Ankle boot__

In this case, we want to group the original labels in 5 new labels with the following mapping between them:

* __Upper part__: T-shirt/top + Pullover + Coat + Shirt Bottom part: Trouser
* __One piece__: Dress
* __Footwear__: Sandal + Sneaker + Ankle boot Bags: Bag


The goal of this exercise is to understand the dataset and experiment with deep learning techniques to train a classifier for the given grouped classes (Upper part, Bottom Part, One piece, Footwear and Bags).

## Data Understanding and Exploratory Data Analysis (EDA)

1. Fashion-MNIST is a dataset of Zalando's article images—consisting of:
* __A training set of 60,000 examples__
* __A test set of 10,000 examples__
* It shares the same image size and structure of training and testing splits.

2. Image size: __28x28__
3. Image type: __grayscale image__
4. Duplicated images: __no__
5. Label distribution: although original labels are balanced for both labels, new labels are unbalanced:

```
# -- Train data
Upper part     24000 -> (Represents 40 % of training data)
Footwear       18000 -> (Represents 30 % of training data)
One piece       6000 -> (Represents 10 % of training data)
Bottom part     6000 -> (Represents 10 % of training data)
Bags            6000 -> (Represents 10 % of training data)

# -- Test data
Upper part     4000 -> (Represents 40 % of training data)
Footwear       3000 -> (Represents 30 % of training data)
Bottom part    1000 -> (Represents 10 % of training data)
One piece      1000 -> (Represents 10 % of training data)
Bags           1000 -> (Represents 10 % of training data)
```

Both datasets have an identical distribution (important note for train-validation split) 

6. Pixels distribution
* Min value: 0
* Max value: 255

6.1. Average pixel distribution per group

<p align="center">
         <img width="450" src="./media/avg_pixel_value_per_class.png">
</p>

6.2. Median pixel distribution per group

<p align="center">
         <img width="450" src="./media/median_pixel_value_per_class.png">
</p>

## Deep Learning Experimentation and Metrics

1. Standarization

2. Train/validation split

## CI/CD Deployment plan

## SQL Relational Database

Let’s imagine that now you have to load the product images from our database along with some additional text features like the product description provided by the retailers. The data is stored in an SQL relational database (postgresql) with the following schema:

<p align="center">
         <img width="450" src="./media/sql_database_schema.png">
</p>
