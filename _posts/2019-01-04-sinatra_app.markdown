---
layout: post
title:      "Sinatra App"
date:       2019-01-04 18:34:38 +0000
permalink:  sinatra_app
---


The idea behind this application was to create a user profile with access to reminders or notes that they could write, and have access to in different types of formats. I wanted to categorize these reminders into 3 groups: daily, weekly, and monthly reminders. I wanted the user to have easier access to the daily reminders because I figured that since these are things that they would want to see every day, they should be prioritized.

Before beginning to code this app, I first wanted to have the ability to see it in my head. I wanted to be able to see the flow and the relationships. I think it is a good idea to see something before getting started. The basic flow of the program would start with the user having access to a welcome page with links to a signup or a login form. I would then want the user to have the ability to log in if they have been saved to the database or to signup with unique and validated information. Once the user is signed in, I would want them to have direct access to their profile. The profile should include a greeting, date, and a list of their daily reminders. I prioritize the daily reminders because I see them as objects with more immediate importance than weekly or monthly reminders. Their profile would also have links to edit their profile, to see an individual daily reminder, to have access to an index of weekly or monthly reminders, to create a new reminder, and to logout. If the user clicks on the link to create a reminder they would have the ability to access a form that would save a reminder, associate a reminder to this user, and then persist it to the database. Once the reminders are saved to the database, the user will then have access to them either on their own individual show page or within the weekly or monthly index pages. The user will then have the ability to access an individual reminders show page and have the ability to edit or delete the reminder as they please. This gives the user the abilities to present to themselves the reminders they want and how they want them. They will ultimately have the ability to create, read, update and delete any of the reminders they want for their profile.

Once I have the basic flow of the program in my head, I then started to establish the objects. I would need to know how I would want these objects to be associated with one another. For this particular application, I saw it as having two classes: a user class, and a reminder class. The user would have a has_many relationship to the reminder class, and the reminder class would have a belongs_to relationship to the user class. These relationships would be represented in my database by assigning a foreign key to the class with the belongs_to relationship, in this case that would be the reminder class. Once the relationship is established, I would then assign the attributes I would want each class to have. For users I would want them to have attributes that I could use for authentication. So I assigned the user with attributes of username, email, and password. For the reminder class, I wanted each reminder to have content and then be categorized by whether they are daily, weekly, or monthly reminders. So I assigned each reminder attributes called note, that would represent the content, and another called frequency, that would represent whether they are going to be daily, weekly, or monthly reminders. Once I had the classes established, I then wanted to create my migrations to get the tables for these classes set up. I used ActiveRecord to do this.

```
class CreateUsers < ActiveRecord::Migration
  def change
    create_table :users do |t|
      t.string :username
      t.string :password_digest
      t.string :email 
    end 
  end
end
```

```
class CreateReminders < ActiveRecord::Migration
  def change
    create_table :reminders do |t|
      t.string :note 
      t.string :frequency 
      t.integer :user_id 
    end 
  end
end
```

Once I had the users table and reminders table represented the way I wanted, I made sure that I had my classes represented with the appropriate associations and validations. I needed the ActiveRecord macro 'has_many :reminders' within the user class, and the macro 'belongs_to :user' within my reminder class. I also wanted to include a set of model level validations. Methods #save and #update take into account these validations when running. They return false when a validation fails and will not persist the data to the database. So for the user I wanted them to be saved with all data fields present and I wanted their username and email to be unique to our database. This is helpful, because it keeps unwanted and empty data out of the database. Another thing I wanted present in my user class was the has_secure_password ActiveRecord macro, which gives us methods that can work with a gem called bcrypt. The bcrypt gem gives us the ability to store a salted and encrypted password to the database. So for whatever password the user gives us, we will never know what the password is, because bcrypt persists it to the database as a an encryption. This gives our user password protection if anyone was ever to access our database. In order for us to validate the password on a user's consequent login, we would need to establish the ActiveRecord macro, has_secure_password, and use the authenticate method. This method will allow us to acknowledge the users password without having any knowledge of password content. The authenticate method that comes along with the has_secure_password macro takes in the password that the user remembers and associates that content with the correct password encryption that bcrypt gave it initially. 

```
class User < ActiveRecord::Base 
  has_many :reminders 
  
  has_secure_password 
  
  validates :username, :email, presence: true 
  validates_uniqueness_of :email, :username
end 
```

