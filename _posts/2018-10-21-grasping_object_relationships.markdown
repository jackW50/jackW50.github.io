---
layout: post
title:      "Grasping Object Relationships"
date:       2018-10-21 16:46:06 +0000
permalink:  grasping_object_relationships
---


As I work through object relationships, I’ve come to realize that talking them out makes it easier to understand. Since I don’t have anybody to talk to at the moment, I figured I’d write them out. After working through some relationships involving an Artist, a Song, and a Genre, I decided to try to come up with my own object relationships example and my thought process behind it. This has been made easier when I try to visualize seeing the physical objects as they relate to one another in ‘real life’. In order to bring these objects to life in code, we need to create the classes from which they come. The classes are the creators, and along with creation comes behaviors. In the examples I believe they described the classes as the factory and the blueprint of objects, which makes sense. Each time an instance of the class is created, the instance object that comes from that class is automatically born with inherent behaviors that the class contains for the object. Once these objects are created along with behaviors they are then free to play and explore the world in which they live, and during this existence they can be brought in contact with other objects from other classes. How these objects interact depends on their relationship with on another.

In this example I will try to explain the three relationships that I have covered the most so far; “has many”, “belongs to”, and “through”. These relationships explain the logic behind the interaction of objects in existence.They try to mimic the real life relationship through code. For example, if I were to create an instance of an Artist I would want this particular artist to have the correct relationships it has with their songs and these songs genres. I would want these relationships to mimic real life. In order to do that I would want this artist to “have many” songs, and for these songs to ‘belong to’ a genre. I would not want the artist to ‘belong to’ a genre, just the songs the artist creates, but I would want the artist to have access to the songs information including the genre. This gives the artist the ability to have a relationship with the genre ‘through’ the ‘many’ songs it has. Understanding these relationships allows you to present them accurately through your code.

So, as I mentioned above, I tried to go ahead and mimic a real life situation through code as best I could. In my example I looked at a sports franchise or team. I tried to visualize the objects I wanted to see in code and mimic their behaviors and relationships. So when looking at a particular team I saw an owner for each team, I saw a team have players, and I saw a team have fans. So when compiling my classes I decided I would have an Owner class, a Team class, a Player class, and a Fan class. With the instance of each class will come behaviors and relationships that I will try to mimic from reality. Once the classes are set, I need to identify the relationships between these objects in their existence. So, for an owner to have a team, a team will thus ‘belong to’ an owner. Now, if the owner were very rich they may own more than one team, but for the sake of this example, I’m just going to just say that an owner only has one team. The team then ‘has many’ players, and it also ‘has many’ fans. The owner does not have any fans or players, rather the owner only has access to these objects ‘through’ its relationship with the team. A particular player also ‘has many’ fans, and a fan, especially if they were a bandwagon fan, can root for many teams and players, not just one player and one team. It’s also pretty clear that fans do not root for the owner and the players do not play for the owner, rather they play for the team, so there would be no need for the owner to have a direct relationship with the fans or the players. So now that we have the relationships figured out, how do we go about mimicking this through our code?

When creating a ‘belongs to’ relationship, I will assign a particular attribute to contain this data. That way if you ever need to ask the object ‘who do you belong to?’, it can reply back with the information. In order to display it in code it would look something like this:
```
team.owner = owner
owner.team = team

```

This code will assign an owner attribute to the Team class(@owner) and a team attribute to the Artist class(@team) to store this information. Depending on where you assign this information will depend on the code, but looking at it in real life, I would like to call a new instance of an owner (Owner.new), with arguments that will assign the owner with a team, because in real life you cannot be an owner without a team, so in order for an owner to be created we must have a team argument to go with it. I could even create a class constructor to include more specifics, and I will do this later on, but for right now I just want to get my relationships working and for them to resemble ‘real life’.
```
class Owner
  attr_accessor :team
	
  def initilaize(name:, team:)
    @team = team
    @name = name
  end 
	
end 

```

