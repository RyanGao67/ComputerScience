## String in JavaScript  
### length of a string  
```
<!DOCTYPE html>
<html>
	<body>
		<h2>JS string property</h2>
		<p>The length property returns the length of a string:</p>
		<p id="demo"></p>
		<script>
			var txt = "1234567890";
			document.getElementById("demo").innerHTML = txt.length;
		</script>
	</body>
</html>
```

### indexOf()  and lastIndexOf()
indexOf() : this function returns the index of the first occurence of a specified text in a string.      
lastIndexOf() : this function returns the index of the last occurence of a specified text in a string. 
```
var str = "Please locate where 'locate' occurs!";
var pos = str.lastIndexOf("locate");

var str = "Please locate where 'locate' occurs!";  
var pos = str.lastIndexOf("locate");
```   
Both function will return -1 if the text is not found. 
Both functions accept a second parameter as the starting position for the search:
```
var str = "Please locate where 'locate' occurs!";  
var pos = str.indexOf("locate", 15);
```   

### Slice
```
var str = "Apple, Banana, Kiwi";
var res = str.slice(7,13);
```

If you omit the second parameter, the method will slice out the rest of the string:
var res =  str.slice(7);


### replace()
```
<!DOCTYPE html>
<html>
<body>

<h2>JavaScript String Methods</h2>

<p>Replace all occurrences of "Microsoft" with "W3Schools" in the paragraph below:</p>

<button onclick="myFunction()">Try it</button>

<p id="demo">Please visit Microsoft and Microsoft!</p>

<script>
function myFunction() {
  var str = document.getElementById("demo").innerHTML; 
  var txt = str.replace(/Microsoft/g,"W3Schools");
  document.getElementById("demo").innerHTML = txt;
}
</script>

</body>
</html>



```

To replace all matches, use a  **regular expression**  with a  `/g`  flag (global match).

### concat()   
var text1 = "Hello";
var text2  = "World";
var text3 = text1.concat(" ", text2);

var text = "Hello" + " " + "World!";

All string methods return a new string. Strings are immutable: Strings cannot be changed, only replaced. 

### trim()
```
var str = "     Hello World!     ";  
alert(str.trim());
```

### charAt() and charCodeAt()
```
var str = "Hello world";
str.charAt(0);
var str = "HELLO WORLD";
str.charCodeAt(0);
var  str =  "HELLO WORLD";  
str[0] =  "A"; **// Gives no error, but does not work**  
str[0];
```   


## Converting a String to an Array

A string can be converted to an array with the  `split()`  method:

### Example

var  txt =  "a,b,c,d,e"; // String  
txt.split(","); // Split on commas  
txt.split(" "); // Split on spaces  
txt.split("|"); // Split on pipe

[Try it Yourself »](https://www.w3schools.com/js/tryit.asp?filename=tryjs_string_split)

If the separator is omitted, the returned array will contain the whole string in index [0].

If the separator is "", the returned array will be an array of single characters:

### Example

var  txt =  "Hello"; // String  
txt.split(""); // Split in characters

[Try it Yourself »](https://www.w3schools.com/js/tryit.asp?filename=tryjs_string_split_char)
