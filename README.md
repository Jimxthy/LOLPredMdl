# League Of Legends Victory Prediction Model
Project for DSC80 at UCSD 
Name: Jimmy Huang 

# Introduction
My dataset is all of the professional League of Legends games played in 2022. The original dataset has 12 rows for each game that was played, 10 rows for each player and 2 rows for each team summary. There are over 100 columns in the data frame, including columns about the pick and bans during the drafting phase, information about the total gold each player earned and the total gold, and others. Competetive League of Legends is really popular, and players have several favorite teams. It would be nice if we could predict whether our favorite teams would win. 


The columns that I am interested in are `'side'`, `'league'`, `'firstdragon'`, `'golddiffat15'`, `'xpdiffat15'`, `'csdiffat15'`, `'killsat15'`, `'teamkills'`, `'firstbaron'`, `'firstherald'`, `'heralds'`, `'elementaldrakes'`, `'barons'`, `'elders'`, and `'result'`. Here is a description for those columns: 

- `'side'`: The side that the team played on, red or blue
- `'league'`: What competitve league that this game was played
- `'firstdragon'`: Whether the team got the first dragon or not
- `'golddiffat15'`: The gold difference at 15 minutes
- `'xpdiffat15'`: The XP difference at 15 minutes
- `'csdiffat15'`: The difference in creep score at 15 minutes
- `'killsat15'`: The number of kills at 15 minutes
- `'teamkills'`: The total number of kills 
- `'firstbaron'`: Whether a team got the first baron
- `'firstherald'`: Whether a team for the first herald
- `'heralds'`: The number of heralds a team got, maximum is 2
- `'elementaldrakes'`: The number of elemental drakes (dragons) a team got, maximum is 4
- `'barons'`: The number of barons a team got
- `'elders'`: The number of elder dragons a team got
- `'result'`: Whether a team won (1) or lost (0)

# Data Cleaning and Exploratory Analysis

## Data Cleaning
First, I decided to keep only the relevant columns that were mentioned above. Secondly, I kept all of the rows that contained the team summaries after each game. This means that there are two rows per game, one row for each team that played in that one game. Next, I changed the values of `'firstdragon'`, `'firstbaron'`, and `'firstherald'` into booleans, which were originally just 1's and 0's. Lastly, I changed the `'side'` column into `'blueside'`, which just indicates whether a team was on the blue side of the map or red side of the map. The value is `True` if the team played on blue side, or `False` if they played on red side. 

Here is the head of my `Lol_clean` data frame:


|  blueside  |  league  |  firstdragon  |  golddiffat15  |  xpdiffat15  |  csdiffat15  |  killsat15  |  teamkills  |  firstbaron  |   firstherald  |  heralds  |  elementaldrakes  |  barons  |  elders  |  result  |
|------------|----------|---------------|----------------|--------------|--------------|-------------|-------------|--------------|---------------|-----------|-------------------|----------|----------|----------|
| True     | LCKC   | False       | 107.0        | -1617.0    | -23.0      | 5.0       | 9         | False      | True        | 2.0     | 1.0             | 0.0    | 0.0    | 0      |
| False    | LCKC   | True        | -107.0       | 1617.0     | 23.0       | 6.0       | 19        | False      | False       | 0.0     | 3.0             | 0.0    | 0.0    | 1      |
| True     | LCKC   | False       | -1763.0      | -906.0     | -22.0      | 1.0       | 3         | False      | True        | 1.0     | 1.0             | 0.0    | 0.0    | 0      |
| False    | LCKC   | True        | 1763.0       | 906.0      | 22.0       | 3.0       | 16        | True       | False       | 1.0     | 4.0             | 2.0    | 0.0    | 1      |
| True     | LPL    | NaN         | NaN          | NaN        | NaN        | NaN       | 13        | NaN        | NaN         | NaN     | NaN             | 1.0    | NaN    | 1      |


## Univariate Analysis
<iframe
  src="assets/lol_fig1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
This is the distribution of the number of dragons each team got each game played. Most teams were able to secure two dragons.

## Bivariate Analysis
<iframe
  src="assets/lol_fig4.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
This is the distribution of kills on blue side compared to red side. It looks like blue side is able to get more kills than red side. 

## Interesting Aggregates

| elementaldrakes | 0.0  | 1.0   | 2.0   | 3.0   | 4.0   |
|-----------------|------|-------|-------|-------|-------|
| blueside        |      |       |       |       |       |
| False           | 7.52 | 9.70  | 13.54 | 16.86 | 19.40 |
| True            | 8.40 | 11.62 | 15.69 | 17.55 | 19.37 |

This shows how blue side is able to get more kills on average compared to red team by the number of elemental drakes that they are able to secure. For example, when blue teams and red teams secure 2 dragons, blue side gets more kills on average compared to red team. 


# Assessment of Missingness

## NMAR Analysis
I think that a lot of columns could be considered NMAR because a lot of the missing data needs to be written in by someone that manages the games. One such column would be the `'elders'` column, because teams usually secure elder dragons after one team secures 4 dragons. Usually, this would take around 30-45 minutes. In order to make this column MAR, it would be nice to have some information about when elder dragons become available, this way the missingness can depend on that column. 

## Missing Dependency

I wanted to explore whether the missingness in the `'firstbaron` column depends on the `'barons'` column. 
<iframe
  src="assets/missfig1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<iframe
  src="assets/empks.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

