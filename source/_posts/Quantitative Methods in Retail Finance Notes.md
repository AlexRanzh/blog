---
title: Quantitative Method in Retail Finance Notes 1
date: 2019-4-13 23:52:11
categories:
    - Notes
    - Math Notes
tags:
    - Credit modelling

    - redis
---
This notes follows materials from the slides for M3S17 at Imperial College London Mathematics Department by Dr. Tony Bellotti. Questions mentioned are referring to past exam papers of M3S17 at Imperial College London.

# Chapter 7: Fraud Detection in Retail Credit

## Types of Fraud:
  - Theft
  - Counterfeit
  - Application
  - Card-not-present/online

## Automated Fraud Detection
This essentially is a classification problem, legitimate transaction Y=1, illegal Y=0.

**Special problems for fraud detection (that distinguish it from other classification problems)**
> 2017 Q3(a) What special characteristics does fraud detection have, considered as a classification problem?
∗ Need to process millions of transactions in real time.
∗ Highly imbalanced classification problem: ratio of fraudulent to legitimate
transactions is typically very small.
∗ Nature of fraud is reflexive. That is, fraudsters adapt to the detection methods applied by banks to stop them.

  1. High volume data.
  2. Data highly imbalanced.
  3. Reflexive nature.

*difference of fraud detection problem from application model?*
  Do not need to explain the model, so nonlinear complicated models are allowed.

## Business Rule method
>For example, the card is used abroad
and it had not been used in the past year in the country
and the card holder did not tell the bank they will be abroad

## Predictive Model method
Construct a two-class classifier - a fraud scorecard.
- the higher the score, the more trustworthy the account is.
- classifier model with function $f$: $\hat{y} = f(x,\theta)$, and estimate $\theta$ with data x.
- filter out some legitimate transaction to balance training data set. e.g. discard small amount transactions, regular transactions and inactive accounts.
**past research have shown that a linear model is not enough, non-linear classifiers like ANN works better.**
- This method works well in detecting known type fraudulent transactions, but not able to deal with new fraud types.

## Anomaly Detection method

Another method is to model only the legal transactions and detect any abnormal new transactions.

### Comparing with predictive modelling:
  pros:
  1. it can detect new types of fraud
  2. it do not worry about the imbalanced data(coz 1)
  con:
  1. it is not as sensible to transactions that are similar to legitimate ones

### One-class classifier only model legitimate pdf over predictor variables
>2017Q3(b) What are the key differences between one-class and two-class classifiers for fraud?
∗ Two-class classifiers are modelling the difference between fraudulent and legitimate transactions.
∗ One-class classifiers just model distribution of legitimate transactions.
∗ One-class classifiers are well-suited to deal with the reflexivity of fraud, since they do not explicitly model the existing pattern of fraud.

### Steps for Anomaly Detection:
1. drop out abnormal transactions: fraudulent transactions, errors, genuine but outliers
2. Let $S = (\mathbf{x_{1}},\mathbf{x_{2}},...,\mathbf{x_{n}})$ be a training sequence of legitimate transactions.
3. estimate $f(\mathbf{x}|S,\gamma)$ where $\gamma$ is an estimation parameter.
4. A classification decision is made with a new observation $\mathbf{x_{new}}$ at:
$$
\hat(y) = I(f(\mathbf{x_{new}}|S,\gamma)>\theta)
$$
where $\theta$ is a threshold in probability.
>2017Q3(e) Which of these will register a fraud alert, based on your result in part (d)?
ANS: T2 and T3 have density less than θ = 0.05 and so will register a fraud alert.

*The threshold $\theta$ is manually set based on the (sensible) strategy of controlling the fraction $\epsilon$ of legitimate cases to be classified as anomalous, based on training data.*
---- This is constrained by how many false alert is generated/business resource can be input to followups.

> 2017 Q3(d) Given a false alarm rate ε = 1/8, and based only on this data, what is the value of θ?
ANS:
Use formula $$
max \theta s.t. \sum^{n}_{i=1}I(f(x_i) > \theta) \leq n(1 − \epsilon)$$
where $$n(1 − \epsilon) = 24 × 7/8 = 21$$
Therefore, count lowest (24 − 21) = 3 densities from the table gives $\theta$ = 0.05.

i.e.
$$
min\theta s.t.  \sum_{i=1}^{n}I(f(\mathbf{x}_{i}|S,\gamma)>\theta)=floor(n\\epsilon)
$$

### Kernel Density Estimator
Crude empirical density:
$$
\hat{F_{EMP}}= \frac{1}{n}\sum_{i=1}^{n}I(x_{i}\leqslant x)
$$

kernel density function:
> 2017Q3(c) Describe Parzen density estimation.

$$
\hat{f(x)} = \frac{1}{nh^m}\sum_{i=1}^{n} K(\frac{x-x_{i}}{h})
$$s.t.
1. $K(z) = K(-z)$
2. $\int K(z) dz = 1$
3. $\mathbf{x_1}, \mathbf{x_2}, ..., \mathbf{x_n}$ is observations with dimension m
4. h is bandwidth constant

### Available data
- Account Data
- Transaction Data
- Personal Data
- Location data

## Social Network Analysis
Using weight $w_{ij}$ to estimate **association feature/degree of connectedness to fraud people** $F_{i}$ and hence have fraud propensity from it.
>2017Q3(f) Describe in words what Fi is measuring?
(f) Variable $F_{i}$ measures the degree of connectedness to other people committing fraud.

### Formula:
$$
F_{i} = \frac{1}{\sum_{i=1}^{n}w_{ij}} \sum_{j=1}^{n} w_{ij}p_{j}
$$
>2017Q3(g) What range of values can Fi take?
$0\leq F_i \leq 1$

>2017Q3(h) Calculate the value of Fi for T4 (node 3)?
Use formula in the question to get (4 + 1)/(2 + 4 + 1 + 1) = 5/8 = 0.625.

>**2017Q3(i) How are values of Fi broadly affecting the density?**
(i)
Low values of $F_i$ are inducing a small **increase** in density.
High values of $F_i$ are inducing a large **decrease** in density.
**Low density means default**

## Model Evaluation
Special performance evaluation for this problem:
1. proportion fraud detected
2. keep low false alarm rate, not to upset customer
3. cost monitoring automated fraud alert

### Receiver-operating characteristics (ROC)
Two CDF measures:
$$y = {0,1}  F_{y}(c)=P(S\leq c|Y=y)$$
- y=0: given a threshold c, probability of fraud transaction that identified as fraud)
- y=1: given a threshold c, probability of false alert generated

### Area under ROC curve(AUC)
$$
A = \int F_{1}(c)F_{0}'(c)dc
$$

### Precision, Recall and alarm rate
- $F(c) = P(S\leq c)$
- $p_{0} = P(Y=0)$
- Recall: $F_{0}(c)$, the proportion of fraud transactions that are detected
- Precision: the proportion alerts that are actually fraud
$P(Y=0|S\leq c)=\frac{F_{0}(c)p_0}{F(c)}$
- Alarm rate: $F(c)= p_0 \frac{recall}{precision}$, unconditional probability of alarm
**Monitoring cost increases linear with alarm rate**

### Precision-recall (PR) curve
properties:
- typical decreasing
- best: 1,1
- worst: $F(c)=F_0 (c)$, horizontal line with $precision =p_0$
