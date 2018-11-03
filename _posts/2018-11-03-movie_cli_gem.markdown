---
layout: post
title:      "Movie CLI Gem"
date:       2018-11-03 17:50:54 +0000
permalink:  movie_cli_gem
---


For my first gem I created a movie command line interface gem. The first step to creating this gem involved getting organized. I started with an overall idea of how I wanted the gem to work. I wanted this gem to interact with a user. In order for this program to work it would need to obtain a particular piece of data from the user. Then I wanted to use that data to obtain information that I could then return and display back to the user. In this particular scenario, I wanted to return to the user nearby theaters and list the movies at each theater. In order to make this interface work, I would need location data from the user.

The first step of this gem was to obtain information, because how would I return to the user nearby theaters if I had no idea where that user is located. So the first step was to figure out where the user is located. This would occur in the first step of the application, and in order for the program to run further the user would have to enter their zip code. This location would give me what I needed to find the appropriate web page to obtain information. The users input will allow the program to progress.

With having the basic flow of the program in mind, I then needed to figure out how I was going to go about doing what I actually wanted to do. I decided to start slow and not jump the gun. Understanding the application and seeing it in my head was one thing, but to actually code it was another. So first things first, I decided to understand how this program was going to communicate. I wanted my classes to contain information and behavior that seemed appropriate and made sense. From the flow it seemed that once the data was obtained from the user I would then need to act on that data, use it to create objects, then display specific information stored in these objects. So I would need an object that could get the appropriate data I needed, and in this gem it was to obtain local theater information from a webpage. Then I would need an object or objects to create new objects from this data. Once these objects are created, I would then want these objects to be able to display stored data to the user. For this project I would want local ‘Theater’ information and local ‘Movie’ information. I would want the theater and movie information to be able to be stored and then called on to be displayed, so I would want these objects to have attributes I could set and get when I needed. With this logic in mind, I saw my project as having 3 class objects. One to obtain information from a webpage that I called 'a class Scraper', one where I could instantiate, store, and display theater information called 'class Theater', and one that could instantiate, store, and display movie information called 'class Movie'.

Once I had an idea of the classes I needed and what I wanted them to do, I then started with the next step of the program; I defined all my classes. Then I started with a step by step manner. The first step was to scrape the website. We can’t create objects and display information without first getting the information we need. So I decided to set up my Scraper class.

The Scraper class involved using the users zip code with a URL to find location-specific information about that webpage. The zip code gives access to the local theaters within the users area. For this project I used the zip code along with the url “http://www.movies.com" to create a location-specific URL. This URL then was used to get the data I needed for the project. The Scraper class would use this URL to obtain information from that webpage.

For this project I used the Open_URI module, and the Nokogiri gem. Open-URI is a module that allows you to make HTTP requests. The request I used for this purpose was the ‘open’ request which will take in an URL and return to you the HTML content of that URL. This HTML content will be a string, and this allowed me use Nokogiri to parse and collect the data I needed. Then once I had the data I needed, I would then use this data for my next step.

Here is how I used Open-URI and Nokigiri in a method within my class Scraper.
```
require 'nokogiri'
require 'open-uri'

class Scraper
  ...............
  def theater_scraper
    html = open(path)
    doc = Nokogiri::HTML(html)
    doc.css("ul.theaterList li")
  end
end 
```

The next step would then use this information to create Theater and Movie objects. For ‘each’ theater object in mind I wanted it have certain information and to be able to display that information. For this application I wanted that information to be the theater name, theater location, and the list of movies the theater had along with their showtimes for that theater.

So for the theater class I kept in mind what behavior I wanted it to have and assigned it the appropriate writer and reader methods.  I also wanted a custom constructor that I could use to instantiate new objects from a data set that the Scraper class would obtain for me. I also wanted it to feature access to ‘each’ theater object. So I needed the program to have a way of knowing about all the theaters that it creates. I assigned this responsibility to the theater class itself by giving it a class variable that would save each instance of a theater instantiated into an array. This class could then give access to this information through a class reader method.

```
class Theater
  attr_accessor :name, :location, :movies
  
  @@all = []
  
  def initialize(attributes)
    attributes.each {|key, value| self.send(("#{key}="), value)}
    self.class.all << self 
  end 
  
  def self.all 
    @@all 
  end 
  
  def self.create_from_scraper(theater_hash)
    theater_hash.each do |key, value|
      Theater.new(value)
    end 
  end
end 
```

In this gem I also wanted the movies that each theater had to actually be movies, and not just movie strings. I wanted these movies to have properties and to be able to display these properties when called upon. So I created a Movie class that could do this. So every movie that got instantiated would have a name, and it would have showtimes. To display this in the code I instantiated every movie object with a name, and showtimes and stored it within the theaters movies instance variable. Each theater has many movies so I stored all these movie instances and their attributes to an array and assigned this to the movies instance variable inside the theater class.

```
class Movie 
  attr_accessor :name, :showtimes 
  
  def initialize(name, showtimes)
    @name = name 
    @showtimes = showtimes 
  end 
end 
```

Once I had the classes coded the way I wanted to, I then had access to the information I wanted by just iterating through the Theater class variable. Creating how to go about making this display work made me think about how I wanted it to interact with the user. I was going to put it in step by step manner in one of my bin files, but instead I decided to create a separate class called Movie_Cli to create methods that could display this information for me. In this class I wanted methods that could be called on an instance of Movie_Cli to interact with my user. These methods would involve greeting the user, obtaining a zip code from the user, scraping the website, parsing that scraped information, using that scraped information to create objects, and then to display that information back to the user by accessing it through objects. 