I performed a permutation test with a significance level of 0.05 and 500 simulations. The test statistic that I used was the Kolmogorov-Smirnov statistic. The observed K-S statistic was 0.007. The p-value was 0.5. The missingness of the `'firstbaron'` does not depend on the `'barons'` column. 

I wanted to explore whether the issingness in the `'firstbaron'` column depends on the `'league'` column. 
<iframe
  src="assets/missfig2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<iframe
  src="assets/emptvd.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

I performed a permutation test with a significance level of 0.05 and 500 simulations. The test statistic that I used was the total-variation distance. The observed TVD was 0.99. The p-value was 0. The missingness of the `'firstbaron'` column depends on the `'league'` column. 

# Hypothesis Testing

The question that I tried to answer was whether teams on blue side won more frequently than red side. To do this, I used only the information in my data frame where `'blueside'` was equal to `True`. Then, I randomly simulated whether that team won or lost. I randomly assigned them `1` for winning or `0` for losing. 

**Null Hypothesis**: Blue side has the same chance to win or lose

**Alternate Hypothesis**: Blue side has a higher chance to win

The test statistic that I used was the difference in proportions between wins and losses. The level of significance that I used was 0.05. 

<iframe
  src="assets/hypothesistest.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The observed test statistic was 0.05, and the p-value was 0. This means we reject the null hypothesis in favor of that blue side wins more often than red side. Since we reject the null hypothesis, we believe that the games are favored towards blue side and that the distribution does not come from the population. 

# Framing a Prediction Problem

I want to build a model that is able to predict whether a team will win by the 20 minute mark. I will be performing binary classification because the response variable that I am trying to predict is whether a team would win or lose. I chose thise response variable because players mostly care about the result, and the result of the game is the most interesting. The metric that I will be using to evaluate my model will be accuracy because I care more about the correct predictions instead of the false negatives and false positives. The features that I will use are information that are made available by the first 20 minutes. Some examples include `'firstdragon'` because a team usually secures at least one dragon by the 20 minute mark. Additionally, for fitting my baseline and final models, I dropped all of the rows that contained `Nan`'s because it is hard to fill in these values since anything could happen in a game of League of Legends, and since I still have plenty of data still even with these dropped rows, I decided to just drop them. 

# Baseline Model

The estimator that I used was a `DecisionTreeClassifier()`. The features that I used in my model included `'blueside'`, `'firstdragon'`, `'golddiffat15'`, `'xpdiffat15'`, `'csdiffat15'`, `'killsat15'`, `'firstbaron'`, `'firstherald'`, and `'heralds'`.`'blueside'`, `'firstdragon'`, `'firstbaron'`, and `'firstherald'` are all nominal. The rest of the features are all quantitative. For my baseline model, I just simply fit a `DecisionTreeClassifier()` on my features, since all of them are either booleans, `1`'s and `0`'s, or numerical already. The training accuracy that I got was 100%, while the testing accuracy was 77%. This is not a good model because the testing accuracy was severely lower than my testing accuracy, indicating that the model was severely overfit to the training data and is not good at generalizing and predicting on unseen data. 

# Final Model

The first feature that I added was a binarized `'xpdiffat15'` with a threshold of 0. The reason that I binarized this column because a team was more likely to win if they were ahead in XP compared to the enemy team, and a team was more likely to win if they had more XP in the early game. Another feature that I added was a squared `'heralds'` column. As a result, all of the possible values in `'heralds'` were either `0`, `1`, or `4`. This is because teams had a higher chance to win compared to when a team only got 0 or 1 heralds. I think that that getting 2 heralds should count for more in the model, so I squared the values. The last feature that I added was a binarized `'golddiffat15'` with a threshold of 2000. This is because small gold leads are insignificant, and if a team had a large gold lead, such as more than 2000, by the 15 minute mark, they would have a higher chance to win compared to teams that were behind or only had a slight gold lead. 

Again, I used a `DecisionTreeClassifier()` for my prediction model. Compared to before when I did not specify which hyperparameters, the hyperparameters that ended up doing the best was `criterion = 'gini'`, `max_depth = 4`, and `min_samples_split = 325`. The way that I selected my hyperparameters was using `GridSearchCV()`, and setting the cross validation to 5 and selected the best parameters. Everytime new parameters appeared, if the parameter was an extreme, I would change the hyperparameters again and performed the same thing. Once the hyperparameters started to stagnate, that was when I decided that those parameters were the optimal parameters. The training accuracy of my final model went down to 84%, and my testing accuracy increased to 84% as well. This shows how my final model is more generalized and does better when predicting with unseen data since the testing accuracy was higher. 

# Fairness Analysis

I wanted to explore whether my final prediction model was able to guess fairly for teams that played on blue side or red side. The evaluation metric that I used was demographic parity and accuracy parity. My final model did not achieve demographic parity since my model predicted that blue side would win more times than red side. My model predicted that blue side would win approximately 53% of its games while it predicted that red side would win only 50% of its games. 

The accuracy for the blue side games was 84%, while the accuracy for the red side was 85%. To test whether this difference is significant or not, I performed a permutation test with the following hypotheses: 

**Null Hypothesis**: The accuracies for blue side games is the same as red side games. 

**Alternate Hypothesis**: The accuracies for red side is higher than blue side. 

I performed 500 simulations and set a significance level of 0.05. The test statistic that I used was blue accuracy - red accuracy, and the observed test statistic was -0.02. The resulting p-value was 0.066. Since the p-value is larger than 0.05, then we fail to reject the null hypothesis. This means that my model does achieve accuracy parity and that the difference in accuracies is indeed not significant. 
