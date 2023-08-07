# Soccer Match Feature Analysis and Result Prediction

This project explored the 'European Soccer Dataset' from Kaggle, which includes lineups and results of +25,000 matches in European leagues, from Seasons 2008 to 2016, as well as team attributes and player attributes. The project also built and compared two models with different feature engineering methods.

Data Source: [Kaggle-European Soccer Dataset](https://www.kaggle.com/datasets/hugomathien/soccer)



#### 01_Data_Overview  [Notebook](https://github.com/lorihe/Springboard-Capstone2---European-Soccer-Data/blob/main/Notebook/01_Data_Overview.ipynb)

The raw data source contains seven tables:
- matches: 25979 rows containing match date,  home team, away team, starting coordinates of all players, betting info, etc.
- countries: id for 11 countries
- leagues: id for 5 leagues
- teams: id, team names for 300 teams
- team_detail: 1458 rows containing team id, date, and attributes such as buildUpPlaySpeed, 	buildUpPlayDribbling, buildUpPlayPassing, etc.
- player: 11060 rows containing player id, player name, birthday, height, weight
- player_detail: 183978 rows containing player id, date, and attributes such as overall_rating, potential, preferred_foot, crossing, finishing, etc.


  
#### 02_Player_Detail_Transformation  [Notebook](https://github.com/lorihe/Springboard-Capstone2---European-Soccer-Data/blob/main/Notebook/02_Player_Detail_Transformation.ipynb)

I simplified the 38 attributes in 'player_detail' table into six categories: 'passing', 'shooting', 'defense', 'goalkeeping', 'offence_misc', 'movement'.
Plotting the correlation heatmap on preliminary grouping indicates whether in-group conflict exists. Some attributes were taken out of the group to avoid canceling out each other.

<div style="display: flex; align-items: flex-start;">
  <img src="https://github.com/lorihe/Springboard-Capstone2---European-Soccer-Data/blob/main/imgs/02-1.PNG?raw=true" alt="Image Description" width="250" height="230">
  <img src="https://github.com/lorihe/Springboard-Capstone2---European-Soccer-Data/blob/main/imgs/02-2.PNG?raw=true" alt="Image Description" width="285" height="232">
  <img src="https://github.com/lorihe/Springboard-Capstone2---European-Soccer-Data/blob/main/imgs/02-3.PNG?raw=true" alt="Image Description" width="280" height="230">
</div>
<div style="display: flex; align-items: flex-start;">
  <img src="https://github.com/lorihe/Springboard-Capstone2---European-Soccer-Data/blob/main/imgs/02-4.PNG?raw=true" alt="Image Description" width="250" height="230">
  <img src="https://github.com/lorihe/Springboard-Capstone2---European-Soccer-Data/blob/main/imgs/02-5.PNG?raw=true" alt="Image Description" width="280" height="230">
  <img src="https://github.com/lorihe/Springboard-Capstone2---European-Soccer-Data/blob/main/imgs/02-6.PNG?raw=true" alt="Image Description" width="280" height="230">
</div>

The final attribute grouping: 
> feature_group = { \
    'passing':['crossing', 'short_passing', 'long_passing'], \
    'shooting':['finishing','free_kick_accuracy','shot_power','long_shots'], \
    'defence':['interceptions','marking','standing_tackle','sliding_tackle'], \
    'goalkeeping':['gk_diving', 'gk_handling', 'gk_positioning','gk_reflexes'], \
    'offence_misc':['ball_control', 'positioning', 'vision'], \
    'movement':['dribbling','acceleration','sprint_speed'] }


#### 03_Match_Transformation  [Notebook](https://github.com/lorihe/Springboard-Capstone2---European-Soccer-Data/blob/main/Notebook/03_Match_Transformation.ipynb)

The data source has noted the XY coordinates of all 22 players in the starting lineup. To reflect the player stats based on different positions, I took an arbitrary move by dividing the field into positions as shown below:

  <img src="https://github.com/lorihe/Springboard-Capstone2---European-Soccer-Data/blob/main/imgs/Positions.jpg?raw=true" alt="Image Description" width="340" height="290">

Upon this, players are labeled as MF(midfielder), ST(striker), W(winger), SB(side back), CB(center back), GK(goalkeeper). In the distribution (see below) MF has the highest count.

  <img src="https://github.com/lorihe/Springboard-Capstone2---European-Soccer-Data/blob/main/imgs/03-1.PNG?raw=true" alt="Image Description" width="300" height="220">

Graphs below are three samples of different starting formations, with player positions labeled.
<div style="display: flex; align-items: flex-start;">
  <img src="https://github.com/lorihe/Springboard-Capstone2---European-Soccer-Data/blob/main/imgs/03-02.PNG?raw=true" alt="Image Description" width="250" height="230">
  <img src="https://github.com/lorihe/Springboard-Capstone2---European-Soccer-Data/blob/main/imgs/03-03.PNG?raw=true" alt="Image Description" width="255" height="230">
  <img src="https://github.com/lorihe/Springboard-Capstone2---European-Soccer-Data/blob/main/imgs/03-04.PNG?raw=true" alt="Image Description" width="250" height="230">
</div>

I merged the info of positions and attributes of each player in each game to get this **Plain stats dataset** as shown below:

- Plain stats dataset
I selected six attributes 'dribbling', 'sprint_speed', 'passing', 'shooting', 'defense', 'goalkeeping' from the player attribute dataset for this research and assign them to each position. This dataset was prepared for the first model.


#### 04_EDA_Plain_Stats  [Notebook](https://github.com/lorihe/Springboard-Capstone2---European-Soccer-Data/blob/main/04_EDA_Plain_Stats.ipynb)

The correlation heatmap below shows that:
1. there are constant high correlations between 'offence_misc',  'passing', and 'shooting' of MF, ST, and W, they should be merged.
2. there are constant high correlations between 'offence_misc' and  'passing' of SB, they should be merged.

All the attributes-position combos were plotted to show their relations with game results (partially shown below). Some plots were as expected, like high offence_misc midfielder and high defense center back both matching with more wins than losses, while striker's defense and centreback's offence_misc didn't show a strong relation with win or loss distribution. Some are not as easily expected, like the midfielder's defense not showing strong relation but the centreback's movement showing relation with more wins.
<div style="display: flex; align-items: flex-start;">
  <img src="https://github.com/lorihe/Springboard-Capstone2---European-Soccer-Data/blob/main/imgs/04-1.PNG?raw=true" alt="Image Description" width="250" height="170">
  <img src="https://github.com/lorihe/Springboard-Capstone2---European-Soccer-Data/blob/main/imgs/04-2.PNG?raw=true" alt="Image Description" width="250" height="170">
  <img src="https://github.com/lorihe/Springboard-Capstone2---European-Soccer-Data/blob/main/imgs/04-3.PNG?raw=true" alt="Image Description" width="250" height="170">
</div>
<div style="display: flex; align-items: flex-start;">
  <img src="https://github.com/lorihe/Springboard-Capstone2---European-Soccer-Data/blob/main/imgs/04-4.PNG?raw=true" alt="Image Description" width="250" height="170">
  <img src="https://github.com/lorihe/Springboard-Capstone2---European-Soccer-Data/blob/main/imgs/04-5.PNG?raw=true" alt="Image Description" width="250" height="170">
  <img src="https://github.com/lorihe/Springboard-Capstone2---European-Soccer-Data/blob/main/imgs/04-6.PNG?raw=true" alt="Image Description" width="250" height="170">
</div>


#### 05_EDA_Adv_Stats  [Notebook](https://github.com/lorihe/Springboard-Capstone2---European-Soccer-Data/blob/main/05_EDA_Adv_Stats.ipynb)

Here I made an attempt on showing paired relation between advantages by creating 'advantage' features. To do this, first I checked out the strength of each position by plotting below.
<div style="display: flex; align-items: flex-start;">
  <img src="https://github.com/lorihe/Springboard-Capstone2---European-Soccer-Data/blob/main/imgs/05-1.PNG?raw=true" alt="Image Description" width="260" height="200">
  <img src="https://github.com/lorihe/Springboard-Capstone2---European-Soccer-Data/blob/main/imgs/05-2.PNG?raw=true" alt="Image Description" width="260" height="200">
  <img src="https://github.com/lorihe/Springboard-Capstone2---European-Soccer-Data/blob/main/imgs/05-3.PNG?raw=true" alt="Image Description" width="260" height="200">
</div>
<div style="display: flex; align-items: flex-start;">
  <img src="https://github.com/lorihe/Springboard-Capstone2---European-Soccer-Data/blob/main/imgs/05-4.PNG?raw=true" alt="Image Description" width="260" height="200">
  <img src="https://github.com/lorihe/Springboard-Capstone2---European-Soccer-Data/blob/main/imgs/05-5.PNG?raw=true" alt="Image Description" width="260" height="200">
  <img src="https://github.com/lorihe/Springboard-Capstone2---European-Soccer-Data/blob/main/imgs/05-6.PNG?raw=true" alt="Image Description" width="260" height="200">
</div>


Combining the observation and some common sense, I created the paired system as below:
Note: dictionary structure{advantage feature: [numerator(s)], [denominator(s)], coefficient}*
>advantage_pair = { \
    'offence_box_adv':(['shooting_ST_home','shooting_W_home'],['defence_CB_away','defence_SB_away'],1),  \
    'offence_GK_adv':(['shooting_ST_home','shooting_W_home'],['goalkeeping_GK_away'],0.5), \
    'defence_box_adv':(['defence_CB_home','defence_SB_home'],['shooting_ST_away','shooting_W_away'],1), \
    'defence_GK_adv':(['goalkeeping_GK_home'],['shooting_ST_away','shooting_W_away'],2),  
    
>   'offence_pass_adv':(['passing_W_home', 'passing_MF_home', 'passing_SB_home'],['defence_CB_away','defence_SB_away'],2/3), \
    'offence_move_adv':(['movement_W_home'],['movement_SB_away'],1), \
    'defence_pass_adv':(['defence_CB_home','defence_SB_home'],['passing_W_away', 'passing_MF_away', 'passing_SB_away'],1.5), \
    'defence_move_adv':(['movement_SB_home'],['movement_W_away'],1),    

>   'offence_misc_adv':(['offence_misc_MF_home','offence_misc_ST_home','offence_misc_W_home'], \
>                       ['offence_misc_MF_away','offence_misc_ST_away','offence_misc_W_away'],1)}


Plotting advantage features showed that some features such as the ability of defenders against offensive passing have an obvious relation with wins and losses. Also, a large number of outliers were noticed. 
  <img src="https://github.com/lorihe/Springboard-Capstone2---European-Soccer-Data/blob/main/imgs/05-7.PNG?raw=true" alt="Image Description" width="750" height="410">
  
 Plotting below shows some advantage features have stronger relations with win or lose than the others.
  <img src="https://github.com/lorihe/Springboard-Capstone2---European-Soccer-Data/blob/main/imgs/05-8.PNG?raw=true" alt="Image Description" width="680" height="440">


#### 06_Modeling  [Notebook](https://github.com/lorihe/Springboard-Capstone2---European-Soccer-Data/blob/main/06_Modeling.ipynb)
Model performance was compared between: 1. a dummy classifier predicting based on dataset results ratio; 2. a model trained on plain stats data; 3. a model trained on advantage feature data.

**Dummy classifier**

  <img src="https://github.com/lorihe/Springboard-Capstone2---European-Soccer-Data/blob/main/imgs/06-1.PNG?raw=true" alt="Image Description" width="400" height="390">


**Plain stats model**

  <img src="https://github.com/lorihe/Springboard-Capstone2---European-Soccer-Data/blob/main/imgs/06-2.PNG?raw=true" alt="Image Description" width="400" height="170"> 
  <img src="https://github.com/lorihe/Springboard-Capstone2---European-Soccer-Data/blob/main/imgs/06-3.PNG?raw=true" alt="Image Description" width="200" height="200">


**Advantage feature model**

Logistic Regression 

  <img src="https://github.com/lorihe/Springboard-Capstone2---European-Soccer-Data/blob/main/imgs/06-4.PNG?raw=true" alt="Image Description" width="400" height="160"> 
  <img src="https://github.com/lorihe/Springboard-Capstone2---European-Soccer-Data/blob/main/imgs/06-5.PNG?raw=true" alt="Image Description" width="210" height="200">

#### Conclusion: 
- The models with plain stats and with advantage stats have similar performance, and both obviously better than the dummy classifier
- For the plain stats model, the most important features are: *defence_CB_home, passing_MF_away, passing_MF_home,
  defence_CB_away, shooting_ST_away, defence_SB_home, shooting_ST_home, defence_SB_away, passing_SB_away, passing_SB_home,  passing_W_home* \
  
  <img src="https://github.com/lorihe/Springboard-Capstone2---European-Soccer-Data/blob/main/imgs/06-6.PNG?raw=true" alt="Image Description" width="600" height="460">  
- For the advantage stats  model, the most important features are: *offence_misc_adv, offence_pass_adv, defence_pass_adv, offence_box_adv* 
 
  <img src="https://github.com/lorihe/Springboard-Capstone2---European-Soccer-Data/blob/main/imgs/06-7.PNG?raw=true" alt="Image Description" width="600" height="300"> 