For the team object, I think the team could be initialized with an owner or without an owner. Some teams in real life could exist with ownership in flux. I think you could still be a team without an owner temporarily, so in order to show this in code I would set the default owner attribute to nil unless explicitly specified.
```
class Team
  attr_accessor :owner

  def initialize(name: owner: nil)
    @owner = owner
    @name = name
  end 

end

```

This relationship allows these objects to have their proper real life relationship mimicked through code. If you asked the instance of team who its owner was, and information was assigned to the owner attribute, it would return that information.
```
mavericks = Team.new(name: "Dallas Mavericks")

mark = Owner.new(name: "Mark Cuban", team: mavericks)

mavericks.owner= mark

mark.team.name
=> "Dallas Mavericks"

dallas.owner.name
=> "Mark Cuban"

```

The objects have a direct relationship with one another and could have access to the information stored in each object if specified.

The next relationship I wanted to mimic was the relationships between the team and the fans or the team and the players. This is the same type of relationship with both; the ‘has many’. The only difference between the fan and the player object would be their relationship to the team. The player would ‘belong to’ a team. They could only play for one team, because thats the requirement under their contract. The fan on the other hand, could be a fan of many teams, so they could have a ‘has many’ type of relationship to the particular team. This type of relationship shown through code would look something like this:
```
class Team 
  attr_accessor :team
	attr_reader :players, :name
	
	def initialize(name:, team: nil)
	  @players = []
		@team = team
		@name = name
 end


 def  add_player(player)
    players << player
 end
 
end
```

The team would have many players, so it would store these players in an array. Creating an attribute @players to do this would give this object the ability to have this type of relationship. Now for the player, they could only belong to a team, so just creating an attribute to store the one team like the ‘belongs to’ relationship example above would suffice.

```
class Player
  attr_accessor :name, :team
	
end
```

For the relationship of the fan to the team would be very similar to the player to the team.

```
class Team
….
  def initialize(name:, owner: nil)
    @fans = []
    @players = []
		@owner = owner
		@name = name
  end

  def add_fan(fan)
     fans << fan 
  end 
	
end

```

This allows the team to have ‘many’ fans, which a team in real life would have. The code mimics the real life relationship. The difference between the player and the fan though is that the fan has the ability to have many teams while the player does not. So the team would have a different type of relationship to the team.
```
class Fan
  attr_accessor :email, :teams
	attr_reader :name

  def initialize(name:, fav_team:, email: nil) 
    @teams = []
		@players = []
		@name = name
		@email = email
		add_team(fav_team)
  end 

  def add_team(team)
    teams << team
  end 
	
end

```

This code allows the fan to have many teams by storing the teams they root for in an array. They could also be a fan of many players, so similar code could be written to mimic the fan having many players and vice versa. The fan however does not root for the owners, just the team and players, and the owner does not have fans. However, lets say the owner wants to know how many fans his particular team has in order to project ticket sales for the upcoming season. The owner could have access to this information ‘through’ the team. So the owner would have to look into the team object for this information. It can be shown in code as:
```
class Owner
…..

  def team_fan_numbers
    team.fans.count
  end 

end

```

This code would use the owners team attribute that stores the object of the team that has the array of fans in it’s fans attribute. This array of fans would have fan objects in a list format. These fan objects could also contain information about each fan of the team. So lets say for instance of the fan has a given email address, and the owner wanted to have access to each fans email address in order to send out a flyer on the upcoming season schedule, then the owner could have this access again through the team, and the code could look something like this:
```
class Owner
…..

  def team_fan_emails
    team.fans.collect do |fan|
	    fan.email
    end
		
end 

```

This code would give the owner access to every fans email in an array. So if the owner wanted to send out an email blast about the upcoming schedule to every fan of the team, the emails could be found by calling the #team_fan_emails on the owner instance. This type of relationship is made possible by the ‘through’ relationship between the owner and the fan, and it’s real life relationship is mimicked through code.

