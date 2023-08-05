# Soccer Match Result Prediction

This project aims at exploring the 'European Soccer Dataset' from Kaggle, which includes lineups and results of +25,000 matches in European leagues, from Seasons 2008 to 2016, as well as team attributes and player attributes. The project built and compared two models with different feature engineering methods.

Data Sourece: [Data](https://www.kaggle.com/datasets/hugomathien/soccer)

1. Data Transformation

The data source has noted the XY coordinates of all 22 players in the starting lineup. To reflect the player stats based on different positions, I took an arbitrary move by dividing the field into positions as shown below:

   [formation](https://github.com/lorihe/Springboard-Capstone2---European-Soccer-Data/blob/main/imgs/Positions.jpg?raw=true)

This division labeled players as MF(midfielder), ST(striker), W(winger), SB(side back), CB(center back), GK(goalkeeper). In the distribution (see below) MF has the highest count.

Graphs below are three samples of different starting formations, with player positions labeled.

- Plain stats dataset
I selected six attributes 'dribbling', 'sprint_speed', 'passing', 'shooting', 'defense', 'goalkeeping' from the player attribute dataset for this research and assign them to each position. This dataset was prepared for the first model.

- Plain stats dataset EDA
The correlation heatmap below shows that:
1. there are constant high correlations between 'passing' and 'dribbling' of one position. The decision is to remove 'dribbling' for all positions.
2. there's a high correlation between 'shooting_MF' and 'passing_MF'. The decision is to remove 'shooting_MF'.
3. there are correlations between different positions which results from unclear reasons. The decision is to ignore it for now.







