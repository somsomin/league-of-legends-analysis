# Roaming or Assisting Bot? What is the Impact of Support Leaving Bot Lane Pre-15 on Team's Likelihood of Winning?

by Somin Park(sompark@umich.edu)

---

## Introduction

In League of Legends, players take different roles with various strategic responsibilities. Therefore, identifying the best timing to decide when and how to roam the map is very important. One of the most pivotal strategic decisions for support players is whether to remain in the bot lane to assist bot player (Attack Damage Carry ADC champions) or leave the lane early to support objectives and skirmishes elsewhere (which if often called roaming). For this reason, this project investigates the impact of early roaming behavior of support players by analyzing whether support players roaming in the first 15 minutes of the game affects team success.

This data science project explores 2022 League of Legends match data as its dataset.

The dataset used in this project is from Oracle’s Elixir, a trusted repository of professional League of Legends match statistics. This dataset contains play-by-play and aggregated statistics for matches from 2022 competitive season with a total of 150,588 rows and 163 columns of data. There are 12,549 unique matches in the dataset and each match has 10 players, with 5 players on each team. 

The research question for this project is the following:

**Does early-game roaming by support players correlate with winning the game?**

This question is important because roaming is often considered "high-risk, high-reward" strategy for support players in the game. Understanding whether roaming in early phase of the game contributes to game sucess will give meaningful insights for players and professional E-Sport coaches to better evaluate support player performance and strategies in the game. 


Here are the columns that I will consider in this project:

| Column Name         | Description                                                                 |
|---------------------|------------------------------------------------------------------------------|
| gameid              | Game Identification                                                         |
| playername          | Name of the player (for tracking behavior per ADC)                          |
| teamname            | Team identifier (used for aggregating results)                              |
| position            | Position - ADC, Support, etc.                                                |
| result              | Binary match result: 1 = win, 0 = loss                                       |
| cspm                | CS per minute; proxy for laning priority and farming behavior                |
| assistsat10         | Number of assists by 10 minutes; proxy for early map presence or roaming     |
| assistsat15         | Number of assists by 15 minutes; proxy for early map presence or roaming     |
| killsat10           | Number of kills by 10 minutes; may also indicate proactive early-game impact |
| killsat15           | Number of kills by 15 minutes; may also indicate proactive early-game impact |
| deathsat10          | Number of deaths by 10 minutes; may be useful to assess riskiness of roaming |
| deathsat15          | Number of deaths by 15 minutes; may be useful to assess riskiness of roaming |
| goldat10            | Total gold at 10 minutes; proxy for early economic success                   |
| goldat15            | Total gold at 15 minutes; proxy for early economic success                   |
| xpat10              | Experience points at 10 minutes; proxy for lane presence and fight participation |
| xpat15              | Experience points at 15 minutes; proxy for lane presence and fight participation |
| csat10              | Total creep score at 10 minutes; another early laning metric                 |
| csat15              | Total creep score at 15 minutes; another early laning metric                 |
| earned gpm          | Earned gold per minute; indicates map activity beyond just farming           |
| dpm                 | Damage per minute; may correlate with combat involvement (i.e., roaming value)|
| datacompleteness    | Whether or not the data is complete in this row                              |

---

## Data Cleaning and Exploratory Data Analysis

### Data Cleaning
To prepare the dataset ready for analysis, there were few steps I took to clean things up and focus on parts that are relevant to this project. 

First, I only kept the rows that are complete, using 'datacompleteness' column that marks whether each row is fully recorded. I created a new dataframe to filter the data that includes the rows that are marked as complete to get rid of any rows that had missing information. 

Second, since I'm interested in how behavior of support champion affects the team's success in the game, and behavior of support is heavily related to the behavior of bot champions, I filtered the dataset once more so that we only include information of support champion, bottion champion players in the game and the overall information about the team's performance. This was done by only including rows that has 'sup', 'bot', and 'team' in the 'position' row. 

Third, I only kept the relevant columns. I selected relevant columns that contains information relevant to the question, and created a separate smaller dataframe that only contain those information. 

I additionally cleared out `champion` and `playername` columns for team-level rows to avoid using them by accident.
I also cleaned key numerical columns by converting every value in each numeric columns to a number.

### Univariate Analysis

