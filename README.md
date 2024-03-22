# League Of Legends Victory Prediction Model
Project for DSC80 at UCSD 
Names: Jimmy Huang 

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

I performed a permutation test with a significance level of 0.05. The test statistic that I used was the Kolmogorov-Smirnov statistic. The observed K-S statistic was 0.007. The p-value was 0.5. The missingness of the `'firstbaron'` does not depend on the `'barons'` column. 

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

I performed a permutation test with a significance level of 0.05. THe test statistic that I used was the total-variation distance. The observed TVD was 0.99. The p-value was 0. THe missingness of the `'firstbaron'` column depends on the `'league'` column. 









