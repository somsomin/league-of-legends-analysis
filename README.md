# Roaming or Assisting Bot? What is the Impact of Support Leaving Bot Lane Pre-15 on Team's Likelihood of Winning?

by Somin sPark(sompark@umich.edu)

---

## Introduction

This data science project explores 2022 League of Legends match data, focusing on the impact of leaving bot lane pre-15 on Attack Damage Carries (ADCs) success.

The dataset used in this project is from Oracle’s Elixir, a trusted repository of professional League of Legends match statistics. This dataset contains play-by-play and aggregated statistics for matches from 2022 competitive season with a total of 150,588 rows and 163 columns of data. There are 12,549 unique matches in the dataset and each match has 10 players, with 5 players on each team. 

In League of Legends, identifying the best timing to decide when and how to roam the map is very important. One of the most pivotal strategic decisions for ADCs is whether to remain in the bot lane to farm or leave the lane early to support objectives and skirmishes elsewhere. For this reason, this project investigates the impact of early roaming behavior by analyzing how leaving the bot lane before 15 minutes affects ADC performance and team success.

The research question for this project is the following:

**How often do ADCs leave their lane early (pre-15 minutes), and how is that correlated with win rate?**

This question will measure:
1) The frequency and context of early roaming by ADCs
2) Whether staying in the lane leads to higher personal or team success
3) Whether the win rate increases or decreases depending on early bot lane presence.


Here are the columns that I will consider in this project:
---

## Data Cleaning

<iframe src="assets/10-80-enrollment.html" width=800 height=600 frameBorder=0></iframe>

## Data Cleaning
To prepare the dataset ready for analysis, there were few steps I took to clean things up and focus on parts that are relevant to this project. 

First, I only kept the rows that are complete, using datacompleteness column that marks whether each row is fully recorded. I created a new dataframe to filter the data that includes the rows that are marked as complete to get rid of any rows that had missing information. 

Second, since I'm interested in how behavior of support champion affects the team's success in the game, and behavior of support is heavily related to the behavior of bot champions, I filtered the dataset once more so that we only include information of support champion, bottion champion players in the game and the overall information about the team's performance. This was done by only including rows that has 'sup', 'bot', and 'team' in the 'position' row. 

Third, I only kept the relevant columns. I selected relevant columns that contains information relevant to the question, and created a separate smaller dataframe that only contain those information. 

---

## Assessment of Missingness


---

## Hypothesis Testing


---