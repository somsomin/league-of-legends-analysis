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

<iframe src="assets/10-80-enrollment.html" width=800 height=600 frameBorder=0></iframe>

To prepare the dataset ready for analysis, there were few steps I took to clean things up and focus on parts that are relevant to this project. 

First, I only kept the rows that are complete, using datacompleteness column that marks whether each row is fully recorded. I created a new dataframe to filter the data that includes the rows that are marked as complete to get rid of any rows that had missing information. 

Second, since I'm interested in how behavior of support champion affects the team's success in the game, and behavior of support is heavily related to the behavior of bot champions, I filtered the dataset once more so that we only include information of support champion, bottion champion players in the game and the overall information about the team's performance. This was done by only including rows that has 'sup', 'bot', and 'team' in the 'position' row. 

Third, I only kept the relevant columns. I selected relevant columns that contains information relevant to the question, and created a separate smaller dataframe that only contain those information. 

---

## Framing a Prediction Problem


---

## Baseline Model


---

## Final Model


---