# Form, list, table, position basic
* How to build form 
* How to build list
* How to build table
* What are five different position value
(From a front-end perspective)

##### There are five different position values
* static
  * not affected by top, bottom, left, right
  * no special way
  * it is positioned according to the normal flow of the page
* relative
  * relative to its normal position
  * other content will not be adjusted to fit into any gap left by the element
* fixed
  * An element with postion:fixed is positioned relative to the viewport, which means that it always stays in the same place even if the page is scrolled.
  * A fixed element does not leave a gap in page where it normally have located
* absolute
  * An element with position :absolute is positioned relative to the nearest positioned ancestor
  * A positioned element is one whose position is anything except static
  * An absolute element does not leave a gap in page where it normally have located
* sticky
  * this element will stick to the top of the viewport
  * An sticky element does not leave a gap in page where it normally have located
eg.
```
div.sticky{
    position:sticky;
    top:0;
    padding:15px;
    background-color:#cae81a;
}
```



#### The following will css and html will show how to build a table. <th> means table head.<tr> means table rows.
```
<table >
  <tr>
    <th>
      first
    </th>
    <th>
      second
    </th>
  </tr>
  <tr>
    <td>this</td>
    <td>that</td>
  </tr>
  <tr>
    <td>
      haha
    </td>
    <td>what</td>
  </tr>
</table>
```
```
table tr td{
  border:2px dashed rgb(255, 100, 80);
}

body{
  background:rgb(255, 0, 255);
  //some more attributes we can add
  //background:url();
  //background-repeat:no-repeat;
  //background-size:cover;
  //the previous line means that the background picture will cover the whole element
  // selected by this selector
}
```
#### Some tricks when using editor
step1: print ol
step2: press 'Tab'
Then the tab will be automaticly created

#### Block level container vs inline container  
<div> and <p> are block level container   
<span> is inline container

#### The following code shows how to build a list in html. ol means ordered list, if we use ul, then an unordered list will be created. 
```
<ol>
    <li></li>
    <li></li>
    <li></li>
</ol>
```

#### The following is a simple, easy-to-understand form. For the input tag, there are a couple of attributes that could be useful, including, type, placeholer, value, onChange, name, etc. 
```
<form action="www.google.com" method="post">
  <label> Username: <input type="text"> </label>
  <button>LogIn</button>
</form>
```