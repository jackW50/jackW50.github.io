---
layout: post
title:      "HTML Forms"
date:       2018-12-11 03:45:35 +0000
permalink:  html_forms
---


As I’m starting to dig into MVC frameworks, I’m beginning to see how important forms are. Forms give us a way of getting information from the user. If our application is going to do something of value for the user, we need information from that user. Forms give us a way of accessing this information. For example, if a user wants to create a new profile on a social website, he or she would need to give us information. We would not be able to create an accurate profile for the user unless they give us information about who they are. The information will allow us to perform a task that is useful. So in order to get the appropriate information, we can use a form. We would have the user enter the appropriate data, and then the form would take that data and trigger the appropriate requests within our app. This interaction is a crucial step for us to determine what the user wants. 

Forms can be displayed in different ways, and can have many different ways of accessing information from the user. For the purpose of this blog post I am going to examine some important form elements, and their attributes. Writing out these element and describing them helps me see when and how to use them. As it pertains to the MVC framework, forms will be part of our view(V). We include them in our HTML and allow users to have direct access to them through their browser.

<h3>Action & Method</h3>
The two most important pieces of information when creating a form are its action and its method. These attributes are contained within the opening form tag, and they are very important because without them, the form is completely useless. The form does not know what to do unless we specify through these attributes. The action tells our form where to send the information. Typically this is a URL of a remote server. 

The method attribute specifies the HTTP method the form will take. It tells the action what to do. The two main methods used for forms are POST and GET. 

In the MVC, the action and method of the form are typically recognized by our controller(C), and the controller will recognize these attributes with a route and initiate the appropriate action.

```
      <form action='/this-is-an-example-url' method='POST'>
        <label for="user_name">Name:</label>
        <input id="user_name" type="text" name="name" placeholder="name">


        <input type="submit" value="submit">
      </form>
```


<h3>Form Displays</h3>
Once the action and the method attributes are specified, we can then begin to design the type of form we want and how we want it displayed. There are several HTML elements designed just for forms. Some elements are designed for how to get the input, and others are designed more for display. All the tags pertaining to the form will be placed within the opening and closing form tags. You can use multiple form tags on a page if you need to.


The ways to get information through a form are specified by the elements within the form tags.
Some common ways to get input from the user is to have them type specific information directly or to click and make selections from a menu of different options.


The **input** element is a diverse element, because it can specify many different displays through it’s type attribute. Changing the type attribute within an input element can display many different form options.

```
      <form action='/this-is-an-example-url' method='POST'>
        <label for="user_name">Name:</label>
        <input id="user_name" type="text" name="name" placeholder="name">


        <input type="submit"value="submit">
      </form>
```


The type attribute of this input element is very dynamic. In the example above it indicates ‘text’, and a text box will appear and the user can type in their data. The value of the input is what the user types in. The form above also has a type attribute that indicates ‘submit’, which will display a button that the user can push when they are done to submit the form. Other type attributes include: ‘button’, ‘password’, ‘tel’, ‘hidden’, ‘radio’, ‘checkbox’, ‘number’, ‘range’, ‘submit’, ‘date’, ‘datetime-local’, ‘email’, ‘color’, ‘file’, ‘image’, ‘month’, ‘reset’, ‘search’, ‘time’, ‘url’, ‘week’. Different types can be used in different contexts to get information we need for our application.

Having menus that users can select from is a common technique that can be user-friendly. When creating options, the values are assigned and then presented to the user in a list to pick from. This design can make it easier because it cuts down on having the user type in text, and the values can be made specific. The drawback is that it may not have the options that user wants, and there is no way for them to specify. 

When using the input element, we can use two popular type attributes: ‘radio’ or ‘checkbox’ to create lists of options that the user can click on.

```
      <h2>Which Hand Are You?</h2>
      <form action='/this-is-an-example-url' method='POST'>
        <input type="radio" name="hand" value="left">Left Handed
        <input type="radio" name="hand" value="right">Right Handed
        <input type="radio" name="hand" value="ambi"> Ambidextrous
        <input type="submit" value="Click Here">
      </form>
```


