---
title: "Machine Learning Project Proposal"
date: 2024-06-14T19:04:15-04:00
draft: false
---
## Introduction/Background

Baseball is one of the most popular sports in the United States, often referred to as “America’s pastime.” With Major League Baseball’s total revenue consistently increasing [1] and the betting industry reaching a revenue of $7 billion in 2023 [2], data analytics has become increasingly important. The MLB collects extensive statistics on baseball performance metrics for teams and players. In particular, the analysis of pitch trajectories is crucial for training pitchers and providing additional content for viewers [3]. This project will utilize data sourced from MLB’s Statcast Search [4], focusing on analyzing the performance of multiple teams from the 2023 season.

## Problem Definition

**Problem:** In baseball, numerous factors contribute to winning a game, and various statistics can predict the success of both teams and individual players. Pitching is arguably the most critical aspect of baseball, as every subsequent game action depends on the outcome of each pitch [5]. This project will focus on pitching, using pitch data such as pitch outcome, type, speed, and movement to predict the likelihood of a strike. This information can then be used to train both pitchers and batters.

**Motivation:** Understanding and predicting pitch outcomes can significantly enhance training methods and strategies in baseball. By accurately predicting the likelihood of a strike, teams can make more informed decisions, improving overall performance.

## Methods

To predict the likelihood of a strike using pitch data from MLB’s Statcast Search, we will employ comprehensive data preprocessing techniques and various machine learning algorithms.

### Data Preprocessing Methods

1. **Feature Scaling:**
   - **StandardScaler** from scikit-learn will standardize features like speed, movement, and spin rate, which are on different scales, to improve the performance of machine learning algorithms.

2. **Encoding Categorical Features:**
   - **OneHotEncoder** from scikit-learn will transform categorical features such as pitch type and pitcher handedness into binary features.

3. **Feature Selection:**
   - **SelectKBest** from scikit-learn will select the best features based on univariate statistical tests, removing all but the k highest scoring features.

### Machine Learning Algorithms

1. **Logistic Regression (Supervised Learning):**
   - **LogisticRegression** from scikit-learn will be used for binary classification to predict if a pitch is a strike (1) or not (0). This model is simple and can indicate which features are most important in predicting a strike based on the coefficient of each feature.

2. **Random Forest Classifier (Supervised Learning):**
   - **RandomForestClassifier** from scikit-learn, an ensemble of decision trees, will capture complex interactions between features. This method can reveal important pitch characteristics and their relationships with the probability of a strike.

3. **Support Vector Machine (Supervised Learning):**
   - **SVC** from scikit-learn will be used for classification, capable of handling high-dimensional spaces and complex decision boundaries through various kernel functions.

## (Potential) Results and Discussion

### Quantitative Metrics

1. **Confusion Matrix:**
   - To compare the model's predictions with real-world values, identifying false positives in pitch analysis.

2. **Feature Scores:**
   - To determine which pitch characteristics contribute most to predicting the outcome.

3. **ROC Curve:**
   - To compare models and assess the difference between random guessing and model predictions.

### Project Goals and Expected Results

Our goal is to use these metrics to validate our model’s predictions on pitch outcomes in baseball games. We expect the results to accurately predict the likelihood of a strike, enhancing the training and strategic decisions for teams and players.

## Gantt Chart
![](/img/ganttchart.png)

## Contributions
![](img/contri.png)


## References

1. C. Gough, “MLB league revenue 2024,” Statista. [Online]. Available: [https://www.statista.com/statistics/193466/total-league-revenue-of-the-mlb-since-2005/](https://www.statista.com/statistics/193466/total-league-revenue-of-the-mlb-since-2005/)
2. “US Sports Betting Revenue Tracker - how much revenue is each state generating in 2023?,” Oddspedia. [Online]. Available: [https://oddspedia.com/us/betting/sports-betting-revenue](https://oddspedia.com/us/betting/sports-betting-revenue)
3. H. Lee, J. Kim, J. Kim, J. Yu, and W. -Y. Kim, "Start-End Time Detection in Baseball Videos for Automatic Pitching Trajectory Analysis," 2019 International Conference on Electronics, Information, and Communication (ICEIC), Auckland, New Zealand, 2019, pp. 1-4, doi: 10.23919/ELINFOCOM.2019.8706498.
4. “Statcast Search,” baseballsavant.mlb.com. [Online]. Available: [https://baseballsavant.mlb.com/statcast_search](https://baseballsavant.mlb.com/statcast_search)
5. H. Lee, J. Kim, J. Kim and W. -Y. Kim, "A Method of Measuring Baseball Position at the Strike Zone," 2020 International Conference on Electronics, Information, and Communication (ICEIC), Barcelona, Spain, 2020, pp. 1-3, doi: 10.1109/ICEIC49074.2020.9051039.
6. J. Schuh and L. Kong, "Classifying Pitch Types in Baseball Using Machine Learning Algorithms," 2023 IEEE Asia-Pacific Conference on Computer Science and Data Engineering (CSDE), Nadi, Fiji, 2023, pp. 1-6, doi: 10.1109/CSDE59766.2023.10487702.
