### comparator java maximum of tree instance
[https://leetcode.com/problems/connecting-cities-with-minimum-cost/](https://leetcode.com/problems/connecting-cities-with-minimum-cost/)     
[https://leetcode.com/problems/optimize-water-distribution-in-a-village/](https://leetcode.com/problems/optimize-water-distribution-in-a-village/)     
[https://leetcode.com/problems/minimum-path-sum/](https://leetcode.com/problems/minimum-path-sum/)  
### implement LRU cache
[https://leetcode.com/problems/lru-cache/submissions/](https://leetcode.com/problems/lru-cache/submissions/)
### write arraylist add unittest
[https://github.com/RyanGao67/JavaArrayList](https://github.com/RyanGao67/JavaArrayList)
[https://www.journaldev.com/110/how-to-implement-arraylist-with-array-in-java](https://www.journaldev.com/110/how-to-implement-arraylist-with-array-in-java)    
### invert a binary tree
[https://leetcode.com/problems/invert-binary-tree/](https://leetcode.com/problems/invert-binary-tree/)    
### given a list of integers, count the number of swaps it will take to move the even numbers to left half and odd to right
```java
    static void segregateEvenOdd(int arr[]) 
    { 
        /* Initialize left and right indexes */
        int left = 0, right = arr.length - 1; 
        while (left < right) 
        { 
            /* Increment left index while we see 0 at left */
            while (arr[left]%2 == 0 && left < right) 
                left++; 
  
            /* Decrement right index while we see 1 at right */
            while (arr[right]%2 == 1 && left < right) 
                right--; 
  
            if (left < right) 
            { 
                /* Swap arr[left] and arr[right]*/
                int temp = arr[left]; 
                arr[left] = arr[right]; 
                arr[right] = temp; 
                left++; 
                right--; 
            } 
        } 
    } 
```
### x=x+y;y=x-y;x=x-y
### version number x/y/z return more recent
```java
package tgao.indocresearch.org;
import java.util.Arrays;
import java.util.Comparator;
public class VersionNumber {
	static class CustomerSorting implements Comparator<String>{
		@Override
		public int compare(String arg0, String arg1) {
			// TODO Auto-generated method stub
			String[] args1 = arg0.split("/");
			String[] args2 = arg1.split("/");
			int i=0,j=0;
			while(i<args1.length && j<args2.length) {
				if(args1[i].compareTo(args2[j])>0)return -1;
				if(args1[i].compareTo(args2[j])<0)return 1;
				i++;j++;
			}
			if(i<args1.length)return -1;
			if(j<args2.length)return 1;
			return 0;
		}
	}
	public static void main(String[] args) {
		String[] versions = new String[] {"1/2/3", "1/3/4", "2/2/1"};
		Arrays.sort(versions, new CustomerSorting());
		for(int i=0;i<versions.length;i++) {
			System.out.println(versions[i]);
		}
	}
}
```

### anagram
[https://leetcode.com/problems/valid-anagram/](https://leetcode.com/problems/valid-anagram/)  
[https://leetcode.com/problems/find-anagram-mappings/](https://leetcode.com/problems/find-anagram-mappings/)   
[https://leetcode.com/problems/find-all-anagrams-in-a-string/](https://leetcode.com/problems/find-all-anagrams-in-a-string/)   
[https://leetcode.com/problems/group-anagrams/](https://leetcode.com/problems/group-anagrams/)  
### factorial 
```java
  int i,fact=1;  
  int number=5;//It is the number to calculate factorial    
  for(i=1;i<=number;i++){    
      fact=fact*i;    
  }    
```
### traverse a map
```java
        HashMap<String, Integer> hm =  HashMap<String, Integer>(); 
        hm.put("GeeksforGeeks", 54);  hm.put("For geeks", 82); 
        for (Map.Entry mapElement : hm.entrySet()) { 
            String key = (String)mapElement.getKey(); 
            int value = ((int)mapElement.getValue() + 10); 
        } 
```


### count number of unique words in a n array of words
[https://leetcode.com/problems/unique-word-abbreviation/](https://leetcode.com/problems/unique-word-abbreviation/)    
```java
public class SetTest {
	public static void main(String[] args) {
		String[] temp = new String[] {"SET", "SET", "TES"};
		Set<String> set  = new HashSet<String>(Arrays.asList(temp));
		System.out.println(set);
	}
}

```
### Traversal of tree reverse string
[https://leetcode.com/problems/binary-tree-postorder-traversal/](https://leetcode.com/problems/binary-tree-postorder-traversal/)   
[https://leetcode.com/problems/binary-tree-preorder-traversal/](https://leetcode.com/problems/binary-tree-preorder-traversal/)    
[https://leetcode.com/problems/flip-binary-tree-to-match-preorder-traversal/](https://leetcode.com/problems/flip-binary-tree-to-match-preorder-traversal/)  
[https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)         
[https://leetcode.com/problems/vertical-order-traversal-of-a-binary-tree/](https://leetcode.com/problems/vertical-order-traversal-of-a-binary-tree/)   
[https://leetcode.com/problems/binary-tree-level-order-traversal/](https://leetcode.com/problems/binary-tree-level-order-traversal/)   

### compare content in two files using java program
```java
File file1 = new File("file1.txt");
File file2 = new File("file2.txt");
boolean isTwoEqual = FileUtils.contentEquals(file1, file2);
/*
It does the following checks before actually doing the comparison:
existence of both the files
Both file's that are passed are to be of file type and not directory.
length in bytes should not be the same.
Both are different files and not one and the same.
Then compare the contents.*/
```
Here are some classes you should know that can be used to read character data:
* Reader: An abstract class to read a character stream.
* InputStreamReader: Class used to read the byte stream and converts to character stream.
* FileReader: A class to read the characters from a file.
* BufferedReader: This is a wrapper over the Reader class that supports buffering capabilities. In many cases this is most preferable class to read data because more data can been read from the file in one read() call, reducing the number of actual I/O operations with file system.   

And here are some classes you can use to write character data to a file:
* Writer: This is an abstract class to write the character streams.
* OutputStreamWriter: This class is used to write character streams and also convert them to byte streams.
* FileWriter: A class to actually write characters to the file.
* BufferedWriter: This is a wrapper over the Writer class, which also supports buffering capabilities. This is most preferable class to write data to a file since more data can be written to the file in one write() call. And like the BufferedReader, this reduces the number of total I/O operations with file system.

Here are the classes used to read the byte data:
* InputStream: An abstract class to read the byte streams.
* FileInputStream: A class to simply read bytes from a file.
* BufferedInputStream: This is a wrapper over InputStream that supports buffering capabilities. As we saw in the character streams, this is a more efficient method than FileInputStream.  
And here are the classes used to write the byte data:
* OutputStream: An abstract class to write byte streams.
* FileOutputStream: A class to write raw bytes to the file.
* ByteOutputStream: This class is a wrapper over OutputStream to support buffering capabilities. And again, as we saw in the character streams, this is a more efficient method than FileOutputStream thanks to the buffering.


The main difference between these two packages is that the read() and write() methods of Java IO are a blocking calls. By this we mean that the thread calling one of these methods will be blocked until the data has been read or written to the file.

On the other hand, in the case of NIO, the methods are non-blocking. This means that the calling threads can perform other tasks (like reading/writing data from another source or update the UI) while the read or write methods wait for their operation to complete. This can result in a significant performance increase if you're dealing with many IO requests or lots of data.

### how to reverse singly linked list, Even odd program using multithreading
[https://leetcode.com/problems/reverse-linked-list/](https://leetcode.com/problems/reverse-linked-list/)    
[https://java2blog.com/print-even-odd-numbers-threads-java/](https://java2blog.com/print-even-odd-numbers-threads-java/)   
[https://www.baeldung.com/java-even-odd-numbers-with-2-threads](https://www.baeldung.com/java-even-odd-numbers-with-2-threads)   
### fibonaci series
[https://leetcode.com/problems/fibonacci-number/discuss/215992/Java-Solutions](https://leetcode.com/problems/fibonacci-number/discuss/215992/Java-Solutions)    
[https://leetcode.com/problems/length-of-longest-fibonacci-subsequence/](https://leetcode.com/problems/length-of-longest-fibonacci-subsequence/)    
[https://leetcode.com/problems/reverse-linked-list-ii/](https://leetcode.com/problems/reverse-linked-list-ii/)    


### sql nsql
[https://www.educative.io/courses/grokking-the-system-design-interview/YQlK1mDPgpK](https://www.educative.io/courses/grokking-the-system-design-interview/YQlK1mDPgpK)
### Spring di ioc
### docker vs vm
[https://geekflare.com/docker-vs-virtual-machine/](https://geekflare.com/docker-vs-virtual-machine/)
### singleton factory
* singleton   
```java
package designPattern;
public class SingletonLazy {
	private static SingletonLazy instance;
	private SingletonLazy() {
		System.out.println("initializing a lazy singleton");
	}
	// race condition
//	public static SingletonLazy getInstance() {
//		if(instance!=null) {}
//		else {instance=new SingletonLazy();}
//		return instance;
//	}
	// double-checked locking
	public static SingletonLazy getInstance() {
		if(instance==null) {
			synchronized (SingletonLazy.class) {
				if(instance==null) {
					instance = new SingletonLazy();
				}
			}
		}
		return instance;
	}
}
```   
* factory
```java
package designPattern;
class Point{
	private double x, y;
	private Point(double a, double b) {
		this.x = a;
		this.y = b;
	}
	static class Factory{
		public static Point newCartesianPoint(double a, double b) {
			return new Point(a, b);
		}
		public static Point newPolarPoint(double a, double b) {
			return new Point(Math.cos(a)*b, Math.sin(a)*b);
		}
	}
	@Override
	public String toString() {return "x="+x+";y="+y;}
}
class FactoryPattern{
  public static void main(String[] args){
    Point point1 = Point.Factory.newCartesianPoint(1, 2);
    Point point2 = Point.Factory.newPolarPoint(10, 3.14);
    System.out.println(point1);
    System.out.println(point2);
  }
}
```
### Final, Finally, Finalize
final	| finally	| finalize
--------|---------------|---------
Final is used to apply restrictions on class, method and variable. Final class can't be inherited, final method can't be overridden and final variable value can't be changed.	| Finally is used to place important code, it will be executed whether exception is handled or not.	| Finalize is used to perform clean up processing just before object is garbage collected.
Final is a keyword.	| Finally is a block.	| Finalize is a method.


[https://www.javatpoint.com/difference-between-final-finally-and-finalize](https://www.javatpoint.com/difference-between-final-finally-and-finalize)  


### restful vs soap 
[https://github.com/RyanGao67/ComputerScience/blob/master/Microservice_with_spring_boot.md](https://github.com/RyanGao67/ComputerScience/blob/master/Microservice_with_spring_boot.md) 
### interface, abstract class polymorphism example
[https://www.geeksforgeeks.org/difference-between-abstract-class-and-interface-in-java/](https://www.geeksforgeeks.org/difference-between-abstract-class-and-interface-in-java/)   

### Name a example of a team experience that did not go well 
### Project you are proud of 
### Why you want to work in tr  

### Sorting
```java
public static void mergeSort(int[] array, int left, int right) {
    if (right <= left) return;
    int mid = (left+right)/2;
    mergeSort(array, left, mid);
    mergeSort(array, mid+1, right);
    merge(array, left, mid, right);
}
 void merge(int[] array, int left, int mid, int right) {
    // calculating lengths
    int lengthLeft = mid - left + 1;
    int lengthRight = right - mid;

    // creating temporary subarrays
    int leftArray[] = new int [lengthLeft];
    int rightArray[] = new int [lengthRight];

    // copying our sorted subarrays into temporaries
    for (int i = 0; i < lengthLeft; i++)
        leftArray[i] = array[left+i];
    for (int i = 0; i < lengthRight; i++)
        rightArray[i] = array[mid+i+1];

    // iterators containing current index of temp subarrays
    int leftIndex = 0;
    int rightIndex = 0;

    // copying from leftArray and rightArray back into array
    for (int i = left; i < right + 1; i++) {
        // if there are still uncopied elements in R and L, copy minimum of the two
        if (leftIndex < lengthLeft && rightIndex < lengthRight) {
            if (leftArray[leftIndex] < rightArray[rightIndex]) {
                array[i] = leftArray[leftIndex];
                leftIndex++;
            }
            else {
                array[i] = rightArray[rightIndex];
                rightIndex++;
            }
        }
        // if all the elements have been copied from rightArray, copy the rest of leftArray
        else if (leftIndex < lengthLeft) {
            array[i] = leftArray[leftIndex];
            leftIndex++;
        }
        // if all the elements have been copied from leftArray, copy the rest of rightArray
        else if (rightIndex < lengthRight) {
            array[i] = rightArray[rightIndex];
            rightIndex++;
        }
    }
}
```
```java
static int partition(int[] array, int begin, int end) {
    int pivot = end;

    int counter = begin;
    for (int i = begin; i < end; i++) {
        if (array[i] < array[pivot]) {
            int temp = array[counter];
            array[counter] = array[i];
            array[i] = temp;
            counter++;
        }
    }
    int temp = array[pivot];
    array[pivot] = array[counter];
    array[counter] = temp;

    return counter;
}

public static void quickSort(int[] array, int begin, int end) {
    if (end <= begin) return;
    int pivot = partition(array, begin, end);
    quickSort(array, begin, pivot-1);
    quickSort(array, pivot+1, end);
}
```
### java exceptions
### How to add dynamic library in java
[https://cs-fundamentals.com/tech-interview/c/difference-between-static-and-dynamic-linking.php](https://cs-fundamentals.com/tech-interview/c/difference-between-static-and-dynamic-linking.php)   
[https://www.chilkatsoft.com/java-loadLibrary-Linux.asp](https://www.chilkatsoft.com/java-loadLibrary-Linux.asp)   

### TCP HTTP

### reverse a string using recursion
```python
class Solution(object):
    def reverseString(self, s):
        l = len(s)
        if l < 2:
            return s
        return self.reverseString(s[l/2:]) + self.reverseString(s[:l/2])
```
### find repeating charactersfrom given string
[https://leetcode.com/problems/first-unique-character-in-a-string/](https://leetcode.com/problems/first-unique-character-in-a-string/)
### find second higest element
[https://leetcode.com/problems/kth-largest-element-in-a-stream/](https://leetcode.com/problems/kth-largest-element-in-a-stream/)    
### what is immutable class write a immutable class
* Declare the class as final so it can’t be extended.
* Make all fields private so that direct access is not allowed.
* Don’t provide setter methods for variables
* Make all mutable fields final so that it’s value can be assigned only once.
* Initialize all the fields via a constructor performing deep copy.
* Perform cloning of objects in the getter methods to return a copy rather than returning the actual object reference.
```java
public final class FinalClassExample {
	private final int id;
	private final String name;
	private final HashMap<String,String> testMap;
	public int getId() {		return id;	}
	public String getName() {		return name;	}
	public HashMap<String, String> getTestMap() {
		return (HashMap<String, String>) testMap.clone();
	}
	public FinalClassExample(int i, String n, HashMap<String,String> hm){
		this.id=i;
		this.name=n;
		HashMap<String,String> tempMap=new HashMap<String,String>();
		String key;
		Iterator<String> it = hm.keySet().iterator();
		while(it.hasNext()){
			key=it.next();
			tempMap.put(key, hm.get(key));
		}
		this.testMap=tempMap;
	}
}
```
### overiding static method
### ER diagram  
[https://www.visual-paradigm.com/guide/data-modeling/what-is-entity-relationship-diagram/](https://www.visual-paradigm.com/guide/data-modeling/what-is-entity-relationship-diagram/)
### where js run

* JavaScript is what is called a Client-side Scripting Language. That means that it is a computer programming language that runs inside an Internet browser (a browser is also known as a Web client because it connects to a Web server to download pages).

### linked list vs arraylist
* ArrayList is implemented as a resizable array. As more elements are added to ArrayList, its size is increased dynamically. It's elements can be accessed directly by using the get and set methods, since ArrayList is essentially an array.   
* LinkedList is implemented as a double linked list. Its performance on add and remove is better than Arraylist, but worse on get and set methods.   
* Vector is similar with ArrayList, but it is synchronized. ArrayList is a better choice if your program is thread-safe. Vector and ArrayList require space as more elements are added. Vector each time doubles its array size, while ArrayList grow 50% of its size each time.   
* LinkedList, however, also implements Queue interface which adds more methods than ArrayList and Vector, such as offer(), peek(), poll(), etc.    Note: The default initial capacity of an ArrayList is pretty small. It is a good habit to construct the ArrayList with a higher initial capacity. This can avoid the resizing cost.    

### SQL haveing vs where
[https://javarevisited.blogspot.com/2013/08/difference-between-where-vs-having-clause-SQL-databse-group-by-comparision.html](https://javarevisited.blogspot.com/2013/08/difference-between-where-vs-having-clause-SQL-databse-group-by-comparision.html)  
### palindrome
### treeset
[https://www.geeksforgeeks.org/treeset-in-java-with-examples/](https://www.geeksforgeeks.org/treeset-in-java-with-examples/)  
### deep swallow copy
[https://stackoverflow.com/questions/184710/what-is-the-difference-between-a-deep-copy-and-a-shallow-copy](https://stackoverflow.com/questions/184710/what-is-the-difference-between-a-deep-copy-and-a-shallow-copy)

### compiler vs interpreter  

A compiler takes entire program and converts it into object code which is typically stored in a file. The object code is also refereed as binary code and can be directly executed by the machine after linking. Examples of compiled programming languages are C and C++.

An Interpreter directly executes instructions written in a programming or scripting language without previously converting them to an object code or machine code. Examples of interpreted languages are Perl, Python and Matlab.

### whata happends behind the scene when sql join   
[https://www.periscopedata.com/blog/how-joins-work](https://www.periscopedata.com/blog/how-joins-work)

### 题目：
Given two unsorted arrays arr1[] and arr2[]. They may contain duplicates. For each element in arr1[] count elements less than or equal to it in array arr2[].
Input : arr1[] = [1, 2, 3, 4, 7, 9]
        arr2[] = [0, 1, 2, 1, 1, 4]
Output : [4, 5, 5, 6, 6, 6]

Input : arr1[] = [5, 10, 2, 6, 1, 8, 6, 12]
        arr2[] = [6, 5, 11, 4, 2, 3, 7]
Output : [4, 6, 1, 5, 0, 6, 5, 7]

答案：
骨骼搜 element-1st-array-count-elements-less-equal-2nd-array
一模一样的题目 但是我用c++写到 要用 binary search的时候 他让我用 stl  所以 我用了 c++ 里面的lower_bound function

### 两轮面试
第一轮面试:
问了我简历上项目的经历 c++的熟练度
两道 李code easy question
两道sql query 问题  前一道题目 就是 select from where 后面一道 就是 inner join
接着给我看了一段c#的代码 让我做 code review

第二轮面试:
问了我项目上的经历
问了一道 dfs/bfs的经典问题

```java
import java.util.Arrays; 
class GFG 
{ 
    static int binary_search(int arr[], int l, int h, int x) 
    { 
        while (l <= h) 
        { 
            int mid = (l+h) / 2; 
            if (arr[mid] <= x) 
	    	l = mid + 1; 
            else 
	    	h = mid - 1;     
        } 
        return h; 
    } 
       
    static void countEleLessThanOrEqual(int arr1[], int arr2[],  
                                 int m, int n) 
    { 
        Arrays.sort(arr2); 
        for (int i=0; i<m; i++) 
        { 
            int index = binary_search(arr2, 0, n-1, arr1[i]);               
            System.out.print((index+1) + " "); 
        } 
    } 
    public static void main(String[] args) 
    { 
        int arr1[] = {1, 2, 3, 4, 7, 9}; 
        int arr2[] = {0, 1, 2, 1, 1, 4}; 
        countEleLessThanOrEqual(arr1, arr2, arr1.length, arr2.length); 
    } 
} 
```



### data normalization
[https://techdifferences.com/difference-between-normalization-and-denormalization.html](https://techdifferences.com/difference-between-normalization-and-denormalization.html)

### sql joins and corelated
[https://www.geeksforgeeks.org/sql-correlated-subqueries/](https://www.geeksforgeeks.org/sql-correlated-subqueries/)

### Thread class, Race condition, Callable and Runnable Synchronisation(S)
```java
if (x == 5) // The "Check"
{
   y = x * 2; // The "Act"

   // If another thread changed x in between "if (x == 5)" and "y = x * 2" above,
   // y will not be equal to 10.
}
```
The point being, y could be 10, or it could be anything, depending on whether another thread changed x in between the check and act. You have no real way of knowing.

In order to prevent race conditions from occurring, you would typically put a lock around the shared data to ensure only one thread can access the data at a time. This would mean something like this:
```java
// Obtain lock for x
if (x == 5)
{
   y = x * 2; // Now, nothing can change x until the lock is released. 
              // Therefore y = 10
}
// release lock for x
```

### Design a object storing info about a pet shop animals and employer
### design database for library
### How to partition a huge volume of data for distributed processing(S)
### How to change design in a distributed environment
### How to handle variable update on GUI thread in a multithread environemnt(s)
### how concurrent hashmap works 
### How to apply security on restful web services if application has more write operation s and less read opperations 
### why wait and notify should be called from  syncronized block
### Hibernate JDBC
### plunker CSS
### design chessgame   hibernate first level cache second level coppy
### what is responsive design
### Socket programming java
### mapreduce in java 
### servelet jsp spring 