```
class Reminder < ActiveRecord::Base 
  belongs_to :user 
  
  validates :note, :frequency, presence: true 
end 
```

So essentially the first step for designing this app was to get the model set up and the proper associations established. When the model is set up we can then persist data, and have access to this data for our controller actions.

So the next step after model completion, was to now get the controller set up. For this application I wanted a controller that could handle login, signup, and logout logic. I also wanted controllers that could handle user shows, edits, and deletions, and another controller that could handle reminder creations, shows, edits, and deletions. Within these controllers I wanted to make sure that I have validations that are only given to users that are logged in and to only allow users to see the reminders that are associated with them.

With these ideas in mind, I went ahead and established 4 controllers; applications, sessions, users, and reminders. The application controller is where I inherit from Sinatra::Base, configure  my settings, define the root url “/“, and include my helper methods. This is the controller that all the other controllers will inherit from. 

```
require './config/environment'
require 'sinatra/flash'

class ApplicationController < Sinatra::Base
  
  configure do
    set :public_folder, 'public'
    set :views, 'app/views'
    enable :sessions
    set :session_secret, "password_secret"
    register Sinatra::Flash
  end

  get "/" do
    if logged_in? 
      redirect "/users/#{current_user.id}"
    else 
      erb :welcome
    end 
  end
  
  helpers do
	.........
```

Once I had this controller established and inheriting from Sinatra::Base, I then wanted to start walking my way through the app. First, I wanted to see a welcome page with links. I set this up in the root url “/“ action to display the welcome page. The welcome page will then have links that will take you to login or signup pages that include forms. This is where I needed the sessions controller to include my login, and signup logic. So I included these get requests and url routes into my sessions controller. In this controller I wanted my login and signup pages to be able to be displayed. Then I wanted the actions of these forms to relay appropriate database interactions. If attempts proved to be successful it would send the user to their profile page or user show page. These controller actions would access the sessions hash that I set in the application controller. For this application, in order to access certain pages, the user would need to be logged in. Not until the user is logged in, will they have access. The login logic helps determine that the user is valid within our database, but since HTTP is a stateless protocol, the next controller action will have no idea who this particular user is, and then they would have to validate themselves for each and every page that is restricted. So for this app we needed a way for the user to navigate through the app without having to prove this validation on each page. This is why I enabled sessions. The session hash lives on the server and this gives the controllers the ability to access it anytime they need. So in order to remember the user as they navigate through the app, we could store information within this hash with a key value pair. Then every controller would have the ability to access this hash, and based off this hash key value pair, determine the user and that they are logged in. This would give the user the ability to navigate the logged in protected pages without having to validate themselves each and every time. For this app in order for the user to log themselves in, I set a key in the sessions hash called user_id and the value equal to the primary key that represents that user in the users table. So for controller actions that need to determine the user and/or if they are logged in, they can access the sessions hash for their answer. This hash is also accessed for the logout action. By simply clearing the hash, you validate that no user is present, thus nobody is logged in.

```
    def logged_in?
      !!session[:user_id]
    end 
    
    def current_user 
      @current_user ||= User.find(session[:user_id]) if session[:user_id]
    end 
```

This functionality and logic gives us the ability to validate users within the users controller and reminders controller. So before a user can be presented their profile page and have access to any CRUD action, they will have to be present in the hash. This allows our application to have fluid exclusivity. A user can easily navigate restricted pages without having to explicitly identify themselves each time.

The other validation that was important to include within this app was the validation that only a user could edit, or delete the reminders that belonged to them. I did not want any user to be able to just edit or delete other users information. This type of validation would need to check the sessions user_id key’s value and compare it to the foreign key of the reminder they were trying to access. This validation would prevent users from having access to information that did not belong to them. 

```
    def user_permission?(user)
      !!user ? user.id == current_user.id : nil 
    end 
    
    def reminder_permission?(reminder)
      !!reminder ? reminder.user_id == current_user.id : nil
    end 
```

Once I had the controller actions I wanted, I then worked on the views of this application. These views were pretty basic. I just wanted each view to display pertinent information, and correct form data. I wanted to have links on each page that allowed for easy navigation as well. 

Overall I enjoyed this project. I think it’s always a ton of fun to be able to see the information come to life, and even more fun to see that information display accurately with the logic you have given it. If you’d like to see the repository for this app here is the link: https://github.com/jackW50/sinatra-reminder-app.