These relationships can continually expand into other methods. Expanding on the code to include more behaviors could require these relationships to be used. For example, what if we wanted the team to draft a player. If the team drafted a player, wouldn’t we want a new player instance to be made on the spot, and to have this player be added to team that drafted him or her, and to have the player assigned to the team that drafted him without having to explicitly tell the player that he or she now plays for this team. We can do this through code in the team class, because the particular team should have the behavior to draft players. You can do this in different ways, but for this example, I am going to instantiate a player within the team instance method. In the player class, I will allow the player to be initialized with a team. This will help give the player access to the team once the player is instantiated in the method. Then I will push this player instance into the players array of the team. This method uses the ‘belongs to’ relationship by assigning the player to a team, and the ‘has many’ relationship by adding the player to a list of many players on the team.
```
class Player
  attr-accessor: :team
  attr_reader :name

  def initialize(name:, team: nil)
    @name = name
    @team = team
  end 
end 

class Team
…….
   def draft_player(name:)
      Player.new(name: name, team: self).tap do |player|
        players << player
      end
   end
	 
end

```

In this code I gave the team more behavior, and when I gave it more behavior I allowed it to use it’s relationships so I didn’t have to explicitly tell it what to do. For example I didn’t have to tell the player directly which team they now belong to and I didn’t have to explicitly tell the team to store this new player in the players array. I did this with the method call on a team to draft_player all at once, and the ability for these objects to have a relationship made this possible.

So I went ahead and created the classes added some behavior, and played around with them in irb. I wanted to see what it would look like, and to think about what I would like for it to do in the future. Some things I’m not sure on yet, is how to get two teams to make a trade, and how an owner could sell their team to another owner. But here are my classes at the moment and playing around with them has been pretty cool so far. Currently my team can draft players, sign players, have access to their roster, cut players, have fans, and add fans. My owners can buy teams, create teams, access the players info, and obtain the teams fans emails. My fans can add players and teams for which they can root for.

```
class Owner
  attr_accessor :team
  attr_reader :name

  def initialize(name:, team:)
    @name = name
    @team = team
  end

  def self.buy_team(name:, team:)
    Owner.new(name: name, team: team).tap do |owner|
      team.owner = owner
    end
  end


  def fans_emails
    team.fans.collect {|fan| fan.email}
  end

  def create_team(name:)
    Team.new(name: name, owner: self).tap do |team|
      self.team = team
    end
  end

  def team_players
    team.players
  end

  def team_fans
    team.fans.collect {|fan| fan.name}
  end

end

class Team
  attr_accessor :owner
  attr_reader :name

  @@teams = []

  def initialize(name:, owner: nil)
    @name = name
    @fans = []
    @players = []
    @owner = owner
    self.class.teams << self
  end

  def self.teams
    @@teams
  end

  def fans
    @fans
  end

  def add_fan(fan)
      fans << fan unless fans.include?(fan)
      fan.teams << self unless fan.teams.include?(self)
  end

  def players
    @players
  end

  def draft_player(name:)
    Player.new(name: name, team: self).tap do |player|
      players << player
    end
  end

  def sign_player(player)
    players << player unless players.include?(player)
    player.team = self
  end

  def roster
    players.collect {|player| player.name}
	end

  #def trade_player
  
	def find_player_by_name(name:)
    players.detect {|player| player.name == name}
  end

  def cut_player(name:)
    players.delete_if {|player| player.name == name}
  end

end

class Fan
  attr_accessor :email
  attr_reader :name, :teams, :players

  def initialize(name:, fav_team:, fav_player:, email: nil)
    @name = name
    @teams = []
    @players = []
    add_team(fav_team)
    add_player(fav_player)
    @email = email
  end

  def add_team(team)
    teams << team unless teams.include?(team)
    team.fans << self unless team.fans.include?(self)
  end

  def add_player(player)
    players << player unless players.include?(player)
    player.fans << self unless player.fans.include?(self)
  end

end

class Player
  attr_accessor :weight, :vertical, :points_per_game, :team
  attr_reader :name, :height

  def initialize(name:, height: nil, weight: nil, team: nil)
    @name = name
    @height = height
    @weight = weight
    @team = team
    @fans = []
  end

  def fans
    @fans
  end

end

```
