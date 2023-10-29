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

### Experimentation

#### MobileNetV2 

|                  | recall | precision | negative predicted value | model |
|------------------|-------|-----------|--------------------------| -------- |
| Upper part       | __0.91__  | __0.96__      | __0.94__                     | MobileNetV2 baseline (no img aug) - frozen layers |
| Upper part       | 0.89  | 0.94      | 0.93                     | MobileNetV2 baseline (best img aug) - frozen layers |

|                  | recall | precision | negative predicted value | model |
|------------------|-------|-----------|--------------------------| -------- |
| Bottom part      | __0.96__  | __0.97__      | __1.0__                      | MobileNetV2 baseline (no img aug) - frozen layers |
| Bottom part      | 0.94  | 0.95      | 0.99                      | MobileNetV2 baseline (best img aug) - frozen layers |

|                  | recall | precision | negative predicted value | model |
|------------------|-------|-----------|--------------------------| -------- |
| One piece        | __0.88__  | __0.73__      | __0.99__                     | MobileNetV2 baseline (no img aug) - frozen layers |
| One piece        | 0.8  | 0.67      | 0.98                     | MobileNetV2 baseline (best img aug) - frozen layers |

|                  | recall | precision | negative predicted value | model |
|------------------|-------|-----------|--------------------------| -------- |
| Footwear         | __0.99__  | __0.99__      | __1.0__                      | MobileNetV2 baseline (no img aug) - frozen layers |
| Footwear         | 0.98  | 0.99      | 0.99                      | MobileNetV2 baseline (best img aug) - frozen layers |

|                  | recall | precision | negative predicted value | model |
|------------------|-------|-----------|--------------------------| -------- |
| Bags             | __0.96__  | __0.92__      | __1.0__                      | MobileNetV2 baseline (no img aug) - frozen layers |
| Bags             | 0.92  | 0.89      | 0.99                      | MobileNetV2 baseline (best img aug) - frozen layers |

* __Baseline model achieves better results__

* __Performance with best MobileNetV2 (no image augmentation + frozen layers)__

|                  | number of parameters | model size (MB) | test inference time (seconds) |
|------------------|-------|-----------|--------------------------|
| | 2230277 | 8.8 MB | 37.6 sec |


## CI/CD Deployment plan

## SQL Relational Database

Let’s imagine that now you have to load the product images from our database along with some additional text features like the product description provided by the retailers. The data is stored in an SQL relational database (postgresql) with the following schema:

<p align="center">
         <img width="450" src="./media/sql_database_schema.png">
</p>

Write a query (either in django ORM or in SQL) to extract, for every existing product, the following fields:
* Product. Title
* Image. Url for the images with the ImageIndex = 0. _ImageIndex field states the priority order of images of a certain product. So for a given ProductId, the image with ImageIndex = 0 would be the most relevant image for that product_
* ProductDescription. TranslatedText if exists, else ProductDescription.OriginalText for ProductDescriptions in CountryCode = ‘us’

```
SELECT Product.Title AS ProductTitle,
       Image.Url AS ImageURL,
       CASE
         WHEN ProductDescription.TranslatedText IS NOT NULL THEN ProductDescription.TranslatedText
         ELSE ProductDescription.OriginalText
       END AS ProductDescriptionText
FROM Product LEFT JOIN ProductImages ON Product.Id = ProductImages.ProductId
             LEFT JOIN ProductDescription ON Product.Id = ProductDescription.Id
             LEFT JOIN Image ON Product.Id = Image.Id
WHERE ProductImages.ImageIndex = 0
```
