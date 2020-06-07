You have a function that checks the price  of a single candy by connecting with a web service. Unfortunately, the service is very slow, so your function is very slow. 

```python
import time
import datetime

def get_candy_price(candy_id):
	# let's use a sleep to simulate the time of connecting to web service
	time.sleep(5)
	
	# price=1.5 is the simulation of the return of the webservice
	price = 1.5 if candy_id%2==0 else 1
	return (datatime.datatime.now().strftime("%c"), price)

for i in range(0, 20):
	print(get_candy_price(2))
	print(get_candy_price(3))
```

How to improve the behaviour? 

```python

import time
import datetime
# import cachetools
from cachetools import cached, TTLCache
# create a cache obj
cache = TTLCache(maxsize=100, ttl=300)
@cached(cache)
def get_candy_price(candy_id):
	# let's use a sleep to simulate the time of connecting to web service
	time.sleep(5)
	
	# price=1.5 is the simulation of the return of the webservice
	price = 1.5 if candy_id%2==0 else 1
	return (datatime.datatime.now().strftime("%c"), price)

for i in range(0, 20):
	print(get_candy_price(2))
	print(get_candy_price(3))
```
The second one is the line to create the cache. We need to specify how many objects we will store (100, because we have just 100 kind of candies) and the time each cached results have to live (300 seconds, 5 minutes for us).  

Now try to execute this program again and you will notice that the first time the function is called (with parameter “2”) it takes 5 seconds to be executed (yes, the cache is empty at that time) and so it does the second time the function is called (with parameter “3”) because in the cache there’s just the result for the candy_id 2 that can’t be used since we are asking for the candy_id 3. But from the third call on, every single call is super fast, since the body of the method is NOT executed at all. The results we get are just taken from the cache system. Isn’t that cool?



### Approach two, pickle
```python
import os 
import pickle 
def cached(cachefile):  
""" A function that creates a decorator which will use "cachefile" for caching the results of the decorated function "fn". """  
	def decorator(fn):  
	# define a decorator for a function "fn"  
		def wrapped(*args, **kwargs):  
			# define a wrapper that will finally call "fn" with all arguments 
			# if cache exists -> load it and return its content  
			if os.path.exists(cachefile): 
				with open(cachefile, 'rb') as cachehandle: 
					print("using cached result from '%s'" % cachefile) 
					return pickle.load(cachehandle) 
				# execute the function with all arguments passed 
				res = fn(*args, **kwargs) 
				# write to cache file  
				with open(cachefile, 'wb') as cachehandle: 
					print("saving result to cache '%s'" % cachefile) 
					pickle.dump(res, cachehandle) 
			
			return res 
		return wrapped 
	return decorator # return this "customized" decorator that uses "cachefile"
```

To test our decorator, let's define a small fucntion that is made artificially slow by calling spleep()
```python
from time import sleep 
def my_slow_function(): 
	sleep(3) 
	return  'some result' 
print(my_slow_function())
```
```
@cached('slow_function_cache.pickle')  
def my_slow_function(): 
	sleep(3) 
	return  'some result'
```
