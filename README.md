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

In refining the prediction model, several features were introduced to enhance its performance in estimating the KDA of ADC players in professional League of Legends matches. Some of these features were used to engineer new features for further analysis.

#### Additional Features



In addition to the original columns of `damagetochampions`, `damagetakenperminute`, `result`, `champion`, new features added include:

- `gamelength` (duration of the game): While this feature exhibits a low correlation with KDA, its purpose is to serve as a tool for transforming `damagetochampions` into damage dealt to champions per minute. Since `damagetochampions` depends on the game's duration, introducing a time variable enables a more meaningful analysis.
- `allysupportchampion` (champion supporting allies): The support champion interacts most frequently with the ADC player throughout laning phase and the rest of the game. Consequently, the choice of a support champion that synergizes effectively with the ADC champion is important in ensuring optimal performance from the ADC.
- `cpkm` (combined kills per minute for all players in the game): This metric serves as an indicator of the game's focus on kills. In instances where a game is notably kill-oriented, an ADC player's KDA will likely tend to be higher more frequently than lower. This expectation arises from the role of ADC players, who are generally tasked with minimizing deaths while actively contributing to kills in the game. 
- `earnedgoldshare` (percentage of team gold earned by the ADC player): A higher value in this metric suggests that the ADC player likely received a substantial portion of the team's gold, possibly through team funneling. Consequently, a high gold share indicates that the player is better positioned to contribute significantly to securing kills.
- `damageshare` (percentage of team damage attributed to the ADC player): A higher value in this metric implies that the ADC player contributed a larger share of the total damage in the game compared to their teammates. This suggests a higher likelihood of the ADC player securing kills, given their substantial impact on the team's damage output.
- `oppchampion` (opponent ADC player's champion): The opponent champion that most frequently interacts with the ADC player. Consequently, the effectiveness of the ADC player may be influenced by the choice of the opposing ADC champion, particularly in terms of whether the opponent's champion provides a counter or advantage against the ADC player's chosen champion.
- `teamkills` (total kills by teammates): If a team earned a high number of kills, then a high kill count can be attributed to the ADC player.
- `teamdeaths` (total deaths of teammates): If the team experiences a high number of deaths, the ADC player is likely to have a proportionate increase in their own death count.

#### Relationship between various variables and KDA

All variables chosen to predict this model are in some way correlated to the response variable. Here are some features that seem to strongly influence KDA:

<iframe src="assets/team_deaths.html" width=600 height=450 frameBorder=0></iframe>
<iframe src="assets/damage_dealt.html" width=600 height=450 frameBorder=0></iframe>
<iframe src="assets/results_kda.html" width=600 height=450 frameBorder=0></iframe>

#### Encoding process
1. 'FunctionTransformer': The feature `allysupportchampion` underwent a transformation to `supavggoldshare`, or the average gold share of the champion in the support role within a team across all matches. A high average gold share for a support champion implies that less gold is allocated to the ADC player, thereby reducing their likelihood of contributing to kills.
2. 'FunctionTransformer': The features `gamelength` and `damagetochampions` were merged to create `damagetochampionsperminute` with the goal of enhancing its effectiveness in predicting KDA. This transformation is grounded in the recognition that `damagetochampions` is influenced by game duration. By introducing `damagetochampionsperminute`, the model is not impacted by varying game durations.
3. 'OneHotTransformer': The feature `oppchampion`, along with `champion`, is converted from categorical data into a quantitative format more suitable for regression.

#### Hyperparameters tested 
Three hyperparameters were specified to optimize the model selection: alpha value, and maximum number of iterations (when applicable). First, three model types, `LinearRegression`, `Lasso`, and `Ridge` were considered. Given that a `LinearRegression` model does not possess inherent hyperparameters, it was fitted directly using the newly defined features. For the `Lasso` and `Ridge` models, alpha values of 0.05, 1.0, 10.0, and 50.0, along with maximum iterations of 100, 150, 200, and 250, were employed for tuning. Subsequently, individual `GridSearchCV` instances were fitted to `Pipeline` configurations containing each `Lasso` and `Ridge` regressor. The resulting models, utilizing the optimal parameters identified by their respective `GridSearchCV` searches, achieved RMSE values of approximately 3.09 and 3.12, respectively.
Since a `LinearRegression` model does not have hyperparameters in itself, it was simply fit using the new parameters. For the `Lasso` and `Ridge` models, alpha values of 0.05, 1.0, 10.0, and 50.0 and maximum iterations of 100, 150, 200, and 250 were used for tuning. Then a `GridSearchCV` was individually fit to a `Pipeline` containing each `Lasso` and `Ridge` regressor. These models, using the best parameters provided by their respective `GridSearchCV` searcher, yielded a RSME of approximately 3.09 and 3.12 respectively. 

#### Summary
The `LinearRegression` model yielded the lowest RSME and therefore was chosen as the final prediction model. The selected regression model achieved an RMSE of approximately 3.03. Although this marks an improvement, the reduction in RMSE was not substantial, indicating that there is room for further adjustments and refinements in various aspects of the model.

## Fairness Analysis

Clearly state your choice of Group X and Group Y, your evaluation metric, your null and alternative hypotheses, your choice of test statistic and significance level, the resulting p
-value, and your conclusion.
#### Groups

To evaluate the fairness of the model, two groups were defined: 
- Matches in which the player played a champion with a positive winrate
- Matches in which the player played a champion with a negative winrate

#### Hypotheses & Test Statistic
- Null hypothesis: The model is fair. There is no difference in the RMSE of a player's KDA when playing on a champion with a positive win rate and playing on a champion with a negative win rate.
- Alternative hypothesis: The model is unfair. The RMSE of a player's KDA when playing on a champion with a positive win rate is higher than the RMSE when playing on a champion with a negative win rate.
- Test statistic: RMSE of a player's KDA when playing on a champion with a positive win rate - RMSE of a player's KDA when playing on a champion with a negative win rate

<iframe src="assets/permutation_test.html" width=700 height=450 frameBorder=0></iframe>

- Significance level: 0.01
- Test statistic: 0.192
- p-value: 0.0028

#### Conclusion
We reject the null hypothesis, and conclude that the model is unfair. The difference between the RNSE of RMSE of a player's KDA when playing on a champion with a positive win rate and RMSE of a player's KDA when playing on a champion with a negative win rate is significant. The observed significant difference provides evidence of systematic biases in the model's predictions. This emphasizes the need for further investigation and potential adjustments to rectify the fairness issues inherent in the model.