**1. `assistsat15`**


 <iframe
 src="assets/boxplot1.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>

Boxplot shows median around 2–3 assists and long upper tail up to 15+. A small but significant group of supports is highly involved early. This could be used to flag roamers who leave lane to help mid/jungle.

**2. `csat15`**

 <iframe
 src="assets/boxplot2.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>

Boxplot shows peak around 20–30 CS, and some rare cases exceeding 100. Most supports have low CS, as expected. A spike around 20–30 may indicate shared farming behavior.


### Bivariate Analysis

To perform Bivariate analysis, I first explored the relationship between Creep Score at 15 Minutes and Assists at 15 Minutes for Support Players and created scatter plot.

 <iframe
 src="assets/BA_Scatterplot1.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>

This plot clearly reveals two behavioral clusters:

- Low CS, High Assists → likely roaming supports, contributing to early skirmishes and map impact.

- High CS, Low Assists → likely lane-staying supports, focusing more on farming or staying tethered to ADC.

The gap between 40–60 CS suggests that players rarely fall in-between these styles, reinforcing the presence of distinct support playstyles.



Next, I explored the relationship between Match Result and Assists at 15 Minutes for Support Players and created box plot.


 <iframe
 src="assets/BA_Scatterplot2.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>

According to this plot, winning supports (`result` = 1) have a higher median number of assists at 15 minutes than losing supports.

There are also more high-outlier assists in the win group — suggesting that early map impact (via assists) correlates positively with team success.


### Interesting Aggregates

To better understand how support player's early roaming behavior contributes to match result, I explored assist counts (`assistsat15`) and creep score at 15 minutes (`csat15`) and how these correlate with win rate. I chose these two variables because these stats reflect diffrent aspects of support player's gameplay, since assists capture map impact while CS captures lane-focused playstyle. 

To do so, I binned support players into categories based on their number of assists and CS at 15 minutes and created pivot table to show the average win rate for each combination and CS bin. 

| Assist Bin \ CS Bin | 0-10 | 11-30 | 31-60 | 61+  |
|---------------------|------|-------|-------|------|
| 0-1                 | 0.402 | 0.397 | 0.410 | 0.476 |
| 2-3                 | 0.528 | 0.519 | 0.544 | 0.547 |
| 4-6                 | 0.687 | 0.650 | 0.722 | 0.750 |
| 7+                  | 0.853 | 0.843 | 0.667 | 1.000 |


Some of the insights from this table were:

1) Higher assist counts correlate strongly with higher win rates, regardless of CS.

2) Low-CS supports (0–10) with high assists perform best-> This suggests that early roaming and teamfight participation is more valuable than farming.


This supports the hypothesis: "Leaving bot lane early to roam and assist teammates is positively correlated with team success for support champions."


---

## Framing a Prediction Problem

Prediction Probelm: 

**"Can we predict whether a team will win a match based on the early-game behavior of their support player, particularly whether the support stays in bot lane to assist the ADC or roams to impact other areas of the map?"**

This is a binary classification problem since

The response variable is `result` (1 = win, 0 = loss) which is categorical. I chose it because this is clearly the variable that determines team's success and performance in the game, and it also matches the structure of our research question of whether support early roaming behavior contributes to winning. 

I'm using a predictive model since the goal is to predict match outcome based on early-game features that would be known by minute 15, such as `csat15`, `assistsat15`, etc. Since theese are realistically knowable at prediction time, it is a valid forward-looking prediction task. 

I will evaluate our model using F1-score rather than simple accuracy. This is because if our classes are slightly imbalanced (e.g., 55% wins, 45% losses), accuracy may be misleading. F1-score balances precision and recall, ensuring we care equally about predicting both wins and losses accurately.


---

## Baseline Model

In my baseline model, I aim to predict whether a team will win a match based solely on the early-game behavior of its support player. 

Specifically, I use two quantitative features: 

`assistsat15`, which captures early map involvement and skirmish participation, and 

`csat15`, which reflects how much the support focused on farming in lane. 

I use a logistic regression classifier trained on these two features. I did not include any ordinal or categorical (nominal) features in this model, so no encoding was necessary. I used a SimpleImputer to fill in any missing values in the numeric columns using the mean of each feature. All preprocessing and modeling steps were wrapped in a single sklearn.Pipeline to ensure reproducibility and clean data handling.

