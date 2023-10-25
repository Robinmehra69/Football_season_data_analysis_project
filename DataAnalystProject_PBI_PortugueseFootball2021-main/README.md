# Football Portuguese League 2021-2022 season
In this project I collect data from Liga Portugal Bwin 2021-2022 season and transform it in Power BI to show league table, top goal scorers, fixtures, team stats.

![Football](https://user-images.githubusercontent.com/61323876/135532587-5dfb9b25-fa23-4ac8-b173-1fe0c7ce25d0.jpg)

## Data Source
The data is collected from [this website](https://www.football-data.org/) via REST API and loaded directly into Power BI.

## Data Collection
From the API I used the following endpoints to get the data that I needed:

* Matches from PPL (Portuguese Primeira Liga)
* Standings
* Scorers

## Data Model
Using the endpoints stated above as a starting point for my analysis and data visualization, I created a couple more tables to achieve my goal.
Here is a brief explanation for each table included in the Power BI model:
* __Date__: I created a Date table to manage date related values for all other tables on the model
* __Measures Table__: table with all calculations defined in Power BI
* __Home Stats__: table with the data from Matches endpoint filtered with only home stats 
* __ResultColor__: Auxiliar table to define background colour when result is a Win (W), Draw (D), Loss (L)
* __Team Stats__: Table with the data from Matches endpoint with all the matches
* __Standings__: Table with the data from Standings endpoint
* __Top Goalscorers__: Table with the data from Scorers endpoint
* __Team Logo__: Table created with list of team names and respective logos to help with the presentation of the created dashboards
*  __Opposing Team Logo__: The same as the table above for the opposing team
*  __Portuguese League Logo__: Table with the name of the league and Logo
*  __Last Refresh__: Table with the date/time record of the last update of data

<img width="809" alt="DataModel" src="https://user-images.githubusercontent.com/61323876/135533240-8cc5ebbf-6c6a-474c-9209-b0f14ca900ff.png">

## Calculations
The following calculations were created in the Power BI reports using DAX (Data Analysis Expressions).
To lessen the extent of coding, the re-use of measures (measure branching) was emphasized:
```
% Over 2.5 Goals = 
IF(
    [Over 2.5 Goals] / [Total Matches Played] = BLANK(),
    0,
    [Over 2.5 Goals] / [Total Matches Played]
)

% Under 2.5 Goals = IF(
    [Under 2.5 Goals] / [Total Matches Played] = BLANK(),
    0,
    [Under 2.5 Goals] / [Total Matches Played]
)

% Wins = 
CALCULATE(
    COUNTROWS('Team Stats') / [Total Matches Played],
    'Team Stats'[Result] = "W"
)+0

Goals Conceded (Avg) = 
SUM(Standings[goalsAgainst]) / SUM(Standings[playedGames])

Goals Scored (Avg) = 
SUM(Standings[goalsFor]) / SUM(Standings[playedGames])

Over 2.5 Goals = 
CALCULATE(
    COUNTROWS('Team Stats'),
    FILTER('Team Stats',
    'Team Stats'[GF] + 'Team Stats'[GA] > 2.5
    ),
    'Team Stats'[Result] <> BLANK()
)+0

Team Selected = SELECTEDVALUE('Team Logo'[Team],"!!!")

Total Matches Played = 
CALCULATE(
    COUNTROWS('Team Stats'),
    'Team Stats'[Result] <> BLANK()
)

Under 2.5 Goals = 
CALCULATE(
    COUNTROWS('Team Stats'),
    FILTER('Team Stats',
    'Team Stats'[GF] + 'Team Stats'[GA] < 2.5
    ),
    'Team Stats'[Result] <> BLANK()
)+0
```

## Dashboards
The finished dashboard consists of visualizations and filters that gives an easy option for the end users to navigate through the data of Portuguese football Primeira Liga. 

There are 3 pages with specific data (League Table & Top 10 Goalscorers, Fixtures, Team Stats) and an additional page defined as tooltip to provide more contextual information and detail on the league table visual.
When a team is highlighted in the league table more info is provided regarding the results of the last 5 fixtures.

Below there is a screenshot providing this insight.

<img width="674" alt="LeagueTableTooltip" src="https://user-images.githubusercontent.com/61323876/135535526-c6ee6aa8-7da4-4c3c-b1e6-10853c634d26.png">

Below I show you a couple more screenshots with the other dashboards defined.

__Click [here](https://app.powerbi.com/view?r=eyJrIjoiNDE0NjgxYTktZTdhMy00OTMzLTk0OTUtZmNjNTE3YWQ1YjM2IiwidCI6IjBiZmE4NTAwLWIxZjItNDU2Ni1iYWYxLTZmNTkzNzA4OTNlNyIsImMiOjh9) to open the dashboard and try it out!__

<img width="672" alt="LeagueTable1" src="https://user-images.githubusercontent.com/61323876/135535728-54bafcb4-170e-45a0-b896-9dd5c8dec4c7.png">
<img width="960" alt="Fixtures" src="https://user-images.githubusercontent.com/61323876/135535742-25bb3ef1-db64-43f4-bc0d-6aaa35eeb731.png">
<img width="669" alt="TeamStats" src="https://user-images.githubusercontent.com/61323876/135535755-93448caa-7729-4cde-b4b9-48131e6e48a8.png">
