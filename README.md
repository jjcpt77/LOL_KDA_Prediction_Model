# Estimating the KDA of ADC Players through a Linear Regression Model

**Author:** Rebecca Wu

In this project, I used a comprehensive dataset on competitive League of Legends matches from 2022 to create a predictive linear regression model. The specific focus of this model is to estimate Kill-Death-Assist (KDA) ratios specifically tailored for Attack Damage Carry (ADC) players based on postgame data. This involves analyzing patterns and insights from the data to train the model, shedding light on the factors influencing players' performance in the professional gaming scene. An exploratory data analysis was performed on this dataset prior to the development of this model, and the analysis findings can be found [here](https://jjcpt77.github.io/competitive_lol_matches_analysis/).

## Framing the Problem

The predictive problem revolves around **estimating the KDA of ADC players in professional League of Legends matches**. In professional League of Legends, an ADC player typically refers to the player who plays the champion positioned in the bottom lane. They are often focused on dealing constant, sustained damage with basic attacks, and are crucial damage-dealers during team fights. KDA is a performance metric that combines the number of kills, deaths, and assists a player achieves in a game. High KDA indicates effective play, emphasizing both offensive contributions (kills) and defensive awareness (few deaths). In high-level competitive matches, teams are more coordinated and more prone to play around their carry players and allow them to get kills, assists, gold, etc. While there may be more than one carry in a team during a match, the ADC role most frequently commits to being a damage dealer. In this way, the KDA value is a specific metric that can quantify the performance of an ADC player and therefore was chosen as the response variable for the model. Since KDA is a continuous variable, and the aim is to predict a numeric outcome, a **linear regression model** was selected. 



In this predictive model, KDA is defined as `(kills + assists) / deaths`. This value is not explicitly given in the dataset, but is instead derived from the player `kills`, `deaths`, and `assists` columns of the dataset. 





## Baseline Model

For the baseline model, the columns incorporated include: 

## Final Model

The final model added on to the 

### Relationship between various variables and KDA

#### 

## Fairness Analysis