I split the data into training and testing sets using an 80/20 split, with a fixed random seed for consistency. I evaluated the model’s performance using precision, recall, and F1-score. 

Below is the classification report of the baseline model.

| Class         | Precision | Recall  | F1-Score | Support |
|---------------|-----------|---------|----------|---------|
| 0 (Loss)      | 0.5666    | 0.7137  | 0.6317   | 2103    |
| 1 (Win)       | 0.6270    | 0.4685  | 0.5363   | 2160    |
|   Accuracy    |           |         | 0.5895   | 4263    |
| Macro Avg     | 0.5968    | 0.5911  | 0.5840   | 4263    |
| Weighted Avg  | 0.5972    | 0.5895  | 0.5834   | 4263    |


Macro-average F1-score: 58.4%

The model performs slightly better than random guessing, and it shows that early-game behavior alone does have some predictive power for team outcomes. As a baseline model, it performs well with simple feature set and sets up a good initial reference point for future improvements. However, since the model is very simple using only two features and does not capture player behavior, I would not consider this model "good" for real predictive use. 

This model serves as a benchmark. In the final model, I plan to improve performance by adding engineered features.

---

## Final Model

### Engineering New Features

Here are some of the new features I added.

#### (1) Roaming Logic 1: `roaming_bystats`

We'll define roaming_bystats = 1 if: `csat15` ≤ 30 AND `assistsat15` ≥ 3
Otherwise, roaming_bystats = 0.

This logic was build because of the following:

1. `csat15` (Creep Score at 15 minutes)
Lower CS = less time spent farming, which often means: leaving the ADC to farm alone, moving toward mid lane or jungle, or prioritizing warding, vision, or roaming skirmishes

2. `assistsat15` (Assists at 15 minutes)
More assists = more teamfight or roam participation, because Supports typically only get assists if they are helping in skirmishes or dives. Therefore assists across lanes imply map activity beyond bot lane

This logic makes sense because

🔹 1. Grounded in in-game behavior: 
Lane-staying supports will naturally have more CS
Roamers will leave lane, give up CS, and try to impact the map through vision and fights

🔹 2. Thresholds are intuitive and supported by your earlier plots: 
Univariate analysis showed that: CS clusters tightly around 20–40 for most supports and Assist counts of 3+ were in the top 30–40%


#### (2) Roaming Logic 2: `roaming_teamlogic`

This logic compares the kills and assists of the support player with their bot lane teammate at 5, 10, and 15 minutes.

For each time point, it calculates: abs(kills_sup - kills_bot) + abs(assists_sup - assists_bot), then it averages those three differences into a `roam_distance` score.

If `roam_distance` > 3, we label the support as `roaming_teamlogic` = 1, meaning:

The support likely spent significant time away from the ADC, indicating roaming behavior.


This logic makes sense because

🔹 1. It uses relative behavior, not just absolute thresholds
Instead of saying "X assists means roaming," it compares support's actions to their ADC's, which adjusts for: game pace, teamfight distribution, and passive vs. aggressive teams

🔹 2. It compares map presence indirectly
If a support and ADC have very different kill/assist counts, it likely means that the support was present in fights the ADC was not (e.g., mid/jungle skirmishes).
This information can be used as a good proxy for “not laning together”, which will imply roaming behavior.

While I'm dealing with a filtered dataframe with only support players, game stats for the bot players that the selected support players are laning with is required to calculate the `roaming_teamlogic`. Therefore, before I build model and train, I extracted bot player stats from each support players' team using gameid and teamname and added them in the existing filtered dataframe so we can use the bot player information to determine roaming behavior usinng roaming logic 2.


#### (3) `is_roaming`

The final `is_roaming` feature defines whether a support player showed roaming behavior during the early phase of the game. A support is classified as having roamed (`is_roaming` = 1) if and only if both of the following criterias are satisfied:

`roaming_bystats`: True when the player has: 1) Low creep score at 15 minutes (`csat15` ≤ 30), indicating less time spent farming in lane and 2) High assist count at 15 minutes (`assistsat15` ≥ 3), suggesting active involvement in map-wide skirmishes

