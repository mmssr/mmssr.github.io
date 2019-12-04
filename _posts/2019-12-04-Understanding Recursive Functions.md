---
layout: post
title: "Understanding Recursive Functions"
date: 2019-12-04
---

Recursion within the context of computer science is when a function calls itelf. Typically, this is done to take a relatively complex problem which is solved via repeated steps, by applying the same logic against repeated iterations. Let's look at an example of a recursive function:  
```

/* this program will accept a string and remove duplicate adjascent values */
public String stringClean(String str) {
  /* return if there are less than 2 chars, no adjascent chars */
  if (str.length() < 2) {
    return str;
  /* else if first char matches second char, return from index 1 onwards
     to drop the matching character and then rerun recursively */
  } else if (str.charAt(0) == str.charAt(1)) {
    return stringClean(str.substring(1));
  /* else, first char must not match, so return index 0, recursively run
     from index 1 onwards */
  } else
    return str.charAt(0) + stringClean(str.substring(1));
}

```  
Notice how stringClean(), the function we defined, calls itself within itself. This breaks up the problem into smaller and smaller problems, allowing us to solve the issue via the repeated steps. The simplest way to view this is with a flow chart:  
![Recursion Flow 2](/assets/recursion2 flow.PNG)  

Here is another example, the famous fibonnaci problem: for int n, return the nth fibonnacci number with n=0 representing the sequence beginning.  
```

public int fibonacci(int n) {
  if (n == 0) { // returns 0 because it cannot go any lower
    return 0;
  } else if (n == 1) {
    return 1; // returns 1 because it cannot go any lower
  } else // actual recursive call below, breaks down main problem to branching smaller problems
    return fibonacci(n - 1) + fibonacci(n - 2);
}

```  
See how the final else breaks down the problem into multiples of the function? You can follow this program path as well:  
![Recursion Flow 1](/assets/recursion1 flow.PNG)  

Powered by [Jekyll](http://jekyllrb.com)  

