---
layout: post
title: "Implicit versus Explicit Variable Type Declaration"
date: 2019-07-26
---

For those who are new to programming, implicit and explicit variable declarations can be a bit confusing. In reality, it's much simpler than we give it credit, and people tend to overthink it. Firstly, let's define a variable declaration. A variable declaration is the point in a program at which we define a variable. Often times a value is assigned as well, which typically looks like this format: ```declaration of variable = value assigned```. It is important to remember that there is a difference between the declaration and assignment, because in order to tell if a variable is declared explicitly versus implicitly, we need to pay attention to the declaration. An explicitly declared variable will specify the variable type within the declaration. An implicitly declared variable will assign a type based upon the assigned value. 
<hr>
<h2>Examples</h2>
```  
int x; //here we are declaring a integer variable named x.  
float pi = 3.14; //here we are declaring a float variable named pi, and also assigning it the value 3.14. This is an explicit type declaration.  
var z; //here we are declaring a variable named z, and allowing the variable type to be defined by a future assignment. This is an implicit variable declaration.
```  
Let's look at our first example, ```int x;```. This is an explicit type declaration, the type is not dependent on the value. We have explicitly stated that this variable will only hold an integer.  

In the second example, ```float pi = 3.14;```, we both assigned a value, 3.14, and declared a type, float. The type was explicitly declared before we assigned a value, and even if we passed it the value "5" it would be stored as a float, because we explicitly declared the variable type to be so.  

In the third example, ```var z;```, we are only declaring the value, and not assigning it a type. Var, however, allows the variable type to be set by the value we pass it. This is therefore an implicit variable declaration, because by looking at the declaration we have no idea what data type the variable will be.  
<hr>
<h2>Which is generally preferred?<h2>
Now, why should we attempt to use one versus the other? The most typical reason is clarity. Imagine you are writing a videogame, and you have your character's inventory handled by a list. In order to have such a list, you would have to declare it. You could do that as:  
```  
List <items> lstInventory = GetCharacterInfo(); //data type explicitly declared  
List lstInventory = GetCharacterInfo(); //data type implicitly declared
```  
GetCharacterInfo() is a function of some sort, and due to the vague name we do not know exactly what it is returning. If we explicitly declare the list as "hey this is specifically a list of items," the list is much more readable vs simply declaring the list. When you are working through a code base, it is much easier to have that sort of information handy, as the code base may be enormous and the clearer a code is written, the easier it will be for you and others to maintain. That being said, there are of course reasons to use implicitly defined variable types, and whatever provides the most clarity is probably the way to go.  

Powered by [Jekyll](http://jekyllrb.com)
