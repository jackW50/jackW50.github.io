---
layout: post
title:      "Macro Tracker"
date:       2019-02-14 17:15:27 +0000
permalink:  macro_tracker
---


Macro Tracker is a rails app I created that allows users to keep track of the macronutrient compositions of their meals. This app was designed to give users the ability to create different types of meals, compromised of different foods, at different serving amounts. Each food would have a macronutrient composition, which would allow the macronutrient composition of the meal to be seen through the scope of the amounts of foods that the meal contains.

Before getting started on coding this app, I wanted to identify my models, and understand why and how they would be associated. These relationships would dictate how to retrieve the data I would need to define the macronutrient composition of a meal.

So initially I knew I would need a user model and a meal model. A user would have many meals, and a meal would belong to a user.  After that I needed to walk my way down to macronutrients. A meal would be compromised of foods, and a food could be in many meals. So in order for this relationship to be accurately represented, I would need a join table between my foods and meals. I also knew that foods are compromised of macronutrients, and macronutrients can be in many foods. So for this relationship, I would need something very similar to the meal-food relationship. I would need a macronutrient model, as well a join table to associate the food to the macronutrient.

With these models and associations established, I then decided to create my tables and run my migrations. Once I had the database set up and represented properly, I seeded the database, and began working on the queries. I would need to access data for my controllers to send to the views. I wrote out the queries as methods and scopes within my models. Once I had the accurate data accessed, I was ready to design the user interface of the program.

With this perspective in mind, I began walking myself through the program. Before a user would have access to the app, I would want the user to be authorized to do so. Before a user can be authorized they would need to be authenticated. So with this in mind, I knew that I would need a signup page and login page to either login or signup a user. Once the user is authenticated, they would then be authorized to see their show page. The user would also then have the ability to see, create, edit, and delete their meals. They would also have the ability to see the foods within the meal, create/add foods to their meal, see an individual food, update a food, or destroy a food. These options and flow helped me see the routes that would be needed to accomplish such an interface.

With this flow in mind, I understood that I would need a sessions controller to handle login and logout functionality. I would need a users controller to handle the signup functionality as well as to handle the user show page view. Then, I would need meals and foods controllers to handle the CRUD functionality. Within the CRUD functionality of the foods, I would need a couple of nested routes. Within the flow discussed above, when a user goes about creating a food, I would want that food to be associated to the meal that it was created under. I also knew I would want a view that would display all the foods that are contained within a meal. So in order take these features into effect, I would need nested routes under a meal that contained the foods ‘new’, and ‘index’ routes.

After I had the routes established, I moved into the controllers and views. When working with the view functionality of the program, I found it helpful to have an active sever which allowed me to see the changes or any current error of the program. I raised and inspected within the controller action I wanted the url to hit. This allowed me to make sure that the routes were being pushed into the right actions. Catching the url within the action allowed me to introspect on the current scope of that action. This gave me access to the data that the url request contained, and it allowed me to make sure that this data was being sent appropriately. 
When using this approach I started at the beginning of the program, and moved my way though. Starting with the signup page. A user would first need to signup before doing anything. Creating a view to match the route and have it displayed appropriately was step one. Once the view established correctly, I moved forward. The signup view would take in data from the user and send a new url request with this data. Once the request got made, I would catch it in my ‘raise inspect’ within the controller action. If the ‘rasie’ was hit I knew I was in the right action. Once this was confirmed I then introspected on the data that the url contained, and made sure that it was appropriate. If it was what I wanted, I designed the action using this data. This approach made it easy for me to just slow down and see the issue at hand in an up front and personal way. Understanding what the url contains and where it is going is very crucial. This ability to stop and see it, allowed me to craft the appropriate action and respond with the appropriate view.

Once the user data met validations, I set a session user_id. This essentially ‘logged’ them into the program. Setting a user_id within the session hash would allow the program to have access to who the current user is. This authentication process takes place under the user create action, and it authorizes the user to see their appropriate user show page. Having the session user_id set is important for the program because the session hash is accessible across all actions, as opposed to url data which is reset after every request. URLs are stateless, and if we needed to rely on the url for further authorization within our program, we would need the user data to be sent within that url on every single request. So essentially we would need the user to login on every request, which would not be ideal for our program, and it would not be very user friendly. 

```
  def create
    @user = User.new(user_params)

    if @user.save
      session[:user_id] = @user.id
      redirect_to user_path(@user)
    else
      render :new
    end
  end
```

So with the user now signed in, and identified as a current user(by using our session hash based off their id), I then walked my way through the other processes of the program. These processes would allow the user to make requests based off link clicks, form submission, or if they wanted, they could manipulate the url themselves and make a request. Within these request-response cycles or this game of back-and-forth, I was able to send each url to the appropriate action and send back an appropriate response. The responses varied, but controller access to the model allowed me to grab the appropriate data for that user request. The query methods came in handy and made accessing this data very easy. The controller would then relay the data back to my view and display the data within an html format. This html string could then be interpreted by the browser and display the data to the user in an eye appealing manner.

All in all this program was fun to design, but I found myself stuck at times, especially with getting the appropriate nested form data the way I wanted. Raising and introspecting on the data of the request within the controller action was very helpful. Also writing custom writer methods made it easier for me to see how my data was being associated with other models. If I could go back in time, I would probably write the form out in plain html first, get the parameters I want, then begin abstracting away. I started with the form builders right off the bat, and I got a little lost initially. Overall this project was fun and rewarding. Attached below is a video walkthrough of the app.

[https://www.youtube.com/watch?v=kapK1dQfBaI&feature=youtu.be](http://)

