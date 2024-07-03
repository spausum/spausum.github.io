---
title: "Midterm Report"
date: 2024-07-03T03:01:50-04:00
draft: false
---
## Introduction/Background

Baseball is one of the most popular sports in the United States, often referred to as “America’s pastime.” With Major League Baseball’s total revenue consistently increasing [1] and the betting industry reaching a revenue of $7 billion in 2023 [2], data analytics has become increasingly important. The MLB collects extensive statistics on baseball performance metrics for teams and players. In particular, the analysis of pitch trajectories is crucial for training pitchers and batters alike. Pitch data can also provide additional content for viewers in the form of 3D pitch trajectory visualizations during lulls in the game [3]. This project will utilize data sourced from MLB’s Statcast Search [4] – focusing on data from the Atlanta Braves 2018 MLB season.

## Problem Definition

**Problem:** In baseball, numerous factors contribute to winning a game, and various statistics can predict the success of both teams and individual players. Pitching is arguably the most critical aspect of baseball, as every subsequent game action depends on the outcome of each pitch [5]. 

Initially, this project aimed to focus on pitching, using pitch data such as pitch outcome, type, speed, and movement to predict the likelihood of a strike. The goal was to use this information to train both pitchers and batters. However, our problem definition has since shifted due to the immense complexity of baseball pitching strategy and the large number of factors that can affect whether or not a specific pitch results in a strike. Factors such as the number of players on base, the level of exhaustion of a pitcher/batter as the game progresses, the current number of balls/strikes/outs, and the types of pitches thrown by a pitcher in a specific inning can all affect the strategy and performance of both pitchers and batters. Not all of these features were sufficiently supported in the data we sourced [4].

Given our new understanding of both our data and baseball pitching strategy, we decided to shift our model to determining the type of a given pitch based on its measured characteristics. This still accomplishes the original goal of providing resources to help train both pitchers and batters. 

The analysis of pitching trajectories is a centerpiece of baseball strategy in many professional baseball leagues, including Major League Baseball (MLB) and the Korean Baseball Organization (KBO) [3]. It is “critically important” for teams to be able to determine the type of pitch based on its trajectory and other characteristics [6] so that a pitcher might examine and thus improve their pitching. Yet this information must be determined manually by specialists [6] in a time-consuming process. Our project attempts to use various machine learning algorithms to categorize a pitch based on its physical characteristics, including speed, rotation, and movement.

**Motivation:** Understanding and predicting pitch outcomes can significantly enhance training methods and strategies in baseball. By efficiently and accurately determining the type of a certain pitch based on its characteristics, teams are better able to compare prospective pitchers, pitchers are better able to analyze and improve their own performance, and even opposing batters are better able to prepare for games. This can improve overall performance and lead to more exciting games.

## Methods

To predict the likelihood of a strike using pitch data from MLB’s Statcast Search, in our first model we applied the following comprehensive data preprocessing techniques and machine learning algorithms.

### Data Preprocessing Methods

**Feature Scaling:** `StandardScaler` from scikit-learn. Pitch data includes various features like speed, movement, spin rate, etc., that are on different scales. The ranges of all these features should be standardized so that each feature contributes approximately proportionately to the final outcome. This is why we standardized these features as part of the data preprocessing portion of our model.

**Encoding Categorical Features:** `OneHotEncoder` from scikit-learn. Our pitch data includes categorical features like pitch outcome (StrikeCalled, StrikeSwinging, Ball, etc.) or pitcher handedness (left-handed or right-handed). To feed these to our machine learning algorithm, we first transformed them into numerical features using a OneHotEncoder.

**Other:**
On top of these methods, we did some initial analysis to remove certain features that are clearly not relevant for our model. Firstly, we discarded any data points that had null values (including data points for which the pitch type was “other” or “undefined”). Additionally, since we are focusing on the characteristics of each individual pitch and not on any specific players/game, features such as Pitcher_ID, Batter_ID, Game_Date, and Inning are irrelevant to our model. We also excluded any features related to the number of balls, outs, and strikes already thrown in a given inning.

### Machine Learning Algorithms

**Logistic Regression (Supervised Learning):** `LogisticRegression` from scikit-learn. Logistic Regression is a linear model that works by taking the input as independent variables and producing a probability value for each output (which all together sum to 1). Logistic Regression is straightforward and can effectively handle the multiple features of our data. Additionally, it can also tell us which features are the most important in predicting the pitch type, allowing for simple and straightforward analysis.

## Results and Discussion

### Quantitative Metrics/Visualizations

#### Confusion Matrix 
![](/images/confusion_mtx.png)

The diagonal elements represent the number of correct predictions for each pitch type, while the off-diagonal elements indicate the misclassifications. The confusion matrix indicates that the model performs well in predicting certain pitch types but may have difficulties with others. For example, while the other pitch types have a predicted and actual accuracy of roughly 80%, the cutter pitch type performs worse. With 228 predicted actual matching out of 439 pitches, its accuracy is roughly 52%.

