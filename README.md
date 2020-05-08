Scouting a Defensive Midfielder: Data Analysis (Using MySQL and Python)

Project Overview:

-	Scouting a premier league midfielder who is below the age of 27 and who will reduce the number of goals conceded by the team.
-	Obtained player stats on the 2018/19 season from footystats.co.uk and as.eu 

Code and Resources Used:

-	Jupyter Notebook 6.0.3
-	MySQL
-	Python Version 3.7
-	Packages used: pandas, numpy, matplot.lib, seaborn
-	Datasets from Footystats.co.uk for the Premier League 2018/19 season: players.csv, teams.csv, league_matches.csv and league_stats.csv.
-	Data from as.eu for the Premier League 2018/19 season (minutes_played, aerial_balls_won, aerial_balls_lost, attempted_passes, Clearances, Clearances_leading_to_possession, Completed_passes, Fouls_committed, passes_intercepted, Successful_tackles, Tackles, Clean_sheets)

Data Cleaning:

-	Corrected positions of some players (e.g. Midfielder -> Defender)
-	Corrected incorrect data from footystats.co.uk by cross referencing with transfermarkt.co.uk
-	Removed player images and their positions from as.eu data
-	Removed hyperlink from player’s name from as.eu data
-	Unmerged cells and deleted blanks from as.eu
-	Joined many datasets scraped from as.eu into one








Exploratory Data Analysis (EDA):










































































Model Building:

Bar Graph: Decrease in goals conceded when player is on the pitch (per 90 mins);

-	I wanted to find the 'Decrease in goals conceded when player is on the pitch (per 90 mins)'. By joining the players.csv and teams.csv tables in MySQL, I was able to create a conceded_WO_per_90 column and subtract it from the conceded_per_90_overall to create the conceded_per_90_diff column. See Below:



-	Python code for Bar Chart:

















Scatter Graph: Retrieving possession;

-	

Using data from as.eu I was able to join them together in MySQL and produce a scatter graph of Successful Tackles v interceptions in Python. (see python code below)
-	Unfortunately, the data did not have the player’s positions and their names were slightly different to the dataset from footystats.co.uk so I was unable to join them and therefore some non-midfielders were included in the graph.



Scatter Graph: Retains possession: 

-	In MySQL, using the ‘successful passes’ and ‘attempted passes’ data from as.eu I was able to calculate the ‘Successful pass percentage’. 
-	In MySQL, using the ‘clearances leading to possession’ and ‘minutes played’ I could calculate ‘clearances leading to possession per 90’.
-	I was then able to plot a scatter graph in Python to compare players possession keeping capabilities (see below).


Radar chart: Key Candidates compared:

-	From the other Graphs I identified Onyinye Ndidi, Declan Rice and Jefferson Lerma as top candidates who were under the age of 27. 
-	I compared Successful pass percentage, Successful tackles per 90, Passes intercepted per 90 and Aerial balls won percentage
-	In MySQL I calculated success percentages for passes and aerial duals, I then times them by 3 to give the chart a clearer look when compared the other variables.
-	I then used Python the create the chart (see below).




