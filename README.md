# Scouting a Defensive Midfielder: Data Analysis (Using MySQL and Python)

# Project Overview:

-	Scouting a premier league midfielder who is below the age of 27 and who will reduce the number of goals conceded by the team.
-	Obtained player stats on the 2018/19 season from footystats.co.uk and as.eu 

## Code and Resources Used:

-	Jupyter Notebook 6.0.3
-	MySQL
-	Python Version 3.7
-	Packages used: pandas, numpy, matplot.lib, seaborn
-	Datasets from Footystats.co.uk for the Premier League 2018/19 season: players.csv, teams.csv, league_matches.csv and league_stats.csv.
-	Data from as.eu for the Premier League 2018/19 season (minutes_played, aerial_balls_won, aerial_balls_lost, attempted_passes, Clearances, Clearances_leading_to_possession, Completed_passes, Fouls_committed, passes_intercepted, Successful_tackles, Tackles, Clean_sheets)

## Data Cleaning:

-	Corrected positions of some players (e.g. Midfielder -> Defender)
-	Corrected incorrect data from footystats.co.uk by cross referencing with transfermarkt.co.uk
-	Removed player images and their positions from as.eu data
-	Removed hyperlink from player’s name from as.eu data
-	Unmerged cells and deleted blanks from as.eu
-	Joined many datasets scraped from as.eu into one

## Exploratory Data Analysis (EDA):

![](https://github.com/cooperh01/pl-defensive-mid/blob/master/Bar_chart.png)

![](https://github.com/cooperh01/pl-defensive-mid/blob/master/Retaining_poss_graph.png)

![](https://github.com/cooperh01/pl-defensive-mid/blob/master/Retrieving_poss_graph.png)

![](https://github.com/cooperh01/pl-defensive-mid/blob/master/Radar_Chart_KCC.png)


## Model Building:

## Bar Graph: Decrease in goals conceded when player is on the pitch (per 90 mins);

-	I wanted to find the 'Decrease in goals conceded when player is on the pitch (per 90 mins)'. By joining the players.csv and teams.csv tables in MySQL, I was able to create a conceded_WO_per_90 column and subtract it from the conceded_per_90_overall to create the conceded_per_90_diff column. See Below:



-	Python code for Bar Chart:
```python
# Bar Graph (Decrease in goals conceded when player is on the pitch (per 90 mins))

plt.figure(figsize=(20,7))
           
sns.barplot(top_30.full_name, top_30.conceded_per_90_diff)
plt.title('Decrease in goals conceded when player is on the pitch, per 90 mins (Top 25)', fontsize=28)
plt.ylabel('Conceded without player per 90 - Conceded with player per 90')
plt.xlabel('Player name')
plt.xticks(rotation=50)

plt.show()
```

## Scatter Graph: Retrieving possession;

-	

Using data from as.eu I was able to join them together in MySQL and produce a scatter graph of Successful Tackles v interceptions in Python. (see python code below)
```python
# Scatter Graph: Obtain possession(Successful Tackles v interceptions)

plt.figure(figsize=(20,20))

pip90 = ivt.passes_intercepted_per90
stp90 = ivt.successful_tackles_per90
plr = ivt.player

plt.scatter(pip90, stp90, c=stp90)
plt.title('Retrieving Possession (Top 50 from Successful tackles per 90)', fontsize=28)
plt.xlabel('Passes Intercepted per 90', fontsize=12)
plt.ylabel('Successful Tackles per 90', fontsize=12)

# labels for points

for i, txt in enumerate(plr):
    plt.annotate(txt, (pip90[i], stp90[i]),
                textcoords="offset points", # how to position the label
                xytext=(0,10), # distance from label to points (x,y)
                size=12, # size of label
                ha='center') # horizontal alignment can be left, right or center

plt.show()
```

## Scatter Graph: Retains possession: 

-	In MySQL, using the ‘successful passes’ and ‘attempted passes’ data from as.eu I was able to calculate the ‘Successful pass percentage’. 
-	In MySQL, using the ‘clearances leading to possession’ and ‘minutes played’ I could calculate ‘clearances leading to possession per 90’.
-	I was then able to plot a scatter graph in Python to compare players possession keeping capabilities (see below).
```python
# Scatter Graph: Retain possession (Successful pass rate v clearences to possesion per 90)

plt.figure(figsize=(20,20))

spp = rpmi.successful_pass_percentage
ctp = rpmi.Clearences_leading_to_possession_per90
plr = rpmi.player

plt.scatter(spp, ctp, c=ctp)
plt.title('Retaining Possession (Top 30 from Successful pass%)', fontsize=28)
plt.xlabel('Successful passes %', fontsize=12)
plt.ylabel('Clearences leading to possession per 90', fontsize=12)

# labels for points

for i, txt in enumerate(plr):
    plt.annotate(txt, (spp[i], ctp[i]),
                textcoords="offset points", # how to position the label
                xytext=(0,10), # distance from label to points (x,y)
                size=12, # size of label
                ha='center') # horizontal alignment can be left, right or center

plt.show()
```

## Radar chart: Key Candidates compared:

-	From the other Graphs I identified Onyinye Ndidi, Declan Rice and Jefferson Lerma as top candidates who were under the age of 27. 
-	I compared Successful pass percentage, Successful tackles per 90, Passes intercepted per 90 and Aerial balls won percentage
-	In MySQL I calculated success percentages for passes and aerial duals, I then times them by 3 to give the chart a clearer look when compared the other variables.
-	I then used Python the create the chart (see below).

```python
plt.figure(figsize=(15,15))
    
# number of variable
categories=list(kc)[1:]
N = len(categories)
 
# angle of each axis in the plot
angles = [n / float(N) * 2 * pi for n in range(N)]
angles += angles[:1]
 
# spider plot
ax = plt.subplot(111, polar=True)
 
# axis to be on top
ax.set_theta_offset(pi / 2)
ax.set_theta_direction(-1)
 
# axe per variable
plt.xticks(angles[:-1], categories)
 
# ylabels
ax.set_rlabel_position(50)
plt.yticks(color="grey")
plt.ylim(0,2.8)
 

 
# Ind1
values=kc.loc[1].drop('player').values.flatten().tolist()
values += values[:1]
ax.plot(angles, values, linewidth=1, linestyle='solid', label="Onyinye Ndidi")
ax.fill(angles, values, 'b', alpha=0.1)
 
# Ind2
values=kc.loc[18].drop('player').values.flatten().tolist()
values += values[:1]
ax.plot(angles, values, linewidth=1, linestyle='solid', label="Declan Rice")
ax.fill(angles, values, 'r', alpha=0.1)

# Ind3
values=kc.loc[108].drop('player').values.flatten().tolist()
values += values[:1]
ax.plot(angles, values, linewidth=1, linestyle='solid', label="Jefferson Lerma")
ax.fill(angles, values, 'g', alpha=0.1)
 
# legend
plt.legend(loc='upper right', bbox_to_anchor=(0.1, 0.1))

# title
plt.title('Radar Chart: Key Candidates Compared (percentages x3)', fontsize=30)

plt.show()
```



