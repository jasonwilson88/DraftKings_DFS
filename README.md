NFL DraftKings Projections

Introduction

In the last ten years, daily fantasy sports (DFS) have exploded in popularity. In traditional fantasy formats, each season begins with a draft where you, and the rest of your league picks their rosters for the rest of the season. In contrast, in the DFS format, players are assigned a salary for the given slate and you are free to pick from the entire pool of players each week so long as your final roster is under the given salary cap. Your team then participates in a tournament against every other player that has entered, and a certain percentage of lineups receive a payout relative to how their lineup finishes. To succeed in daily fantasy football, accurate projections are a must. However, despite there being free and subscription-based projections on the internet, the majority of these projections are lazily calculated. This is due to the format of DFS itself. If hundreds to thousands of players are generating their lineups off of the same projections, then it is unlikely that any individual will differentiate themselves enough to win money. Therefore, it is in the best interest of the people developing these projections to keep their most accurate projections to themselves. This is because the profit margin is higher to use them, rather than provide them for a fee. In addition, DraftKings assigns salaries to players based on their projected score meaning that the goal is to “beat the house” by taking advantage of undervalued players. Therefore, the goal of this project is to collect data for daily fantasy football, train a model, generate predictions, and compare test error to internet provided projections. The trained models will be at an immediate disadvantage due to experts making hand corrections for things such as injuries, players getting benched/promoted, etc. 

Methods

Dataset Creation

The data for this project was gathered from dailyfantasydata.com and included models for QBs, RBs, WRs, and TEs. The predictor variables differed for each position, however they will all follow the same general format: average season statistics, average RedZone statistics, Vegas game odds, team offense and opposing defense stats. Data was initially collected on a short-term basis with only a few hundred observations per position.1 However, the results suggested that this approach was too short sighted. Therefore, historical data was collected starting at 2015. Data was collected on a per game basis and averaged for each player leading up to the respected week where that week’s score was assigned as the outcome variable.2 Per game data was not available for team statistics so season averages were used for each week of the year. This approach was not ideal but was deemed the most appropriate solution given the circumstance. The final datasets from 2015 to 2018 week 9 included 1712 observations x 65 columns for QBs, 5574 x 51 for RBs, 5744 x 68 for WRs, and 2737 x 68 for TEs.3 

Model Training

Those familiar with fantasy sports and in particular fantasy football are well aware of the high variability of fantasy scores. While a player’s average scores may suggest how that player will produce across an entire season, it says nothing about how they perform week to week. Some players in particular are known for being boom/bust candidates. Therefore, only models which reduce variance were considered. In addition, many predictor variables and sports statistics show strong multicollinearity meaning linear models would have to correct for this. The final decision being to try a more traditional linear approach that can handle multicollinearity in partial least squares, and a non-parametric approach in random forest regression. Principal components to be used in regression were chosen using 10-fold cross validation. Random forest parameters were chosen using the sk.learn RandomizedSearchCV function using 5-fold cross-validation with 100 iterations. 

Testing

    QBs Week	PLS	    RF	    Provided	Player Average
    10	      62.12	  38.36	   48.17	    48.01
    11	      99.27	  66.87	   64.94	    70.97
    12	      49.19	  46.13	   42.29	    55.47
    13	      70.37	  60.91	   51.87	    67.03
    Average	      70.2375 	  53.0675  51.8175          60.37
The model was tested over the course of four weeks (NFL weeks 10, 11, 12, and 13).3,4,5,6 After each week the test data was appended to the training data in order to increase the number of training observations for the next week. Test set sizes were approximately 40 QBs, 110 RBs, 110 WRs, and 40 TEs per week. Test errors were compared to the provided projections and a player’s average score for projections for each week. In addition, the test errors were averaged for comparison. 

Results
The results for each week and averages are provided in the tables below. Error is reported in terms of mean squared error (MSE).

The partial least squares model struggled with the QBs projections having significantly higher error than every other predictor including a player’s average score. The random forest model was able to achieve similar results as the provided projections and well below the player’s average as a predictor. 

    RBs Week	PLS	  RF	  Provided	Player Average
    10	      69.45	  67.12	    55.98	  76.68
    11	      38.06	  41.58	    33            42.56
    12	      49.62	  51.22	    41.58	  49.39
    13	      34.62	  34.04	    33.4	  35.18
    Average	      47.9375     48.49     40.99	  50.9525

For RBs, the partial least squares model was able to outperform the random forest regression model. However, both performed relatively poorly compared to the provided projections. In addition, the errors are similar to those for a player’s average score. This could indicate that more predictor variable could be needed to explain the variance in RB scores as the dataset was large relative to the number of predictors. 

    WRs Week	PLS	  RF	  Provided	Player Average
    10	        37.43	  36.39	  32.59	        35.17
    11	        60.31	  60.32	  53.4	        56.63
    12	        46.88	  43.6	  42.37	        44.14
    13	        44.89	  39.53	  37.82	        45.46
    Average	        47.3775	  44.96	  41.545	45.35

    TEs Week	  PLS	  RF	 Provided	Player Average
    10	        25.26	  29.3	  30.03	        25.94
    11	        31.71	  27.95	  25.1	        31.13
    12	        16.7	  15.48	  15.59	        14.48
    13	        25.82	  23.05	  24.39	        21.97
    Average	        24.8725	  23.945  23.7775	23.38
For the WRs and TEs, the random forest regression model was again able to outperform the partial least squares model and approach the provided projections. However, in both circumstances the error rates approached those of the player’s average as a prediction. As for WRs, it appears that the RF model is converging on that of the provided projections and beginning to outperform the player’s average projections. More data would be needed to see if this trend continues. 

As for the TEs, the error rates seem to be relatively homogenous across the four predictors for each week. One possible explanation for this is the small number of fantasy relevant TEs in the NFL. The majority of TEs have underwhelming stat lines leading the majority of their fantasy scores to fall within a narrow margin with outliers. 

Conclusion

Overall the results of this experiment are satisfying. The generated dataset and models were able to adequately model the data and achieve consistent results with those provided by a subscription-based website. The random forest regression model was able to outperform partial least squares on 3 of the four positions. In addition, partial least squares appeared to be overall inadequate to model the variability in QB fantasy points. 

Although test errors never fell to a point below those of the provided projections, this is expected. The test errors for this model suffered from several disadvantages including a lack of hand corrections for things the model cannot see and less relevant players getting an increased projection due to projected increased team production. This is intentional bias in the model in order to find undervalued players by the system, however it comes at the cost of bad projections near 0. 

Additional work on this data would be to see which projections each model is closer on to determine if the models are accomplishing their overall goal of finding undervalued players. This would amount to checking errors within given score ranges for each model. 
