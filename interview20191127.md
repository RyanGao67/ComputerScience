### Final, Finally, Finalize
final	| finally	| finalize
--------|---------------|---------
Final is used to apply restrictions on class, method and variable. Final class can't be inherited, final method can't be overridden and final variable value can't be changed.	| Finally is used to place important code, it will be executed whether exception is handled or not.	| Finalize is used to perform clean up processing just before object is garbage collected.
Final is a keyword.	| Finally is a block.	| Finalize is a method.

### anagram
(https://leetcode.com/problems/valid-anagram/)[https://leetcode.com/problems/valid-anagram/]
(https://leetcode.com/problems/find-anagram-mappings/)[https://leetcode.com/problems/find-anagram-mappings/]
(https://leetcode.com/problems/find-all-anagrams-in-a-string/)[https://leetcode.com/problems/find-all-anagrams-in-a-string/]
(https://leetcode.com/problems/group-anagrams/)[https://leetcode.com/problems/group-anagrams/]
### count number of unique words in a n array of words
(https://leetcode.com/problems/unique-word-abbreviation/)[https://leetcode.com/problems/unique-word-abbreviation/]
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
(https://leetcode.com/problems/binary-tree-postorder-traversal/)[https://leetcode.com/problems/binary-tree-postorder-traversal/]
(https://leetcode.com/problems/binary-tree-preorder-traversal/)[https://leetcode.com/problems/binary-tree-preorder-traversal/]
(https://leetcode.com/problems/flip-binary-tree-to-match-preorder-traversal/)[https://leetcode.com/problems/flip-binary-tree-to-match-preorder-traversal/]
(https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)[https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/]
(https://leetcode.com/problems/vertical-order-traversal-of-a-binary-tree/)[https://leetcode.com/problems/vertical-order-traversal-of-a-binary-tree/]
(https://leetcode.com/problems/binary-tree-level-order-traversal/)[https://leetcode.com/problems/binary-tree-level-order-traversal/]
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
(https://leetcode.com/problems/reverse-linked-list/)[https://leetcode.com/problems/reverse-linked-list/]
### fibonaci series
(https://leetcode.com/problems/fibonacci-number/discuss/215992/Java-Solutions)[https://leetcode.com/problems/fibonacci-number/discuss/215992/Java-Solutions]
(https://leetcode.com/problems/length-of-longest-fibonacci-subsequence/)[https://leetcode.com/problems/length-of-longest-fibonacci-subsequence/]
(https://leetcode.com/problems/reverse-linked-list-ii/)[https://leetcode.com/problems/reverse-linked-list-ii/]
### restful vs soap
### interface, abstract class polymorphism example
### design database for library
### factorial 
### Name a example of a team experience that did not go well 
### Project you are proud of 
### comparator java maximum of tree instance
### Why you want to work in tr

### Sorting
### Interface vs abstract class
### java exceptions
### How to add dynamic library in java
### given a list of integers, count the number of swaps it will take to move the even numbers to left half and odd to right
### x=x+y;y=x-y;x=x-y
### version number x/y/z return more recent
### TCP HTTP
### implement LRU cache
### write arraylist add unittest
(https://www.journaldev.com/110/how-to-implement-arraylist-with-array-in-java)[https://www.journaldev.com/110/how-to-implement-arraylist-with-array-in-java]
### invert a binary tree
(https://leetcode.com/problems/invert-binary-tree/)[https://leetcode.com/problems/invert-binary-tree/]
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
### find second higest element
(https://leetcode.com/problems/kth-largest-element-in-a-stream/)[https://leetcode.com/problems/kth-largest-element-in-a-stream/]
### what is immutable class write a immutable class
### overiding static method,write singleton class
### ER diagram https://www.guru99.com/er-diagram-tutorial-dbms.html
### fizzbuzz  
### where js run
### how to manipulate java string
### Design a object storing info about a pet shop animals and employer
### mapreduce in java 
### corejava servelet jsp spring 
### linked list vs arraylist
### process vs thread
### Socket programming java
### SQL haveing vs where
### palindrome
### treeset

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
// C++ implementation of For each element in 1st  
// array count elements less than or equal to it 
// in 2nd array 
#include <bits/stdc++.h> 
  
using namespace std; 
  
// function returns the index of largest element  
// smaller than equal to 'x' in 'arr'. For duplicates 
// it returns the last index of occurrence of required 
// element. If no such element exits then it returns -1.  
int binary_search(int arr[], int l, int h, int x) 
{ 
    while (l <= h) 
    { 
        int mid = (l+h) / 2; 
  
        // if 'x' is greater than or equal to arr[mid],  
        // then search in arr[mid+1...h] 
        if (arr[mid] <= x) 
            l = mid + 1; 
  
        // else search in arr[l...mid-1]     
        else
            h = mid - 1;     
    } 
      
    // required index 
    return h; 
} 
  
// function to count for each element in 1st array, 
// elements less than or equal to it in 2nd array 
void countEleLessThanOrEqual(int arr1[], int arr2[],  
                             int m, int n) 
{ 
    // sort the 2nd array 
    sort(arr2, arr2+n); 
      
    // for each element of 1st array 
    for (int i=0; i<m; i++) 
    { 
        // last index of largest element  
        // smaller than or equal to x 
        int index = binary_search(arr2, 0, n-1, arr1[i]); 
          
        // required count for the element arr1[i] 
        cout << (index+1) << " "; 
    } 
} 
  
// Driver program to test above 
int main() 
{ 
    int arr1[] = {1, 2, 3, 4, 7, 9}; 
    int arr2[] = {0, 1, 2, 1, 1, 4}; 
    int m = sizeof(arr1) / sizeof(arr1[0]); 
    int n = sizeof(arr2) / sizeof(arr2[0]); 
    countEleLessThanOrEqual(arr1, arr2, m, n); 
    return 0; 
}  
```
### Dependency inversion yilaizhuru kongzhifanzhuan foreach(S)
### How to partition a huge volume of data for distributed processing(S)
### Thread class, Race condition, Callable and Runnable Synchronisation(S)
### Core java, Multithreading, sql joins and corelated
### How to change design in a distributed environment
### How to handle variable update on GUI thread in a multithread environemnt(s)
### immutable class 
### how concurrent hashmap works 
### How to apply security on restful web services if application has more write operation s and less read opperations 
### why wait and notify should be called from  syncronized block
### Hibernate JDBC
### plunker CSS
### data normalization
### compiler vs interpreter
### implement singlton pattern
### design chessgame  deep swallow copy hibernate first level cache second level coppy
### whata happends behind the scene when sql join
### what is responsive design