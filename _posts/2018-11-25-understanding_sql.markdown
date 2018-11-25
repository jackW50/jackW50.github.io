---
layout: post
title:      "Understanding SQL"
date:       2018-11-25 20:43:36 +0000
permalink:  understanding_sql
---


Persisting information to a database helps us manage the data associated with our applications. SQL gives us a way to insert data and and a way to retrieve the necessary data from these databases. This retrieved data helps our applications run properly.  At first I had a hard time understanding the SQL retrieval statements. These statements are important because they help us specify the types of information we want returned back to our application. We can ask our programs questions about data, and it is our programs responsibility to go into our tables to retrieve this data to answer the questions. SQL actually reads out nicely once I started getting the hang of it.

In order to help me grasp some of these retrieve statements I designed my own examples. One of the examples I designed was a sports-agent-players-fans model. For this basic model I persisted data pertaining to teams, agents, fans and players. Each class has a relational database that stores their information, and the database will factor in the associations between these objects. So before I created the tables I wanted to recognize the relationships between these objects. In this example I wanted the relationships to be able to be represented in the tables properly. I had a team having many players, and I saw the players belonging to a team. I  also had an agent representing many players and a player only having one agent. The next relationship was that a fan can root for many players and that a player can have many fans. This relationship was a ‘many to many’, and in order to represent it in the database I created a join table called ‘players_fans’.

```


CREATE TABLE teams (
  id INTEGER PRIMARY KEY,
  name TEXT
);

CREATE TABLE agents (
  id INTEGER PRIMARY KEY,
  name TEXT
);

CREATE TABLE players (
  id INTEGER PRIMARY KEY,
  name TEXT,
  team_id INTEGER,
  agent_id INTEGER,
  salary INTEGER
);

CREATE TABLE fans (
  id INTEGER PRIMARY KEY,
  name TEXT,
  email TEXT
  );

CREATE TABLE players_fans (
  id INTEGER PRIMARY KEY,
  player_id INTEGER,
  fan_id INTEGER
  );

```
Now that the tables are created and representing the relationships I want, I can start persisting information.
```
INSERT INTO teams (name) VALUES ( “Atlanta Hawks”);
INSERT INTO teams (name) VALUES (“Shanghai Sharks”);
INSERT INTO teams (name) VALUES (“Brooklyn Nets”);

INSERT INTO agents (name)  VALUES (“Jerry Maguire");
INSERT INTO agents (name)  VALUES (“Norman Dale”);
INSERT INTO agents (name)  VALUES (“Happy Gilmore");

INSERT INTO players (name, team_id, agent_id, salary) VALUES (“Thomas”, 1, 1, 5000000);
INSERT INTO players ( name, team_id, agent_id, salary) VALUES (“Brian”, 1, 2, 2010000);
INSERT INTO players ( name, team_id, agent_id, salary) VALUES (“Shannon”, 1, 3, 7000000);
INSERT INTO players ( name, team_id, agent_id, salary) VALUES (“Thomas”, 2, 2, 2000000);
INSERT INTO players ( name, team_id, agent_id, salary) VALUES (“Ryan”, 2, 2, 2010000);
INSERT INTO players ( name, team_id, agent_id, salary) VALUES (“John”, 2, 3, 6500000);
INSERT INTO players ( name, team_id, agent_id, salary) VALUES (“Cam”, 2, 2, 5000000);
INSERT INTO players ( name, team_id, agent_id, salary) VALUES (“Andre”, 3, 1, 4010000);
INSERT INTO players ( name, team_id, agent_id, salary) VALUES (“Joe”, 3, 2, 5600000);

INSERT INTO fans (name, email) VALUES (“Timmy”, “tim@soandso.com“);
INSERT INTO fans (name, email) VALUES (“Billy”, “bil@soandso.com“);
INSERT INTO fans (name, email) VALUES (“Jimmy”, “jim@soandso.com“);

INSERT INTO players_fans (fan_id, player_id) VALUES (1, 4);
INSERT INTO players_fans (fan_id, player_id) VALUES (1, 2);
INSERT INTO players_fans (fan_id, player_id) VALUES (1, 3);
INSERT INTO players_fans (fan_id, player_id) VALUES (2, 3);
INSERT INTO players_fans (fan_id, player_id) VALUES (2, 5);
INSERT INTO players_fans (fan_id, player_id) VALUES (3, 3);
INSERT INTO players_fans (fan_id, player_id) VALUES (1, 6);

```
So once I get the databases set up and data persisted, I can now ask myself questions so I can practice using SQL to retrieve information to answer these qustions. So I'll just make up some questions I want to answered:

1. What team does the most popular player play for?
2. If an agent takes 6% of the players salary, who is the highest paid agent, and what is the amount?
3. Who are the players that are represented by Norman Dale and make over $4000000?
4. Who is the Shanghai Sharks most popular agent?

