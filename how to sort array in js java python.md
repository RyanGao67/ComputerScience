##### How to sort a list in python?
```
s=[[12, 'tall', 'blue', 1],
[2, 'short', 'red', 9],
[4, 'tall', 'blue', 13]]
s = sorted(s, key = lambda x: (x[1], x[2]))
```
##### How to sort an array in js?
```
var homes = [
    {
    "h_id": "3",
    "city": "Dallas",
    "state": "TX",
    "zip": "75201",
    "price": "162500"},
{
    "h_id": "4",
    "city": "Bevery Hills",
    "state": "CA",
    "zip": "90210",
    "price": "319250"},
{
    "h_id": "6",
    "city": "Dallas",
    "state": "TX",
    "zip": "75000",
    "price": "556699"},
{
    "h_id": "5",
    "city": "New York",
    "state": "NY",
    "zip": "00010",
    "price": "962500"}
];

homes.sort(function(a,b){
      if (a.city===b.city){
         return (b.price-a.price);
      } else if(a.city>b.city){
         return 1;
      } else if(a.city<b.city){
         return -1;
      }
   });

console.log(homes);
```
##### How to sort an array in java? 
```
import java.util.*; 
import java.lang.*; 
import java.io.*; 
class Student { 
    int rollno; 
    String name, address; 
    public Student(int rollno, String name, String address) { 
        this.rollno = rollno; 
        this.name = name; 
        this.address = address; 
    } 
    public String toString() { 
        return this.rollno + " " + this.name + " " + this.address; 
    } 
} 
class Sortbyroll implements Comparator<Student> { 
    // Used for sorting in ascending order of roll number 
    public int compare(Student a, Student b) { 
        return a.rollno - b.rollno; 
    } 
} 

class Main { 
    public static void main (String[] args) { 
        Student [] arr = {new Student(111, "bbbb", "london"), 
                          new Student(131, "aaaa", "nyc"), 
                          new Student(121, "cccc", "jaipur")}; 
        Arrays.sort(arr, new Sortbyroll()); 
    } 
} 
```

```
import java.util.Arrays; 
import java.util.Collections; 
public class SortExample { 
    public static void main(String[] args) { 
        String arr[] = {"practice.geeksforgeeks.org", 
                        "quiz.geeksforgeeks.org", 
                        "code.geeksforgeeks.org"
                       }; 
        Arrays.sort(arr); 
        Arrays.sort(arr, Collections.reverseOrder()); 
        System.out.printf("Modified arr[] : \n%s\n\n", 
                          Arrays.toString(arr)); 
    } 
} 
```
