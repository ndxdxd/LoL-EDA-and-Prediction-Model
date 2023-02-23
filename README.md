# The-Carry

by Andy Truong (amt007@ucsd.edu)

# Introduction

A DSC80 Exploratory Data Anaylsis Project on which League of Legends (LoL) position/role "carries" the hardest, the Attack Damage Carry (Bot), commonly refered to as the ADC, or the Mid Laner (Mid).

It has long been a debated topic about which role in the game is the most impactful between the Mid Laner and the ADC. The Mid Laner being the spearhead of the team, as the player controls the shortest path to the enemy's nexus. Whereas the ADC is the main damage source for their team to pick off enemy players and guide their team to success.

Although both roles are essential to winning, we want to know which role in particular between the two "carries", performs exceptionally well in matches and the most effective on the team, the hardest.

The data set which we will use is from Oracle's Elixir, which provides CSV files of data on League of Legend Competitive Matches from 2014 to 2023. However, for our project, we will be focusing on 2022, since it is the most recent completed data set. There are groups of 12 rows which contain one match summary in the data set: 10 rows which represents the number of players on both teams, and 2 more rows that contain the team's summary data.

The data set has 149232 rows, and 123 columns. However, we will be focusing on these columns which will help answer our question: position, datacompleteness, result, playername, kills, assists, deaths, totalgold, damagetochampions, doublekills, triplekills,quadrakills, and pentakills.

position : position/role for each team, which contains 'top','jng' (jungle),'mid','bot', and 'sup' (str).
datacompleteness : whether a game has missing data, contains 'complete' and 'partial' as values (str).
result: indicates whether that player/team won the match (1 is True, 0 is False) (int)
playername: name of the player on the team (str).
kills: total number of kills for that player (int).
assists: total number of assists for that player (int).
deaths: total number of deaths for that player (int).
totalgold: total gold accumalted in the game for that player (int).
damagetochampions: total damage to champions by the player (int).
doublekills: number of double kills made by the player (int).
triplekills: number of triple kills made by the player (int).
quadrakills: number of quadra kills made by the player (int).
pentakills: number of penta kills made by the player (int).

# Cleaning and EDA

Since we are focused on the position played by each player, we do not need the rows which contain the team's summary data. Therefore, we keep the rows where the playername column is not null, since the team rows have null values in the playername column.

After that, we want to convert the result column to a series of boolean values which indicate whether a team has won or lost.
Thus, we apply a lambda function which converts those 1 and 0s to Trues and Falses.

To create another metric to determine which role performs the best, we will use a point system, that is found in the League of Legends Wikia, called "Dominance Factor" that is used to determine how dominant the player was in the game.
**Equation: Dominance Factor = 2*Kills + Assists - 3*Deaths**

Unsurprsingly, the data set does contain empty values. Therefore, we must fill those null values with np.NaNs, specifically within the columns that contain the multi kills.

Furthermore, we want to be able to get the amount of multi kills each player got in a game. So we will add up the count of double, triple, quadra, and penta kills and assign a new column to that dataframe called 'multikills'

We will also drop playername column since we do not need to know the names of each player.

## Univariate Analysis

Average Dominance factor score between Mid and Bot.

<iframe src="assets/dominance_factor_bar.html" width=800 height=600 frameBorder=0></iframe>

## Bivariate Analysis

## Interesting Aggregates

# Assessment of Missingness

## NMAR Analysis

A column where we believe could be argued as NMAR, where the missingness depends on the values themselves, is the doublekill column. Since there is a chance that games are not as "intense" with multiple killing sprees as enemies could be picked off one by one, it is reasonable that games could not have double kills since there could be moments where it isn't possible or the game is simply won through objectives. Or simply there could be no carry on the team that eliminates a lot of enemies at once.

## Missingness Dependency

We will run a permutation test to determine if the damagetochampions column is MAR vs MCAR, with the column datacompleteness. We will compare the distribution of data completeness when damagetochampions is missing vs not missing.

We chose our test statistic to be be Total Variation Distance, since we are looking at two categorical distributions, complete vs partially complete in the data completeness column. We will also use a significance level of 0.05

After running our permutation test by shuffling the missing damagetochampion columns, we found that our p value is 0.012, which is less than the signifcant level. Thus, we reject the null hypothesis that the damagetochampion column does not depend on the data completeness column, and is likely to be MAR dependent on datacompletness.

insert graphs and tables

We also will run a permutation test to determine if multi kills is MAR vs MCAR with the column position.

In similar fashion, we choose TVD as our test statistic and a significance level of 0.05.

After running our permutation test, we get a p value of 1, which suggests that we strongly fail to the null hypothesis that the distribution of positions is the same when multi kills is missing or not missing. This means that that the multkills column could potentailly be MCAR.

insert graphs and tables

# Hypothesis Testing

Now to finally answer our question, which position carries the hardest, bot or mid? To determine this, we will run a permutation test.

Null Hypothesis: The Dominance Factor of both Bot and Mid positions will be the same on average
Alt Hypothesis: The Bot position's average Dominance Factor would be greater than the Dominance Factor of the Mid Laner.

The test statistic that we will use is the difference in means of dominance factor between bot and mid.

<iframe src="assets/file-name.html" width=800 height=600 frameBorder=0></iframe>
