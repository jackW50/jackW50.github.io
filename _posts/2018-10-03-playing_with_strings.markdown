---
layout: post
title:      "Playing With Strings"
date:       2018-10-04 02:20:39 +0000
permalink:  playing_with_strings
---


When operating on string objets in Ruby, whether out in the open or nested away in an array or hash, I try to keep track of the methods that have helped me achieve certain specifications. These methods come with every string object, and are inherited from the String class. A couple of methods to note, that I seem to have to keep googling about are the ones that I will talk about here.

Frequently in the labs I have encountered the need to remove whitespace from strings. I’ve found this especially true when obtaining user input with the **gets** method. This is because when the **gets** method obtains input from the terminal it will return that information with a new line character attached to the end of the input. A couple of methods that I have used to remove this newline character have been **chomp** and **strip**. Both of these methods when called on the string will remove the newline character. I have used **strip** more often because it will remove both the leading and trailing whitespace, as opposed to **chomp** which will just remove the trailing aspect.

 For example the string, “ word\t“,  has whitespace before and after ‘word’. The need to remove whitespace is important, because that space actually counts as a character. If you were to test this, the length of “word” and “ word\t“ would be different. The “word” object has a length of 4, and the “ word\t“ object has a length of 6, because it counts the whitespace before and after the word. And if you were to compare the two objects, “word” == “ word\t“, the comparison would equate to false. So, in order to get the above comparison to work, and to make the length of the “ word\t“ object equal to the actual amount of letters present in the word, we would need to remove the whitespace. Calling **strip** on this string would accomplish this goal, but calling **chomp** would not.
```
one = " word\t“
two = “word”
	
one.length	
=> 6
	
two.length	
=> 4
	
one == two	
=> false 
	
one.strip	
=> “word”
	
one.chomp	  
=> “ word”
	
one.strip == two	
=> true
	
one.chomp == two	
=> false
```

A useful adaptation of **strip** is that it can also allow you to pick and choose which side of the whitespace you want removed. For, example, lets say you just wanted to remove the whitespace to the left of the word. You could do so by designating a prefix to the **strip** method to make your removal specific. The prefixes are used to specify which side you want removed. They are ‘l’ for left and ‘r’ for right. So if you want the “ word\t“ string to remove the left whitespace and return “word\t“ then we would call **lstrip** on the string. Lets say we wanted to just remove the right side, then we would call **rstrip** on the string and this would return “ word”.
```
one = " word\t“
two = “word”
	
one.lstrip 	
=> “word\t“
	
one.rstrip 
=> “ word”
```

Another frequent encounter when dealing with strings is when the string is containing a specific character or set of characters that I want to have replaced. For example if I encounter the string “I am a human”, and I want to remove human and replace it with “robot”. Now I could convert this string to an array, and remove the element, then replace that element with what I want at that index, then re-convert it back to a string. That just seems like a lot of work though, especially when I could just call a string method. This particular string method is called **gsub**, and I have found it very helpful. This method takes two arguments, the character or characters in the string you want removed as the first argument and then what you want that character or set of characters replaced with as the second argument. So from the first example, if I wanted to replace “human” with “robot”, then I would call this method on the string “I am a human”.gsub(“human”, “robot”), and this will return the string “I am a robot”. This is cool, but this method is even cooler because it can take regular expressions as an argument and you could identify specific patterns scattered throughout your string that you want replaced. Using a regular expression makes this very easy. For example, lets say we are given the string “jack-o-lattern_halloween” and we want all the hyphens and the underscore removed and replaced with just spaces. When using gsub we would use a regular expression to make this happen. 
```
 "jack-o-lattern_halloween".gsub(/[-_]/, " “) 
 => "jack o lattern halloween”
```
This method can also be applied in other ways as well. We can actually assign a block to it, or even place a hash as a second argument. Blocks can help us apply a particular behavior to each of the characters that we select. For example, given the string “Where are my vowels?”, we could use gsub to select all the vowels in the string and run a block of code to each particular vowel.
```
 “where are my vowels?”.gsub(/[aeiou]/) {|vowel| vowel.upcase} 
 => “whErE ArE my vOwEls?”

Calling this block of code on the vowels of the string will capitalize each vowel in the string. 
```
When using a hash as the second argument we can call multiple replacements to specific characters throughout the string. This can help replace sections of the string with specific values. The characters you want replaced are represented as the key and the replacement is represented as the value. For example, if using the string, “o1o oo1 1oo o11 o1o1oo1”, I want to substitute a hyphen in every space as well as replace every “o” with a ‘0’. I could do this using **gsub** with a hash as the second argument. 
```
 “o1o oo1 1oo o11 o1o1oo1”.gsub(/[\so]/, “ “ => “-“, “o” => “0”)
 => “010-001-100-011-0101001"
```
These are just a couple of the methods that have helped me out so far. I have found **gsub** in particular very useful and versatile. 