The cutter pitches might have performed worse due to their similarity to fastballs and imbalanced data representation (fastballs represent about 13,000 out of our 24,000 datapoints). A cutter ball is thrown like a fastball but with both fingers placed together towards one side of the baseball to produce a cutting movement when thrown. Because there are more fastball data and due to its similarity to fastballs, the model might have a harder time distinguishing the two.

#### ROC curve
![](/images/multi_roc.png)

The ROC curve and AUC scores for each pitch type provide a measure of the model's ability to distinguish between different classes. The ROC curve plots the true positive rate against the false positive rate for each pitch type. As the curve moves towards the top left, it indicates that the model is correctly identifying a higher proportion of true positives. This means the model is effectively recognizing positive instances. The curves for the five classes are far to the top left and thus more accurate. More quantitatively, we can see that their area under the curve (AUC) is close to one. The AUC represents the probability that the classifier will rank a randomly chosen positive instance higher than a randomly chosen negative instance.

### Analysis of Logistic Regression Model

#### Feature coefficients
![](/images/log_coef.png)

The logistic regression model achieved an accuracy of approximately 88.52% on the test set. This indicates that the model performs relatively well in classifying pitch types based on the given features. The feature importance analysis reveals that `release_speed`, `z_movement`, and `release_pos_x` are among the most significant predictors.

Release speed having a high negative coefficient indicates that as the release speed increases, the likelihood of a pitch being classified into certain types decreases. This suggests that faster pitches are less likely to be classified into other types and more likely to be classified as fastballs.

Z Movement having a high positive coefficient indicates that as vertical movement increases, the likelihood of a pitch being classified into certain types increase.
For each increase in `z_movement`, the odd of the pitch being classfied as a curveball or another pitch with more vertical movement increases by 1.309

An increase in `release_pos_x` would increasese the odd of the pitch being classified as a pitch with more horizontal releaes position like sliders.


**Strengths:**
The model provides clear interpretability through feature coefficients, allowing us to understand the importance of each feature. The high accuracy and AUC scores indicate that the model can effectively classify pitch types.

**Weaknesses:**
Certain pitch types may still be misclassified, as indicated by the off-diagonal elements in the confusion matrix. The model might be overfitting to specific features, as seen in the relatively high coefficients for some features. Curveballs also seen to be overshadowed and misclassified as fastballs as mentioned earlier.

### Next Steps

We can try to optimize our model some more. We can conduct grid search or random search to optimize hyperparameters for the logistic regression model. We also believe more features might be beneficial. We can try some feature engineering techniques to increase features for optimization

Since the confusion matrix informed us of the reduced performance for cutter balls, and since they are very similar to fastballs, we can categorize the two as fastballs to simplify the model and potentially have higher accuracy

We can also implement additional models such as Random Forest Classifier and Support Vector Machine to capture complex feature interactions. We can compare the results of the Random Forest Classifier and logistic regression.

## Gantt Chart
![](/images/midterm_gantt.png)

## Contributions
![](/images/midterm_contri.png)


## References

1. C. Gough, “MLB league revenue 2024,” Statista. [Online]. Available: [https://www.statista.com/statistics/193466/total-league-revenue-of-the-mlb-since-2005/](https://www.statista.com/statistics/193466/total-league-revenue-of-the-mlb-since-2005/)
2. “US Sports Betting Revenue Tracker - how much revenue is each state generating in 2023?,” Oddspedia. [Online]. Available: [https://oddspedia.com/us/betting/sports-betting-revenue](https://oddspedia.com/us/betting/sports-betting-revenue)
3. H. Lee, J. Kim, J. Kim, J. Yu, and W. -Y. Kim, "Start-End Time Detection in Baseball Videos for Automatic Pitching Trajectory Analysis," 2019 International Conference on Electronics, Information, and Communication (ICEIC), Auckland, New Zealand, 2019, pp. 1-4, doi: 10.23919/ELINFOCOM.2019.8706498.
4. “Statcast Search,” baseballsavant.mlb.com. [Online]. Available: [https://baseballsavant.mlb.com/statcast_search](https://baseballsavant.mlb.com/statcast_search)
5. H. Lee, J. Kim, J. Kim and W. -Y. Kim, "A Method of Measuring Baseball Position at the Strike Zone," 2020 International Conference on Electronics, Information, and Communication (ICEIC), Barcelona, Spain, 2020, pp. 1-3, doi: 10.1109/ICEIC49074.2020.9051039.
6. J. Schuh and L. Kong, "Classifying Pitch Types in Baseball Using Machine Learning Algorithms," 2023 IEEE Asia-Pacific Conference on Computer Science and Data Engineering (CSDE), Nadi, Fiji, 2023, pp. 1-6, doi: 10.1109/CSDE59766.2023.10487702.
