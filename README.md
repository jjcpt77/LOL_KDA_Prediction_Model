# Estimating the KDA of ADC Players through a Linear Regression Model

**Author:** Rebecca Wu

In this project, I used a comprehensive dataset on competitive League of Legends matches from 2022 to create a predictive linear regression model. The specific focus of this model is to estimate Kill-Death-Assist (KDA) ratios specifically tailored for Attack Damage Carry (ADC) players based on postgame data. This involves analyzing patterns and insights from the data to train the model, shedding light on the factors influencing players' performance in the professional gaming scene. 


An exploratory data analysis was performed on this dataset prior to the development of this model, and the analysis findings can be found [here](https://jjcpt77.github.io/competitive_lol_matches_analysis/).

## Framing the Problem

The predictive problem revolves around **estimating the KDA of ADC players in professional League of Legends matches**. In professional League of Legends, an ADC player typically assumes the role of the champion positioned in the bottom lane, specializing in dealing consistent, sustained damage through basic attacks. This position is crucial during team fights, where ADC players play a crucial role as primary damage-dealers. The KDA metric (defined as `(kills + assists) / deaths`), serves as a performance indicator of effective play, emphasizing both offensive contributions (many kills and assists) and defensive awareness (few deaths). In high-level competitive play, teams strategically coordinate around their carry players, facilitating opportunities for kills, assists, and gold accumulation. Consequently, KDA is a metric for quantifying the performance of ADC players, thus justifying its selection as the response variable for the model. Since KDA is a continuous variable, and the aim is to predict a numeric outcome, a **regression model** was selected. 


The metric chosen to evaluate the model is Root Mean Squared Error (RMSE). RMSE is preferred over other suitable metrics, such as Mean Absolute Error (MAE), because it penalizes larger errors more significantly. In the context of predicting KDA, accurately capturing the nuances and minimizing errors in estimation is crucial for a meaningful assessment of ADC performance. It is important to note that at the time of prediction, only the features available post-game will be known. Examining post-game data, such as damage share and game result, aids in understanding the effectiveness of different lane compositions and helps teams gauge the contribution of ADCs to a match overall.

## Baseline Model

The baseline model selected to predict KDA is a linear regression model, incorporating four features that specify the champion selected by the player and exhibit direct correlations with player kills, deaths, and/or assists

**Quantitative features**: 
- `damagetochampions`: The amount of damage inflicted on other champions during the entirety of the game
- `damagetakenperminute`: Damage sustained by the player per minute

**Nominal features**:
- `result`: The outcome of the game, represented by 1 for a win and 0 for a loss
- `champion`: The specific champion chosen by the player

This model yielded an RMSE of approximately 3.4194. 
The `champion` column was one-hot encoded to convert categorical data into a quantitative format that allows the model to interpret and include champion information into its predictions. The `result` column, while also nominal, is already presented in a numerical format. The columns `damagetochampions` and `damagetakenperminute` are discrete and continuous, respectively.


This model yielded a RMSE of approximately 3.42. The RMSE can be interpreted as a measure of the average deviation between the predicted KDA values and the actual KDA values, therefore a lower RMSE indicates better predictive accuracy. In the context of this model, an RMSE of around 3.42 suggests that, on average, the model's predictions deviate by approximately 3.42 units from the actual KDA values. In the context of KDA values, an RMSE of 3.42 represents a substantial percentage of the overall scale of the response variable, indicating a significant level of prediction error, and therefore cannot be considered "good".

## Final Model

#### Additional Features


#### Encoding process

#### Hyperparametrs and 

 

The final model did not massively decrease the RMSE, and therefore still contains many errors to go thorugh
The final model added on to the 

#### Relationship between various variables and KDA

All variables chosen to predict this model are in some way correlated to the response variable. Here are some features that seem to influence KDA:

<iframe src="assets/team_deaths.html" width=800 height=450 frameBorder=0></iframe>
<iframe src="assets/damage_dealt.html" width=800 height=450 frameBorder=0></iframe>
<iframe src="assets/results_kda.html" width=800 height=450 frameBorder=0></iframe>

## Fairness Analysis

<iframe src="assets/results_kda.html" width=1050 height=450 frameBorder=0></iframe>
