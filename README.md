
**Goal:** Predict future Instacart reorder and average cart size based off behaviors of initial orders. Identify the most active audience (high reorder, high cart size) and their most important behaviors. 

**Business use:**  Audience segmentation, marketing, lookalike modeling, revenue/inventory prediction

**Data:** 3 million orders, 200k users, 32MM+ items purchased; data included information like product, department, aisle, days since prior order, order number, DOW, hour, cart size and were stored in local Postgresql tables for ease.   

**Process:**  The initial dataset I used was incredibly well-kept - there were no invalid data or NaNs to deal with. However, I noticed that Instacart aggregated days since last order data >30 days into one bucket, which created distortions in the actual order gap distribution, so I distributed these randomly throughout the days 30-60 to create a longer tail. 

In addition, I decided to evaluate deviations in time and order gap data by creating mean and standard deviation features for each order. I believe that those who consistently order or schedule their orders would be more likely to reorder the same products i.e. are creatures of habit. In addition, I created a streak feature to evaluate frequency they repurchase product in their first orders.

I created 6 classes for behavior made from cart size tertiles and reorder top/bottom halves to predict which I called personas, which I upsampled to increase predictive power across the different classes. 

> Whales - High reorder rates, high avg cart sizes
> Guppies - High reorder rates, medium avg cart sizes
> Ad Hoc Purchasers - High reorder rates, small avg cart sizes
> Explorers - Low reorder rates, high avg cart sizes
> Avg Consumer - Low reorder rates, medium avg cart sizes
> Snap Decision Makers - Low reorder rates, low avg cart sizes


I leverage several different models ex. one vs all logistic , Random Forest Classifier, Gradient Boosting Classifier, Adaboost, and a voting ensembler that incorporated all of the different models. While the ensembler was marginally better, it wasn't significant enough to warrant the computational expense so I just leveraged RFC as the best model. 

I used grid search to optimize hyperparameters, including number of trees and tree depth. 

Ultimately I achieved an F1 score of 70% through the optimized classifier. 

**Best predictors:**   
Total products in initial orders
Log avg cart size
0 streak 
Order gap standard deviation