Using the type attribute radio will display a list of options. The options are headings with a little circle dot that you can click on to select the option. So for the HTML above there will be 3 options that you can pick from to indicate what your dominant hand. The type attribute ‘radio’ will only allow you to select one option though. This can limit it’s use, especially for lists that might involve you selecting multiple items. If we wanted to represent a list with the capability to select multiple items, we could use the input element with type attribute ‘checkbox’.

```
      <h2>How Do You Exercise?</h2>
      <form action='/this-is-an-example-url' method='POST'>
        <input type="checkbox" name="exercise-a" value="treadmill">Treadmill
        <input type="checkbox" name="exercise-b" value="elliptical">Elliptical
        <input type="checkbox" name="exercise-c" value="yoga">Yoga
        <input type="checkbox" name="exercise-d" value="weights">Weight Training
        <input type="submit" value="Submit">
      </form>
```

Using the attribute type checkbox will display a list of options with boxes you can check off when these options are a match. This type of display will allow the user to select multiple options at once as opposed to just selecting one option as is the case in type ‘radio’.



The **select** element is another element that will allow you to pick from a list of options with values. Select defines a drop down list of options you can choose from. Within the opening and closing select tags are option tags that specify each option. Select opening tag usually assigns an attribute called name. Then within the opening and closing tags the option elements will specify that values that can be chosen.

```
    <h2>Shoe Size</h2>
    <form action='/this-is-an-example-url' method='POST'>
      <select name="shoe-size">
        <option value="8">8</option>
        <option value="9">9</option>
        <option value="10">10</option>
        <option value="11">11</option>
        <option value="12">12</option>
      </select>
      <input type="submit" value="done">
    </form>
```

By default, the first item selected in the drop-down list is selected. To allow the user to select more than one you can use ‘multiple’ within the opening select tag.

```
    <h2>Shoe Size</h2>
    <form action='/this-is-an-example-url' method='POST'>
      <select name="shoe-size" size="5" multiple>
        <option value="8">8</option>
        <option value="9">9</option>
        <option value="10">10</option>
        <option value="11">11</option>
        <option value="12">12</option>
      </select>
      <input type="submit" value="done">
      <p>*Hold down command key to select multiple options</p>
    </form>
```



To include a list of pre-defined options into an input element where you could either enter text or pick from the list of options you could use the **datalist** element.  In order to use the datalist element you will need to assign the input element with a list attribute and assign the datalist element with an id. The input will include the datalist if the id is the same as the list attribute. The datalist element has option elements that define the value for each option. So the resulting display of using a datalist element with an input element will be a text field that include a drop down menu of pre-defined options. It gives you two options, either type in what you want, or select from the list.

```
    <h2>Language</h2>
    <form action='/this-is-an-example-url' method='POST'>
      <input list="languages" name="language">
      <datalist id="languages">
        <option value="Spanish">
        <option value="French">
        <option value="English">
        <option value="Italian">
        <option value="German">
      </datalist>
      <input type="submit" value="send">
    </form>
```



For entering small amounts of text using the input element with type attribute equal to ‘text’ is sufficient, but for entering large amounts of text that can span multiple lines, a text area should be used. This element will allow you to create a box that can take in text that is larger than one line. In the textarea opening tag,  you can specify the size of box. The two attributes that are used to do this are rows and cols. The rows attribute specifies the visible number of lines in a text area, and the cols attribute specifies the visible width of a text area. You can also design the size of the text area using CSS .

```
    <h2>Daily Summary</h2>
    <form action='/this-is-an-example-url' method='POST'>
      <textarea name="summary-text" rows="15" cols="40">enter text here</textarea>
      <input type="submit" value="submit"
    </form>
```

 *To see the forms display render the HTML written in the examples above.
 [http://htmledit.squarefree.com/](http://)




</br>
Source:
1. https://www.w3schools.com/tags/tag_input.asp
2. https://www.w3schools.com/html/html_form_elements.asp
