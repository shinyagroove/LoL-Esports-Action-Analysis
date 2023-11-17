# Analysis of Action Intensity in Top-Tier Pro LoL Leagues


## Overview
This data science project investigates ingame action intensity within professional League of Legends esports matches in 2022. The dataset used for this analysis, which is available [here](https://oracleselixir.com/tools/downloads), was originally compiled from detailed match statistics.

---


## Introduction
Esports are rapidly evolving and becoming a significant part of the entertainment industry and competitive game genre in general. This analysis serves to clarify which professional League of Legends league offers the highest level of excitement, as defined by specific action metrics. This insight can guide fans looking to maximize their viewing experience, especially when their time is limited, by focusing on the most exhilarating league. More specifically, this project aims to answer the question: **Looking at tier-one professional leagues, which league has the most “action-packed” games? Is the amount of “action” in this league significantly different than in other leagues?**



The 2022 dataset contains 149400 rows and 123 columns, with each `'gameid'` corresponding to 12 rows – one for each of the 5 players on both teams and 2 containing summary data for the two teams. 

The columns used in order to answer the questions are:
- `'gameid'`: An unique identifier for each match
- `'league'`: League is a competitive gaming circuit where professional players compete in LoL, they are organized regionally.
- `'ckpm'`: Average combined kills per minute (team kills + opponent kills)
- `'dpm'`: Average damage to champions per minute
- `'dragons'`: Number of dragons secured by the team in that match
- `'heralds'`: Number of heralds secured by the team in that match
- `'barons'`: Number of barons secured by the team in that match
- `'towers'`: Number of towers secured by the team in that match
- `'inhibitors'`: Number of inhibitors secured by the team in that match
- `'gamelength'`: Length of the match(in seconds)

And calculated columns:
- `'totalobjectives`': Total number of `'dragons'`, `'heralds'`, `'barons'`, `'towers'`, `'inhibitors'` combined.
- `'objectivepm'`: `'totalobjectives`/`'gamelength'`(converted to minutes)
- `'z_ckpm'`: Normalized `'ckpm'`
- `'z_dpm'`: Normalized `'dpm'`
- `'z_objectivepm'`: Normalized `'objectivepm'`
- `'actionscore'`: This is a direct measure of how frequently champions are killed in the game, which is a clear indicator of action. A higher CKPM would suggest more frequent team fights or skirmishes. **Defined** as `'z_ckpm'`+`'z_dpm'`+`'z_objectivepm'`



---


## Data Cleaning and Exploratory Data Analysis

### Data Cleaning
Steps taken:

1. Filtered rows, only matches from tier-one professional leagues are kept
2. Filtered rows, only rows that contain team statistics are kept
3. Filled in missing values in `'heralds'` column
4. Converted `'gamelength'` from seconds to minutes
5. Only relevent columns needed for analysis are kept, which are listed above
6. Calculated and added `'totalobjectives` column, refer to above for more detail
7. Aggregated rows, since statistics from each team are not needed, I went ahead and aggregated data for each match.
    - `'league'`: Took the last entry in each match
    - `'ckpm'`: Took the mean of two teams in each match
    - `'dpm'`: Took the sum of two teams in each match
    - `'dragons'`: Took the sum of two teams in each match
    - `'heralds'`: Took the sum of two teams in each match
    - `'barons'`: Took the sum of two teams in each match
    - `'towers'`: Took the sum of two teams in each match
    - `'inhibitors'`: Took the sum of two teams in each match
    - `'gamelength'`: Took the mean of two teams in each match
    - `'totalobjectives'`: Took the sum of two teams in each match
8. Calculated and added `'objectivepm` column, refer to above for more detail
9. Calculated z-scores of `'ckpm'`, `'dpm'`, `'objectivepm'` and added corresponding columns
10. Calculated and added `'actionscore'` column, refer to above for more detail


Cleaned dataset has 3040 rows and 16 columns.


---


### Univariate Analysis
<iframe src="assets/Distribution of gamelength(in minutes).html" width=800 height=520 frameBorder=0></iframe>
This is the distribution of `'gamelength'` in minutes, from this graph we spotted some unusually long games(45+ mins)
and some extremely short games. We also can see from this graph that a typical game lasts between 25-45 mins.

<iframe src="assets/Distribution of dpm.html" width=800 height=520 frameBorder=0></iframe>
It is pretty suprising to see there is a game that has a dpm of 7500-7600!

<iframe src="assets/Distribution of ckpm.html" width=800 height=520 frameBorder=0></iframe>
There is a game with 2+ ckpm!

---


### Bivariate Analysis
<iframe src="assets/Relationship between dpm and ckpm.html" width=800 height=520 frameBorder=0></iframe>
We see a positive correlation between dpm and ckpm, which... makes sense. The more damage players do, the more death
there are.

<iframe src="assets/Relationship between dpm and objectivep.html" width=800 height=520 frameBorder=0></iframe>
This is a little suprising, because you would typically expect a game to end more quickly if the players on both team are constantly fighting and dying. However, We do not see such trend in this graph.

---


### Interesting Aggregates

| league   |     ckpm |   objectivepm |   dragons |   heralds |   barons |   towers |   gamelength |
|:---------|---------:|--------------:|----------:|----------:|---------:|---------:|-------------:|
| CBLOL    | 0.874236 |      0.695468 |   4.6214  |   1.99177 |  1.41564 |  12.6461 |      32.9014 |
| LCK      | 0.700287 |      0.664385 |   4.82441 |   1.97645 |  1.42827 |  12.152  |      33.6677 |
| LCS      | 0.733371 |      0.667585 |   4.53922 |   1.98693 |  1.38235 |  12.085  |      33.0265 |
| LEC      | 0.804023 |      0.678104 |   4.41564 |   1.99177 |  1.45267 |  12.5597 |      33.221  |
| LJL      | 0.778508 |      0.670623 |   4.57477 |   1.96729 |  1.31308 |  11.6682 |      32.0295 |
| LLA      | 0.803713 |      0.656565 |   4.51337 |   1.96257 |  1.35294 |  11.9626 |      33.159  |
| LPL      | 0.839325 |      0.687861 |   4.43257 |   1.98092 |  1.34097 |  12.0318 |      31.5574 |
| PCS      | 0.833358 |      0.681979 |   4.39852 |   1.97417 |  1.2214  |  11.5314 |      30.9672 |
| VCS      | 1.05357  |      0.707931 |   4.13003 |   1.9969  |  1.32817 |  11.8266 |      30.016  |


The significance of this lies in its ability to infuse vast amounts of game data into a format that is immediately accessible and comparable. We can learn a lot about what a typical game in each tier-one league looks like by looking at this table. For example, dragons, heralds, and barons columns reflect teams' control over critical objectives, often key to winning. Towers indicate map control, while game length suggests the efficiency of regional strategies.


---


## Assessment of Missingness

### NMAR Analysis
Many of the columns has missing data, but I don't think any of them are NMAR. Because when you take `'league'` into account, you will find out that different league records data differently. For example, most of the leagues don't record url, only leagues like LPL, LDL and DCUP recorded it, which are all held in China. Therefore I suspect that most if not all missing data are dependent on country which is not recorded in this dataset, except data that are missing by design(MD) for players, like data about objectives(`'dragons'`, `'heralds` etc.) We would conclude that these data are missing at random(MAR) if we have data about where the matches are held.


---


### Missingness Dependency

<iframe src="assets/Distribution of missing values in heralds column across leagues.html" width=800 height=520 frameBorder=0></iframe>
As we have discovered from exploring the dataset, all of the missing herald data in this dataset come from the **LPL** league and in fact none of the match in LPL league has the herald data. Therefore, we suspect that the missingness of "heralds" column depends on the column "league".

We will run a permutation test to calculate analyze the dependency of missingness of this column on other columns, and use p-value of `0.05`

`Null Hypothesis`: The distribution of missingness in "heralds" column does not depend on column "league"<br>
`Alternative Hypothesis`: The distribution of missingness in "heralds" column depends on column "league"

Observed statistics = 1.0
After the hypothesis test, we got a p-value of `0.0` which is below the predefined cutoff, thus we `reject` the null hypothesis.
<iframe src="assets/Empirical Distribution depend.html" width=800 height=520 frameBorder=0></iframe>




Now let's examine a coloumn that `'heralds'` does not depend on.

`Null Hypothesis`: The distribution of missingness in "heralds" column does not depend on column "league"<br>
`Alternative Hypothesis`: The distribution of missingness in "heralds" column depends on column "league"

We got a p-value of 0.411, which is above the predefined cutoff, thus we `fail to reject` the null hypothesis.
<iframe src="assets/Empirical Distribution does not depend.html" width=800 height=520 frameBorder=0></iframe>


---


## Hypothesis Test

Again, to remind you, the question this analysis is trying to answer is: **Looking at tier-one professional leagues, which league has the most “action-packed” games? Is the amount of “action” in this league significantly different than in other leagues?**

`Null Hypothesis`: There is no significant difference in the amount of "action" (as measured by metrics such as 'ckpm', 'dpm', 'totalobjectives', etc.) across the tier-one professional leagues.<br>
`Alternative Hypothesis`: There is a significant difference in the amount of "action" across the tier-one professional leagues, with at least one league having a measurably higher level of "action" in its games compared to the others.

We will use `0.05` as the p-value cutoff and use difference in mean as the test statistics.

Observed difference is 1.9070013740063492, and the p-value is `0.0`. Thus we will `reject` the null hypothesis and conclude that the 'actionscore' for the VCS league is significantly different from the other leagues.
<iframe src="assets/Hypothesis test.html" width=800 height=520 frameBorder=0></iframe>


---

## Conclusion

Although it seems like we might have come to a conclusion that VCS league has the most action-packed game within the tier-one leagues, but I just want to remind you that the `"action"` is defined by myself and it is by no means a *correct* metric. Everyone enjoys esports differently, some may only consider a single key teamfight determining the outcome of a match thrilling, some might simply just want to see more teamfights and skirmishes like myself. You are welcomed to define your own `"action"` and find out which league is most enjoyable for you!

Safe gaming! Gamers!