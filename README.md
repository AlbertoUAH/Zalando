# Zalando
Automatic classification of fashion images (by [Zalando](https://github.com/zalandoresearch/fashion-mnist)) via Deep Learning

<div style="text-align: center;">
         <img width="450" src="./media/fashion-mnist-sprite.png">
</div>

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
