## Regular Expression in Python
  
### Tags:
>```"."```   
 
Replace with any character   

>```"*"```    

Repeating character  

>```[]```  

Take the place of multiple options or defining range. ```[A-Za-z]*```

>```+```  

Repeate the character ```"Hello2441139" -> "[0-9]+ -> 2441139```

>```\w```

For representing ```[A-Za-z-]```


>```\s```  
  
 White space character space, tab, new line.
 
 
 ### Examples:
 > Example 1:  

A top-level web address, meaning that it contains alphanumeric characters (which includes letters, numbers, and underscores), as well as periods, dashes, and a plus sign, followed by a period and a character-only top-level domain such as ".com", ".info", ".edu", etc.   
```
pattern = r"^[\w.+-]*\.[A-Za-z]*$"

"gmail.com" > True
"www@google" > False
"www.Coursera.org" > True
"web-address.com/homepage" > False
"My_Favorite-Blog.US" > True
``` 

> Example 2

The check_time function checks for the time format of a 12-hour clock.

```
pattern = r"^[1-9][1-2]?\:[0-5][0-9] ?(am|pm|PM|AM)$"

"12:45pm" > True
"9:59 AM" > True
"6:60am" > False
"five o'clock" > False
```
 

