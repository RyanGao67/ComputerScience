## Date in Java, Javascript and Python
 This introduction gives a quick start on how to create a date type object instead of a detailed manual. Then can start from here to facilitate your searching with google or stackoverflow. 
1. First let's take a look at the following code snippet which should be self-explanotory
```java
package dateTest;
import java.util.*;
class DateTest{
    public static void main(String[] args){
        // This will create a new date object with the current time
        Date date = new Date();
        // getTime() obtain the number of milliseconds that have elapsed since midnight
        // January 1, 1970
        System.out.println(date.getTime()); // 1548057183923
        System.out.println(date.toString()); // Mon Jan 21 07:53:03 UTC 2019
        // The following are a couple of ways to new a Date object 
        // Date firstDate1 = new Date(int year, int month, int date);
        // Date firstDate2 = new Date(int year, int month, int date, int hrs, int min);
        // Date firstDate3 = new Date(int year, int month, int date, int hrs, int min, int sec);
        Date date1 = new Date(1995, 6, 4); // Month is 0 based, so the month here is July
        Date date2 = new Date(1995, 6, 4, 10, 30); 
        Date date3 = new Date(1995, 6,4, 10, 30, 30);
        System.out.println(date1.toString()); // Thu Jul 04 00:00:00 UTC 3895
        System.out.println(date2.toString());  // Thu Jul 04 10:30:00 UTC 3895
        System.out.println(date3.toString());  // Thu Jul 04 10:30:30 UTC 3895
        System.out.println(date1.before(date2));  //return true
        System.out.println(date1.after(date2));   // return false
        System.out.println(date1.equals(date2));  // return false
        System.out.println(date2.compareTo(date1));  //return 1
        System.out.println(date1.compareTo(date2));  //return -1
        System.out.println(date1.getClass());  // return class.java.util.Date
        
        
        // The following is the recommended way for date
        String[] months = {"Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", 
         "Oct", "Nov", "Dec"};
        int year;
        GregorianCalendar gcalendar = new GregorianCalendar();
        int month = gcalendar.get(Calendar.MONTH);     
        System.out.println(month);  // print 0
        System.out.print("Date: "); 
        System.out.print(months[gcalendar.get(Calendar.MONTH)]);   // print Jan
        System.out.print(" " + gcalendar.get(Calendar.DATE) + " ");  // print 21
        System.out.println(year = gcalendar.get(Calendar.YEAR));  // print 2019
        System.out.print("Time: ");
        System.out.print(gcalendar.get(Calendar.HOUR) + ":");
        System.out.print(gcalendar.get(Calendar.MINUTE) + ":");
        System.out.println(gcalendar.get(Calendar.SECOND));
        
        // GregorianCalendar(int year, int month, int date)
        // GregorianCalendar(int year, int month, int date, int hour, int minute)
        // GregorianCalendar(int year, int month, int date, int hour, int minute, int second)
        gcalendar = new GregorianCalendar(1995, 6, 4, 10, 30, 30);
        System.out.print("Date: ");
        System.out.print(months[gcalendar.get(Calendar.MONTH)]);
        System.out.print(" " + gcalendar.get(Calendar.DATE) + " ");
        System.out.println(year = gcalendar.get(Calendar.YEAR));
        System.out.print("Time: ");
        System.out.print(gcalendar.get(Calendar.HOUR) + ":");
        System.out.print(gcalendar.get(Calendar.MINUTE) + ":");
        System.out.println(gcalendar.get(Calendar.SECOND));
    }
}
```
2. For Javascript Date type is pretty easy
```javascript
// we need to pass a millisecond representation to Date
var time0 = new Date(Date.UTC(1995, 5, 4, 12, 12, 12));
var time1 = new Date(Date.parse("May 25, 2004"));
var time2 = new Date(1995, 5, 4, 12, 12, 12); 
// The constructor will call Date.UTC() behind the scene
var time3 = new Date("May 25, 2004");
// The constructor will call Date.parse() behind the scene
console.log(time1.valueOf());// return the milliseconds representation
console.log(time0 > time1); // false
console.log(new Date().toString()); // "Mon Jan 21 2019 00:27:23 GMT-0800 (北美太平洋标准时间)"
```


3. Python

```python
import datetime
datetime_object = datetime.datetime.now()
print(datetime_object) # 2018-12-19 09:26:03.478039

date_object = datetime.date.today()
print(date_object) # 2018-12-19

d = datetime.date(2019, 4, 13)
print(d) # 2019-04-13

from datetime import date
# date object of today's date
today = date.today() 
print("Current year:", today.year)
print("Current month:", today.month)
print("Current day:", today.day)
```
```
from datetime import time
# time(hour = 0, minute = 0, second = 0)
a = time()
print("a =", a)

# time(hour, minute and second)
b = time(11, 34, 56)
print("b =", b)

# time(hour, minute and second)
c = time(hour = 11, minute = 34, second = 56)
print("c =", c)

# time(hour, minute, second, microsecond)
d = time(11, 34, 56, 234566)
print("d =", d)
```
a = 00:00:00
b = 11:34:56
c = 11:34:56
d = 11:34:56.234566
```
from datetime import time

a = time(11, 34, 56)

print("hour =", a.hour)
print("minute =", a.minute)
print("second =", a.second)
print("microsecond =", a.microsecond)
```

hour = 11
minute = 34
second = 56
microsecond = 0
```
from datetime import datetime

#datetime(year, month, day)
a = datetime(2018, 11, 28)
print(a)

# datetime(year, month, day, hour, minute, second, microsecond)
b = datetime(2017, 11, 28, 23, 55, 59, 342380)
print(b)
```

2018 11-28 00:00:00
2017-11-28 23:55:59.342380
```
from datetime import datetime

a = datetime(2017, 11, 28, 23, 55, 59, 342380)
print("year =", a.year)
print("month =", a.month)
print("hour =", a.hour)
print("minute =", a.minute)
print("timestamp =", a.timestamp())
```
year = 2017
month = 11
day = 28
hour = 23
minute = 55
timestamp = 1511913359.34238

