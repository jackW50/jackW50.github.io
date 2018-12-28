---
layout: post
title:      "Active Record Retrieval"
date:       2018-12-28 05:02:40 +0000
permalink:  active_record_retrieval
---


Object Relational Mapping (ORM) connects the objects of an application to a database. When using an ORM we are able to persist objects and their appropriate associations to tables. We can then call on these tables to return to us instances of these objects. Once we have these objects and the data they contain, we can then use them within our application. Active Record is an ORM framework that can retrieve data without having to write SQL statements directly. 

When it comes to object retrieval, Active Record provides you with a variety of finder methods that can help you accomplish your retrieval. Each finder method allows you to pass arguments into it to help you perform specific queries. Using these methods in different ways can return to us very specific information.

The **find** method is a method you can call on a class. Passing this method an argument of a primary key returns to you an instance of the object that matches that primary key. Each instance of an object can be stored as a row in a table and this row is represented by a unique primary key. You can also pass in an argument of an array of primary keys and get returned multiple instances that match those keys in an array. Important note for this method is that when you pass in a primary key that is not found in the database, it will raise an **ActiveRecord::RecordNotFound exception**.

The **first** method is a finder method that returns the first record in the list as an object. This method’s default setting is based off the records being ordered by their primary key in ascending order. So if you're just calling this method on a class without an argument, it will result in this method returning the instance with the primary key or id of 1. If you pass this method a number argument, it will return the records up to that number as instances. So if you pass it 4, this method will return the records with the primary keys 1-4 in an array of instances. This method can also be used for other ordered collections if specified as such. If you were to order the list in alphabetical order, then the first method would return the record with the lowest alphanumeric score regardless of the primary key. It is also important to know that this method returns **nil** when no record is found.
```
@movie = Movie.order(:name).first 
```
The **last** method works very similarly to the first method, except that it returns to you records starting at the other end of the spectrum. Rather than returning from the first position in the list, it returns from the last position in the list. By default it returns the last record ordered by primary key. Just like the first method you can pass it a numerical value and it will return you that number of records as instances, but instead of starting from the front of the list, it starts from the end. If the order of the collection is specified, then the method will return the last item of that specified order. This method also returns **nil** when no record is found.

By specifying the order of how you want your objects to be returned you can use the **order** method. Using this method along with the other finder methods can allow you to do some pretty cool things with the data you are accessing. This method takes in an argument of the column name for which the objects are to be ordered.

So let’s say you have an application that shows movie information. This application can add new movies to the database with names and a star ranking represented as a number 1-10. 1 being the lowest amount of stars and 10 being the highest amount of stars. As you begin to review movies, you add them to the database. Each movie is saved to the movies table in a row with a unique primary key value represented as an integer, a name represented as a string, and the star_ranking you assign to the movie represented as an integer. As the movies get persisted the primary keys of each movie saved to the database will increment by 1. So the first movie you save will have a primary key of 1, the second will have a primary key of 2, the third will be 3, and so on. Once you have these objects persisted in the table as records, you can then get access to this data through these finder methods that Active Record gives us. Using these methods in specific ways can return to us the information that we want to present to the user through a view. This can be fun. So from the methods that I have talked about, I could use them within a controller method to retrieve pieces of data that I can put into an instance variable, and then use this variable in my view that the controller action may display.

For example, if I wanted to have a page that lists my top ten movies, I would need to display a page that shows the accurate information in a reader friendly way. Before I display this information I need to access the information from my database. Accessing it using the Active Record finder methods can make it very easy. To get my top ten movies I would need to find the 10 movies within the database that have the highest ranking. I could do it in a variety of ways, but in this example I will use the order method and the last method.
```
@movies = Movie.order(:star_ranking).last(10)
```
The order method orders the records based off their star ranking, which is an integer 1-10. This orders the movies in ascending order with the lower ranking movies appearing first and the higher ranked movies towards the end. In order to get the movies with the highest ranking, we will want the ones from the end of this list. I used the last method and not the first method. The last method when passed a numerical value will return the number of these records starting from the end of the list. So when passing it 10, it gives us the 10 movies starting from the end. This returns to us the 10 movies with the highest star ranking. Within a controller action we can store this information in an instance variable. We would have the controller action display a view, and this view would then have access to this information through the instance variable. We could use the variable to show a list of the highest ranked by embedding it within our html.
```
	<% @movies.each do |movie| %>
		<ul>
			<li><%= “#{movie.name}: #{movie.star_ranking}%></li>
		</ul>
	<%end%>
```
If we wanted to use the first method with the order method to get the same return value we would have to specify the direction of order. By default this method orders in ascending order, but you can specify it to order in descending order with DESC like so:
```
@movies = Movie.order(star_ranking: :desc).first(10)
```
This retrieval will return the same thing as using the order method with the last method.

