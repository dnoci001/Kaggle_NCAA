# Kaggle_NCAA
Predict Win Probablities in NCAA March Madness Tournament

Yearly Mens and Womens College Basketball host 'March Madness' where the top ranked 64 teams compete. 
Kaggle has hosted their own competition inviting data scientist to predict win probablities for all possible 
team mach-ups in the tournament. Kaggle has provided historical data of wins in losses of regular season 
games back to 1985 and detailed game stats back to 2003.

## Detailed Game Stats
    #FGM - field goals made (by the winning team)
    #FGA - field goals attempted (by the winning team)
    #FGM3 - three pointers made (by the winning team)
    #FGA3 - three pointers attempted (by the winning team)
    #FTM - free throws made (by the winning team)
    #FTA - free throws attempted (by the winning team)
    #OR - offensive rebounds (pulled by the winning team)
    #DR - defensive rebounds (pulled by the winning team)
    #Ast - assists (by the winning team)
    #TO - turnovers committed (by the winning team)
    #Stl - steals (accomplished by the winning team)
    #Blk - blocks (accomplished by the winning team)
    #PF - personal fouls committed (by the winning team)
    
## Method
    
   A common method is to apply a ranking metric to each competitor and predict the outcome using some combination 
   of the rankings. Commonly used metrics are Elo and Glicko. One of the shortcomings of these metrics is the need
   for a user to designate a parameter (C) for the sensitivity to recent vs. historical matches. There are strong 
   arguments for the what the optimal value of this parameter might be given the context. To bypass this shortcoming
   we will compute glicko ratings using multiple values of this parameter and make our prediction using an ensemble 
   of glicko ratings. 
   
   Commonly these rating systems are just applied to the win state of competitors, but they aren't
   inherintly limited to just that, so we will update a rating after each game for all statistics given in the detailed 
   game file. This will allow us to capture more of the nuance to the games. Ex. Maybe TeamA lost to TeamB, but they out
   performed in their FGM3, so by considering all stats we develop a more detailed picture of each teams strengths and 
   weaknesses. 
   
   We will then calculate the covariate matrix for these rankings from the regular season games. This will be used with numpy's
   multivariate sampling to simulate games between competitors. We now can get believable rankings for each competitor and how 
   they might vary relative to whome they are competing against, but we need a method to combine these to a final metric to 
   determine the outcome our simulated game. I have decided to use the 'GameScore' metric which was a linear combination of the
   stats we ranked. Since it isn't clear that the combination coefficets for the stats would also be appropriate for our ranking
   of these stats we will revaluate appropriate coefficets.
   
   We can then simulate thousands of games for each match-up for all Glicko ratings using different C values. Giving use a picture 
   of game outcome between a more historical representation of each team (lower C), to a picture of more recent representation
   of each team (higher C). We can then use logistic regression to get a final probablity of the outcome of the game.
   
   # Glicko
   
   ![](https://github.com/dnoci001/Kaggle_NCAA/blob/main/images/rating_deviation.svg)