`roaming_teamlogic`: Measures support player’s divergence from their bot lane player's early-game stats. It compares the support’s and bot laner’s kills and assists at 10 and 15 minutes. A high difference (average > 3) implies that the support was likely involved in separate fights elsewhere on the map.

Only when both variables are true labels the player's behavior as roaming (`is_roaming` = 1). 

This method improves precision by combining individual behavior metrics with relative team context and team strategy.

### About the Final Model

For the final model, I used a combination of raw and engineered features to predict whether a team would win a match based on the early-game behavior of its support player. I included several quantitative performance metrics (`assistsat10`, `assistsat15`, `goldat10`, `goldat15`, `xpat10`, `xpat15`, `csat10`, and `csat15`) along with four engineered features: `roaming_bystats`, `roaming_teamlogic`, `roam_distance`, and `is_roaming` to caputre difference aspects of support player's behaivor in early stage of the game. 

* `roaming_bystats`: roaming logic based on individual player's game stats - flags whether a support had a low CS and high assist counts by 15 minutes
* `roam_distance`: continuous proxy for statistical divergence between support and ADC within the team - measures how different support's skirmish involvement was from their team's bot player using assist and kill data at multiple timestamps
* `roaming_teamlogic`: roaming logic based on team's map presence using `roam distance` - flags whether support player is likely to have roamed or not using `roam_distance` calculations
* `is_roaming`: composite feature capturing agreement between the two definitions (`roaming_bystats` and `roaming_teamlogic`)

I also included deltas and timestamps (e.g., `assistsat10` vs. `assistsat15`), to capture temporal progression, improving from using only snapshot stats from 15 minute.

These features were chosen because they are not just statistical proxies but they reflect actual roles of the player in the game and their decision-making patterns. Including these new features allows the model to better identify roaming behavior by differentiating playstyles that are more proactive and involve map-involved supports or more passive and lane-focused. Therefore, the final model better captures the relationship between the support player's early game behavior and final match outcomes more effectively than raw stats alone, like the base model. 

These improvements in the final model allowed me to capture the nuances of early map movement and playstyle variations.

To prepare the data for modeling, I used a pipeline that included a `StandardScaler` for most numeric features and a `QuantileTransformer` for `csat15`, which was skewed based on earlier visual analysis. I used `SimpleImputer` to handle any missing values. Since I chose a `GradientBoostingClassifier`, I wanted to leverage its ability to model complex nonlinear patterns while maintaining interpretability through feature importance.

For hyperparameter tuning, I used `GridSearchCV` with 5-fold cross-validation to evaluate combinations of `n_estimators` (number of boosting stages), `learning_rate`(step size), and `max_depth`(tree complexity). These parameters control bias-variance tradeoff and the learning pace of this model. I selected targeted grid of reasonable values to prevent overfitting while having computational efficiency. The best-performing combination was `n_estimators=100`, `learning_rate=0.1`, and `max_depth=3`


Below is the classification report of the final model.

| Class         | Precision | Recall  | F1-Score | Support |
|---------------|-----------|---------|----------|---------|
| 0 (Loss)      | 0.6006    | 0.6572  | 0.6276   | 2103    |
| 1 (Win)       | 0.6325    | 0.5745  | 0.6021   | 2160    |
|   Accuracy    |           |         | 0.6153   | 4263    |
| Macro Avg     | 0.6166    | 0.6158  | 0.6149   | 4263    |
| Weighted Avg  | 0.6168    | 0.6153  | 0.6147   | 4263    |

Macro-average F1-score: 58.4% ->> final model 61.5%

Looking at the F1 Score of the final model, it has fairly balanced F1 scores indicating that the model does not overly favor one outcome over the other. The Class1 F1 Score was also improved from the baseline model from 53.6% to 60.2%, showing that we improved the prediction of wins. 
The final model achieved an accuracy of 61.5% on the test set, improving upon the baseline model’s 58.9%, which used only two features: `assistsat15` and `csat15`. I believe the improvement is meaningful, since it is powered by newly engineered features that reflect a deeper understanding of the actual game mechanics and role-specific dynamics/strategy. 

Below is the confusion matrix for the final model. This shows that the model is not heavily skewed toward one class and a higher recall for Class 0, implying more True Negatives than True Positives.

 <iframe
 src="assets/confusion_matrix.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>

---