Let’s just say that these are some questions I want for my program to answer. In order to answer these questions I will use SQL retrieval statements.  When setting up SQL statements I first like to recognize the information the questions want before I try to figure out how I will access that information. So for the first question, it is asking for a team. This question doesn’t specify a specific property of the team, so we can use * to just return the row of the team. So with this information I can then set up the first part of my SQL. Since I want a team this information will come from my teams table.
```
SELECT teams.* FROM teams
```
Once I have the information identified, I then look at my table relationships to figure out how to access this information. So in the first question, when I’m asking myself who the most popular player is, I would have to figure out which player has the most fans. To get this information we will need access to our players_fans table. To get information from another table we will have to JOIN the tables, and since the teams table does not have direct access to the players_fans table we will have to work our way to that table through the players table.
```
SELECT teams.* FROM teams 
INNER JOIN players ON teams.id = players.team_id 
INNER JOIN players_fans ON players.id = players_fans.player_id
```
Once I have access to the tables that will give me the information, I need to solve the question. I will now use logic to figure out how I would solve this question. So how do I get the player with the most fans?? In order to figure this question out, I will have to look into the players_fans table and group the players together. When I have players grouped by their foreign key(player_id), I will then need to rank them by how many fans that they have. So we will have to add together how many fans are associated with this particular player. 
```
SELECT teams.* FROM teams 
INNER JOIN players ON teams.id = players.team_id 
INNER JOIN players_fans ON players.id = players_fans.player_id 
GROUP BY players_fans.player_id 
ORDER BY( COUNT(players_fans.fan_id)) 
DESC LIMIT 1;
```
In this query I use the GROUP BY function to give me the unique players and not duplicates, and once they are grouped I use another function ORDER BY to rank the players by how many fans are associated with them. Then in this rank I put them in descending order(DESC) from highest to lowest, and limit this selection to just one, which will give us our top option or player with the most fans. 

This process of first identifying data, then figuring out which tables we need access to, then defining the logic to get the specific information is how I have been figuring out how to write these statements.

2. If an agent takes 6% of the players salary, who is the highest paid agent, and what is the amount?

In this query I would have to find the highest paid agent. I think it can be solved in may different ways, but identifying that I need an agent and a salary is my first step. In the agents table they have a name, but they do not have a salary. So what information am I trying to identify? Since agent takes 6% of the players salary I want to return the sum of 6% of each players salary that that agent represents. So the information I would need to identify are the players salary, and the agents name.
```
SELECT agents.name, players.salary
```
Once the information is identified, how would the information be accessed? This query as we can see draws information from two different tables, so I will need to join them to gain access. 
```
SELECT agents.name, players.salary FROM agents 
INNER JOIN players ON players.agent_id = agents.id
```
Now that the tables are joined I will then figure out the logic on how to answer the question. This answer will involve a little bit of math and some limiters. I first need to make sure that the salary that I want returned is represented appropriately to the agent The agent makes 6% of the players salary they represent, and not the whole amount. So I need to make that representation accurate. 
```
SELECT agents.name, SUM(players.salary * .06) FROM agents 
INNER JOIN players ON players.agent_id = agents.id
```
Next I want to figure out how to group the agents into a ranking. I used the GROUP BY function to group the information by agent_id, and then I need to rank these agent_id’s into a descending order by the sum of 6% of each players salary they represent.
```
SELECT agents.name, SUM(players.salary * .06) FROM agents 
INNER JOIN players ON players.agent_id = agents.id 
GROUP BY players.agent_id 
ORDER BY (SUM(.06 * players.salary));
```
When I get the order, then I want to limit the selection to 1 to just get the top choice. We can also put in an alias column name for the column we want to represent the agents salary and it can look something like this.
```
SELECT agents.name, SUM(players.salary * .06) AS “agent_salary” FROM agents 
INNER JOIN players ON players.agent_id = agents.id 
GROUP BY players.agent_id 
ORDER BY (SUM(.06 * players.salary)) DESC 
LIMIT 1;
```

3. Who are the players that are represented by Norman Dale and who make over $4000000?

For this question I will need to identify players.
```
SELECT players.name FROM players
```
Next I will need access to agents table
```
SELECT players.name FROM players 
INNER JOIN agents ON agents.id = players.agent_id
```
Next I figure out the logic. The players need to be represented by the agent with the name of Norman Dale, and their salary needs to be over $4000000.
```
SELECT players.name FROM players 
INNER JOIN agents ON agents.id = players.agent_id 
WHERE players.salary > 4000000 AND agents.name = “Norman Dale”;
```
4. Who is the Shanghai Sharks most popular agent?

Our next question is asking for us to find an agent. 
```
SELECT agents.name FROM agents
```
I will need access to the players that belong to the Shanghai Sharks. So I will need access to our players and their teams table.
```
SELECT agents.name FROM agents 
INNER JOIN players ON players.agent_id = agents.id 
INNER JOIN teams ON players.team_id = teams.id 
```
Then I will figure out the logic to solve the question. I will need  the players who play for the Shanghai Sharks. Then for those players I will need to GROUP them by their agent_id and order them in descending order by how many players they represent. 
```
SELECT agents.name FROM agents 
INNER JOIN players ON players.agent_id = agents.id 
INNER JOIN teams ON  players.team_id = teams.id 
WHERE teams.name = "Shanghai Sharks" 
GROUP BY agent_id 
ORDER BY(COUNT(players.id)) DESC
LIMIT 1;
```
Designing these examples has helped me learn how to design SQL retrieval statements. There are many other questions you can ask, and play around with. Practicing this way has helped me out a ton, and keeping things down to a step-by-step method helps keep it simple.
1. First identify the data
2. Access the tables 
3. Figure out the logic on how to get the specific information I need.
