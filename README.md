# Soccer Match Result Prediction

This project explored the 'European Soccer Dataset' from Kaggle, which includes lineups and results of +25,000 matches in European leagues, from Seasons 2008 to 2016, as well as team attributes and player attributes. The project also built and compared two models with different feature engineering methods.

Data Source: [Kaggle-European Soccer Dataset](https://www.kaggle.com/datasets/hugomathien/soccer)

### 01_Data_Overview

The raw data source contains seven tables:
- matches: 25979 rows containing match date,  home team, away team, starting coordinates of all players, betting info, etc.
- countries: id for 11 countries
- leagues: id for 5 leagues
- teams: id, team names for 300 teams
- team_detail: 1458 rows containing team id, date, and attributes such as buildUpPlaySpeed, 	buildUpPlayDribbling, buildUpPlayPassing, etc.
- player: 11060 rows containing player id, player name, birthday, height, weight
- player_detail: 183978 rows containing player id, date, and attributes such as overall_rating, potential, preferred_foot, crossing, finishing, etc.
  
### 02_Player_Detail_Transformation

I simplified the 38 attributes in 'player_detail' table into six categories: 'passing', 'shooting', 'defence', 'goalkeeping', 'offence_misc', 'movement'.
Plotting the correlation heatmap on preliminary grouping indicates whether in-group conflict exists. Some attributes were taken out of the group to avoid canceling out each other.

The final attribute grouping:
    'passing':['crossing', 'short_passing', 'long_passing'],
    'shooting':['finishing','free_kick_accuracy','shot_power','long_shots'],
    'defence':['interceptions','marking','standing_tackle','sliding_tackle'],
    'goalkeeping':['gk_diving', 'gk_handling', 'gk_positioning','gk_reflexes'],
    'offence_misc':['ball_control', 'positioning', 'vision'],
    'movement':['dribbling','acceleration','sprint_speed']

f





The data source has noted the XY coordinates of all 22 players in the starting lineup. To reflect the player stats based on different positions, I took an arbitrary move by dividing the field into positions as shown below:

   ![formation](https://github.com/lorihe/Springboard-Capstone2---European-Soccer-Data/blob/main/imgs/Positions.jpg?raw=true)

This division labeled players as MF(midfielder), ST(striker), W(winger), SB(side back), CB(center back), GK(goalkeeper). In the distribution (see below) MF has the highest count.

Graphs below are three samples of different starting formations, with player positions labeled.

- Plain stats dataset
I selected six attributes 'dribbling', 'sprint_speed', 'passing', 'shooting', 'defense', 'goalkeeping' from the player attribute dataset for this research and assign them to each position. This dataset was prepared for the first model.

- Plain stats dataset EDA
The correlation heatmap below shows that:
1. there are constant high correlations between 'passing' and 'dribbling' of one position. The decision is to remove 'dribbling' for all positions.
2. there's a high correlation between 'shooting_MF' and 'passing_MF'. The decision is to remove 'shooting_MF'.
3. there are correlations between different positions which results from unclear reasons. The decision is to ignore it for now.







