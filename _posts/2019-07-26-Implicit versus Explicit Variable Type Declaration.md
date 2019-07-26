---
layout: post
title: "Implicit versus Explicit Variable Type Declaration"
date: 2019-07-26
---

For those who are new to programming, implicit and explicit variable declarations can be a bit confusing. In reality, it's much simpler than we give it credit, and people tend to overthink it. Firstly, let's define a variable declaration. A variable declaration is the point in a program at which we define a variable, often times we also assign a value as well, which can look something like this: ```declaration of variable = value assigned```. It is important to remember that there is a difference between the declaration and assignment, because in order to tell if a variable is declared explicitly versus implictly, we need to pay attention to the declaration. An explicitly declared variable will specify the variable type within the declaration. An implicitly declared variable will assign a type based upon the value assigned to it. Here are some examples:   
```  
int x; //here we are declaring a integer variable named x. This is an explicit type declaration, the type is not dependent on the value.  
float pi = 3.14; //here we are declaring a float variable named pi, and also assigning it the value 3.14. This is an explicit type declaration as well; though we did assign a value, the type was assigned by the declaration and not the value.  
var z; //here we are declaring a variable named z, and allowing the variable type to be defined by a future assignment. This is an implicit variable declaration as the variable type is dependent on whatever value is assigned.
```  
Now, why should we attempt to use one versus the other? The most typical reason is clarity. Imagine you are writing a videogame, and you have your character's inventory handled by a list. In order to have such a list, you would have to declare it. You could do that as:  
```  
List <items> lstInventory = GetCharacterInfo(); //data type explicitly declared  
List lstInventory = GetCharacterInfo(); //data type implictly declared
```  
GetCharacterInfo() is a function of some sort, and due to the vague name we do not know exactly what it is returning. If we explicitly declare the list as "hey this is specifically a list of items," the list is much more readable vs simply declaring the list. When you are working through a code base, it is much easier to have that sort of information handy, as the code base may be enormous and the clearer a code is written, the easier it will be for you and others to maintain.  

Powered by [Jekyll](http://jekyllrb.com)
