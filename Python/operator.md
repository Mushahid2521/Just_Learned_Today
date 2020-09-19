## Operator Module      
This module helps to sort the item based on the specific item in the list or disctionary.  

```
import operator 

sorted(fruit.items(), key=operator.itemgetter(0))

sorted(fruit.items(), key=operator.itemgetter(1))

sorted(fruit.items(), key = operator.itemgetter(1), reverse=True)
```