Using these methods provided by Active Record allows us to retrieve data without having to write SQL statements. The equivalent SQL statement from the above Active Record retrieval query would be:
```
sql = <<-SQL
	SELECT * FROM clients ORDER BY movies.star_ranking DESC LIMIT 10
SQL
```
The Active Record finder methods can be easier to use, and they may save you time when having to write retrieval queries. 

The **find_by** method matches the first record that matches a certain amount of conditions that are passed in as an argument. This method can be useful if you want to just find one record that matches specific conditions. The SQL equivalent to writing this would be using the keyword WHERE with a LIMIT to 1. This finder just returns one record WHERE certain conditions are met. When no record is found based off certain conditions, this method will return **nil**.

So from the movie example above, if the user did a search by a particular movie name, we would want to return to the user the movie that matched the name they typed. We could go into our database to get the information we need.
```
@movie = Movie.find_by(name: “Fight Club”)
```
The argument we pass the method is the condition that must be met. So for this retrieval we want to get the movie that has a name of “Fight Club”. To make this more dynamic, and more realistic, the user would probably be entering their search data into a html form. This form will be submitted with an action(url) and a method(POST). The forms data will be sent to the appropriate controller action via the action and method. Within this controller action the form data will be able to be accessed via a hash called params. So when we write our finder method we could include the params key that will give us the value the user typed in. The more dynamic version would look like this:
```
@movie = Movie.find_by(name: params[:name]) 
```
The SQL equivalent statement would be:
```
sql = <<-SQL
	SELECT * FROM movies WHERE (movies.name = params[:name]) LIMIT 1
SQL
```
The find_by method works great if we just want to find one record that meets certain conditions, but what if we want to find multiple records, and what if we want to chain other methods to our search? If we wanted to do this we can use the **where** method. It is similar to the find_by method and represents the WHERE of the SQL statement. When no record is found both these methods will return **nil**. The difference between the where method and the find_by method is that where will return multiple records unless specified and you can chain other methods to this retrieval. The where method takes an argument of conditions to be met on your query. Conditions can be specified as a string, array, or hash. It is preferred to build your conditions as an array or hash, and not as a string.
```
@movies = Movie.where(name: params[:name]).first
```
This method call will return to us the same value as the find_by method above. We chain the first method to the end of this call to return to us just the first record that matches this condition. The find_bymethod and the where method can take the same condition, but the find_by will return to us the first record match implicitly, and the where method will return to us all the records that match the condition. So in order to just get the first match we have to explicitly tell the query to return just the first record that matches the condition. 

Let’s say we want to return the greatest movies of all time. Our movie database has several movies with a 10 star ranking. We want to list all these movies to the user, however if we queried our database using the find_by method we would only get in return the first movie that matches the condition (star_ranking: 10). This would not be what we need. We would want to use the where method to return to us all the records that match this condition.
```
@movies = Movie.where(star_ranking: 10)
```
This method returns to us a list of movie objects that have a star ranking of 10. The where method helps us return all the records that match a certain condition. This method also allows us to chain other methods onto it to make our search more specific. 

From the methods above our return values are records re-instantiated as objects. These objects have all the information within that row that they contain. These objects can be called on to access that information within that row. But what if we didn’t need all the information from these rows, and only need a specific subset of it. For example in SQL the * after the SELECT represents everything in the row. In order to just access a specific column within that row, rather than all the columns, we can specifically state the table and which column of the table we want returned. For example the SQL to select just the movie.names from the database would be:
```
sql = <<-SQL
	SELECT movies.name FROM movies
SQL
```
If we were to write this selector in Active Record we would use the **select** method and pass it the argument of the column name. Like so:
```
@movie_names = Movie.select(:name)
```
This method will still return to us re-instantiated objects, but these objects will not have all the information from the row, just the column in the row that you specify. If there is no column within the table that you are calling on, then you will get **ActiveModel::MissingAttributeError**.

Active Record offers many different methods that can mimic SQL retrieval statements. I just mentioned a few that I have been using so far. There are many more. I’m still figuring out how to use the methods to achieve specific purposes. I think the major advantages to these methods are that they are easier to write and easier to read. Using them may save you some time in the long run.

