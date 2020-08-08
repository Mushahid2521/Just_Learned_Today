## Regular Expression in Python

[**Practice Reg Ex here**](https://regex101.com/)
[****]
  
### Tags:
>```"."```   
 
Replace with any character   

>```"*"```    

Repeating character  

> ```{}```  
  
Repeating to a specific amount of times. ```[0-9]{4}, [0-9]{,6}, [0-9]{2,}```  
  
   
  

>```[]```  

Take the place of multiple options or defining range. ```[A-Za-z]*```

>```+```  

Repeate the character ```"Hello2441139" -> "[0-9]+ -> 2441139```

>```\w```

For representing ```[A-Za-z-]```


>```\s```  
  
White space character space, tab, new line.
 
 >```^``  

Starting of the line and complement of any range or value,  ```Ex: [^0-9]```

> **Capturing Groups**    
  
re.search(r"^(\w*), ([\w\s\.]*)$", "Kennedy, Jhon F.")   
```
result[0] -> Kennedy, Jhon F.
result[2] result[1] -> John F. Kennedy 
```
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

> Example 3

Return the Uppercase word after process ID.

```
regex = r"\[(\d+)\].*\b([A-Z]+)\b"

"bad_process[12345]: ERROR Performing package upgrade")) > 12345 (ERROR) > [result[1] result[2]
```   




> **re.sub(find, replace, text)**  
```
new_record = re.sub(r"\b([0-9-]+)\b", r"+1-\1","Sabrina Green,802-867-5309,System Administrator")
Sabrina Green,+1-802-867-5309,System Administrator

new_record = re.sub(r"\b([0-9-]+)\b", r"+1-\1", Sabrina Green,802-867-5309,System Administrator)
Sabrina Green,+1-802-867-5309,System Administrator

```

> Question 2
The multi_vowel_words function returns all words with 3 or more consecutive vowels (a, e, i, o, u). Fill in the regular expression to do that.  
```
pattern = r"\b[^\s]*[aeiou]{3,}\w+\b"
result = re.findall(pattern, text)
Obviously, the queen is courageous and gracious. -> ['Obviously', 'queen', 'courageous', 'gracious']
```

 

