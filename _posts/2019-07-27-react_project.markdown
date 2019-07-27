---
layout: post
title:      "React Project"
date:       2019-07-27 20:12:45 +0000
permalink:  react_project
---


When I first read the requirements for this project I had a lot of uncertainty, more so than all the other projects. I found react to be fun framework to interact with throughout the curriculum, but I was unsure on how I would ever build anything with it. This project was a good awakening into that mindset. While I still have much practice ahead of me, I started to get the hang of it as I directed my thoughts towards this mindset. After talking with an instructor in the project planning meeting, I decided to keep the project simple, and make sure I understood the flow of the program.

I decided to create a rails api of coffee products, and to interact with this data through fetch requests from my react front-end. I wanted the react app to be able to send get coffees, post coffee, update coffee, and delete coffee requests to my back-end. I also wanted the react app to send post comment, and delete comment requests as well. This type of interaction would be crucial to the state of my react app, so in order to have it work properly I would need a well organized database, and the appropriate controller actions.

The first step was to refresh myself on active record and associating the data properly. For this project I ended up having three models: a coffee model, a roast model, and a comment model. A coffee would have many roasts, and roasts would have many coffees. A coffee would have many comments and a comment would belong to a coffee. For these associations to be represented in my database, I gave the comments table a user_id integer property, and created a join table coffee_roasts that stored a coffee_id integer and a roast_id integer. These associations helped keep the data appropriately organized and allowed me to use active record helper methods to call upon the associated data, and to create, update, or delstroy associated data.

Once the database and models were organized properly, I then proceeded to create the routes that would be needed. For this app I understood I would need a coffee index, create, update, and destroy route, and nested under a particular coffee route I would need comment create, and destroy routes. The routes actions would then be detailed in a coffees controller and a comments controller. So when a request would come in from my react app, the request would hit my controller action, and a data interaction would take place. This interaction would involve either calling upon data in the database to return back to the user, inserting data into the database,  or removing data from the database. These calls to actions would fulfill the user requests from my front-end.

After I had the rails api fully created, I then proceeded to the front end, and wrapped my head around the ways in which I could allow the user to see, create, update, and delete the data in a user friendly format. This is where React proved itself very effective. The framework allowed me to keep things in their own little separate area, and easy to organize. I decided to use redux middleware to handle the state of the app, and redux thunk to handle action creator asynchronous requests. This middleware gave my components the ability to access state data, update state data, and to send requests to the back-end. It gave simplicity to my components and allowed me to make much of them re-usable and stateless.

To start with the react app, I drew out my containers and how I wanted them to be displayed, then I started from the bottom with the smallest component and worked my way up to the containers. Once I had each component created and props passed appropriately down to each component, I then connected the containers to the store to access my state and my action creators. This access allowed my containers to pass down state data and actions to the components via props that they could use for rendering purposes and to include in event handlers. This gave the react app easy access to state and allowed async requests to be easily sent to the back-end if requested to do so by the user.

I think the more I practice with React, the more enjoyable it seems to become. I remember when I first started with it and how hard it was form me to see what was going on, but now I can follow the program from the top down and from the bottom up. The toughest thing about this project was just finding a starting point. Once I got going it seemed to go very smoothly. All in all I look forward to using React for future projects.

Video Walkthrough:
[https://www.youtube.com/watch?v=bgCy6P_bVjE&t=9s](http://)
