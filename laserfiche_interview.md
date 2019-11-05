### What do you like most about computer science?

### Constructor 
Initialize the data member of a class object 
A constructor is run wheneven an object is created.



### Encapsulation
it is a protective shield that prevents the data from being accessed by the code outside this shield.

Data Hiding: The user will have no idea about the inner implementation of the class. It will not be visible to the user that how the class is storing values in the variables. He only knows that we are passing the values to a setter method and variables are getting initialized with that value.
Increased Flexibility: We can make the variables of the class as read-only or write-only depending on our requirement. If we wish to make the variables as read-only then we have to omit the setter methods like setName(), setAge() etc. from the above program or if we wish to make the variables as write-only then we have to omit the get methods like getName(), getAge() etc. from the above program
Reusability: Encapsulation also improves the re-usability and easy to change with new requirements.
Testing code is easy: Encapsulated code is easy to test for unit testing.

### inheritance and polimorphism
In inheritance, there is a base class, which is inherited by the derived class. When a class inherits any other class, the member(s) of the base class becomes the member(s) of a derived class.


The compile time polymorphism is achieved through “overloading” whereas, the run time polymorphism is achieved through “overriding”.

The polymorphism allows the object to decide “which form of the function to be invoked when” at both, compile time and run time.

### inheritance 
In inheritance, there is a base class, which is inherited by the derived class. When a class inherits any other class, the member(s) of the base class becomes the member(s) of a derived class.

### Class or struct 
The only difference between using class and struct to define a class is the default access level

### Dynamic binding and static binding
The binding which can be resolved at compile time by compiler is known as static or early binding. The binding of static, private and final methods is compile-time.The reason is that the these method cannot be overridden and the type of the class is determined at the compile time.In inheritance, there is a base class, which is inherited by the derived class. When a class inherits any other class, the member(s) of the base class becomes the member(s) of a derived class.

Static binding example
Here we have two classes Human and Boy. Both the classes have same method walk() but the method is static, which means it cannot be overriden so even though I have used the object of Boy class while creating object obj, the parent class method is called by it. Because the reference is of Human type (parent class). So whenever a binding of static, private and final methods happen, type of the class is determined by the compiler at compile time and the binding happens then and there.


### Stack and heap
stack is used for statcic memory allocation, heap is userd for dynamic memory allocation. both stored in the computer's RAM. 
The stack memory is where local variables get stored or constructed. Stack also hold parameters passed to functions. The stack follows the patern of LIFO. The stack is usually in CPU cache, so operations in stack tend to be faster. It is also a limited resource and shouldn't be used for anything large Running out of stack memory is called stack overflow. 
objects created with new keyword allocate on the heap, there is no pattern  in heap memory and it almost always manually freed.

```
class Human{
   public static void walk()
   {
       System.out.println("Human walks");
   }
}
class Boy extends Human{
   public static void walk(){
       System.out.println("Boy walks");
   }
   public static void main( String args[]) {
       /* Reference is of Human type and object is
        * Boy type
        */
       Human obj = new Boy();
       /* Reference is of HUman type and object is
        * of Human type.
        */
       Human obj2 = new Human();
       obj.walk();
       obj2.walk();
   }
}
```
```
Output:

Human walks
Human walks
```

Dynamic Binding or Late Binding
When compiler is not able to resolve the call/binding at compile time, such binding is known as Dynamic or late Binding. Method Overriding is a perfect example of dynamic binding as in overriding both parent and child classes have same method and in this case the type of the object determines which method is to be executed. The type of object is determined at the run time so this is known as dynamic binding.

Dynamic binding example
This is the same example that we have seen above. The only difference here is that in this example, overriding is actually happening since these methods are not static, private and final. In case of overriding the call to the overriden method is determined at runtime by the type of object thus late binding happens. Lets see an example to understand this:

```
class Human{
   //Overridden Method
   public void walk()
   {
       System.out.println("Human walks");
   }
}
class Demo extends Human{
   //Overriding Method
   public void walk(){
       System.out.println("Boy walks");
   }
   public static void main( String args[]) {
       /* Reference is of Human type and object is
        * Boy type
        */
       Human obj = new Demo();
       /* Reference is of HUman type and object is
        * of Human type.
        */
       Human obj2 = new Human();
       obj.walk();
       obj2.walk();
   }
}
```
```
Output:

Boy walks
Human walks
```


Static binding happens at compile-time while dynamic binding happens at runtime.  
Binding of private, static and final methods always happen at compile time since these methods cannot be overridden. When the method overriding is actually happening and the reference of parent type is assigned to the object of child class type then such binding is resolved during runtime.   
The binding of overloaded methods is static and the binding of overridden methods is dynamic.  

### restful 
* Application programming interface   
* Representational State Transfer   
* Architecture style for designing network applications   
* Relies on a stateless, client-server protocol, almost always HTTP   
* Treats server objects as resources that can be created or destroyed  
* can be used by virtually any programming language  

GET: Retrieve data from a specified resource   
POST: Submit data to be processed to a specified resource   
PUT: Update a specified resource    
DELTE: DELTEte a specified resource    
HEAD: get   
OPTIONS : Returns the supported HTTP method       
PATCH:   

GET https://mysite.com/api/users

### A web service that complies to the SOAP web services specifications is a soap web service.
Simple Object Access protocal. Simple Object Access Protocol (SOAP) and Representational State Transfer (REST) are two answers to the same question: how to access Web services.SOAP relies exclusively on XML to provide messaging services.The point is that SOAP is highly extensible, but you only use the pieces you need for a particular task. For example, when using a public Web service that’s freely available to everyone, you really don’t have much need for WS-Security.Part of the magic is the Web Services Description Language (WSDL). This is another file that’s associated with SOAP. It provides a definition of how the Web service works, so that when you create a reference to it, the IDE can completely automate the process. 

Soap Vs Rest
SOAP is definitely the heavyweight choice for Web service access. It provides the following advantages when compared to REST:

Language, platform, and transport independent (REST requires use of HTTP)
Works well in distributed enterprise environments (REST assumes direct point-to-point communication)
Standardized
Provides significant pre-build extensibility in the form of the WS* standards
Built-in error handling
Automation when used with certain language products
REST is easier to use for the most part and is more flexible. It has the following advantages when compared to SOAP:

No expensive tools require to interact with the Web service
Smaller learning curve
Efficient (SOAP uses XML for all messages, REST can use smaller message formats)
Fast (no extensive processing required)
Closer to other Web technologies in design philosophy


### Was pretty much on why Laserfiche and other behavorial questions.
document management software. A few clicks laserfiche turns document to secure searchable files. 


### 3. What is virtual distructor  
* Deleting a derived class object using a pointer to a base class that has a non-virtual destructor results in undefined behavior. To correct this situation, the base class should be defined with a virtual destructor. 
* Making base class destructor virtual guarantees that the object of derived class is destructed properly, i.e., both base class and derived class destructors are called.
* As a guideline, any time you have a virtual function in a class, you should immediately add a virtual destructor (even if it does nothing). This way, you ensure against any surprises later.
### Polymorphism
* Polymorphism is the ability of an object to take on many forms. The most common use of polymorphism in OOP occurs when a parent class reference is used to refer to a child class object.


### How would use reverse a string using a stack?
```java
// O(1) extra place
// class Solution {
//     public void reverseString(char[] s) {
//         int i=0,j=s.length-1;
//         while(i<j){
//             char temp = s[i];
//             s[i] = s[j];
//             s[j] = temp;
//             i++;
//             j--;
//         }
//     }
// }
// O(n) extra place
class Solution{
    public void reverseString(char[] s){
        Stack<Character> ss = new Stack<>();
        for(char c: s){
            ss.push(c);
        }
        for(int i=0;i<s.length;i++){
            s[i] = ss.pop();
        }
    }
}
```
### sum of primes till n
[lc ](https://leetcode.com/problems/prime-arrangements/)
```java
class Solution {
    public int numPrimeArrangements(int n) {
        int mod = (int)(1e9 + 7);
        int count = getnumOfPrime(n);
        long r= 1;
        System.out.println(count);
        for(int k=2;k<=count;k++)r = (r*k)%mod;
        
        for(int k=2;k<=n-count;k++)r = (r*k)%mod;
        
        return  (int)(r);
    }
    
    public int getnumOfPrime(int n){
        boolean[] len = new boolean[n+1];
        Arrays.fill(len, 2, len.length, true);
        for(int i=2;i*i<len.length;i++){
            if(len[i]){
                for(int j=i*i;j<n+1;j+=i){
                    len[j] = false;
                }
            }
        }
        int result = 0;
        for(int i=2;i<len.length;i++){
            if(len[i])result++;
        }
        return result;
    }
}
```

### The Antisocial Club meets every week at Jim's Bar. Since they are so antisocial, however, everyone always sits as far as possible from the other members, and no one ever sits right next to another member. Because of this, the 25-stool bar is almost always less than half full and unfortunately for Jim the members that don't sit at the bar don't order any drinks. Jim, however, is pretty smart and makes up a new rule: The first person to sit at the bar has to sit at one of two particular stools. If this happens, then the maximum number of members will sit at the bar. Which stools must be chosen? Assume the stools are numbered 1 to 25 and are arranged in a straight line.

* The first person must take either stool 9 or 17 (because of symmetry, it doesn't matter which). Assume they pick seat 9. The next person will pick seat 25, since it is the furthest from seat 9. The next two people will take Seats one and 17. The next three will occupy 5, 13, and 21. The next six will occupy 3, 7, 11, 15, 19, and 23. This seats the maximum of 13 people, and no one is sitting next to another person. If a seat other than 9 or 17 is chosen first, the total bar patrons will be less than 13.
* 分析：25个座位，隔一个坐一个肯定是最优的坐法，能最多坐13个人。（这里麻烦拿出纸笔画一画啊，座位序号从1开始）如果先安排人坐了1，下个人就会坐25，再下个人坐13，接着就会坐7（or 19），接着就是坐19（or 7），然后就是4，10，16，22。由此可见这种方法肯定不是最优的。因为你先安排人坐了1，最后3号座位就没法坐了。所以为了保证3号位有人坐，那就的保证5号位有人坐；为了保证5号位有人坐，就得保证9号位有人坐；为了保证9号位有人坐，就得保证17号位有人坐。17再翻倍大于25了，所以循环结束，返回17号座位是第一个被安排的座位。所以每次循环从3开始，直到数字不能再翻倍，并且此方法适用于任何非25的数字，所以follow up也解决了。
pseudocode
```
int findFirstSeat(int totalSeats){
    if (totalSeats <= 2) return 1;
    int firstSeat = 3；
    while (firstSeat * 2 - 1 < totalSeats){
        firstSeat = firstSeat * 2 - 1;
    }
    return firstSeat;
}
```


### What't the difference between stack and queue
BASIS FOR COMPARISON  |	STACK   |	QUEUE
----------------------|---------|-------------
Working principle     |	LIFO (Last in First out)	|   FIFO (First in First out)
Structure	          |Same end is used to insert and delete elements.	|   One end is used for insertion, i.e., rear end and another end is used for deletion of elements, i.e., front end.
Number of pointers used| 	One	|Two (In simple queue case)
Operations performed	| Push and Pop	| Enqueue and dequeue
Examination of empty condition	| Top == -1	| Front == -1 || Front == Rear + 1
Examination of full condition   | Top == Max - 1   |   Rear == Max - 1
Variants	|   It does not have variants.	|   It has variants like circular queue, priority queue, doubly ended queue.
Implementation	|  Simpler	|   Comparatively complex
### What techniques are used to counter sql injection.
* Parameterized Statements
Programming languages talk to SQL databases using database drivers. A driver allows an application to construct and run SQL statements against a database, extracting and manipulating data as needed. Parameterized statements make sure that the parameters (i.e. inputs) passed into SQL statements are treated in a safe manner.

For example, a secure way of running a SQL query in JDBC using a parameterized statement would be:

```
// Define which user we want to find.
String email = "user@email.com";

// Connect to the database.
Connection conn = DriverManager.getConnection(URL, USER, PASS);
Statement stmt = conn.createStatement();

// Construct the SQL statement we want to run, specifying the parameter.
String sql = "SELECT * FROM users WHERE email = ?";

// Run the query, passing the 'email' parameter value...
ResultSet results = stmt.executeQuery(sql, email);

while (results.next()) {
  // ...do something with the data returned.
}
```
Contrast this to explicit construction of the SQL string, which is very, very dangerous:

```
// The user we want to find.
String email = "user@email.com";

// Connect to the database.
Connection conn = DriverManager.getConnection(URL, USER, PASS);
Statement stmt = conn.createStatement();

// Bad, bad news! Don't construct the query with string concatenation.
String sql = "SELECT * FROM users WHERE email = '" + email + "'";

// I have a bad feeling about this...
ResultSet results = stmt.executeQuery(sql);

while (results.next()) {
  // ...oh look, we got hacked.
}
```
The key difference is the data being passed to the executeQuery(...) method. In the first case, the parameterized string and the parameters are passed to the database separately, which allows the driver to correctly interpret them. In the second case, the full SQL statement is constructed before the driver is invoked, meaning we are vulnerable to maliciously crafted parameters.You should always use parameterized statements where available, they are your number one protection against SQL injection.

* Object Relational Mapping
Many development teams prefer to use Object Relational Mapping (ORM) frameworks to make the translation of SQL result sets into code objects more seamless. ORM tools often mean developers will rarely have to write SQL statements in their code – and these tools thankfully use parameterized statements under the hood.

The most well-known ORM is probably Ruby on Rails’ Active Record framework. Fetching data from the database using Active Record looks like this:

```
def current_user(email)
  # The 'User' object is an Active Record object, that has find methods 
  # auto-magically generated by Rails.
  User.find_by_email(email)
end
```
Code like this is safe from SQL Injection attacks.

* Escaping Inputs
If you are unable to use parameterized statements or a library that writes SQL for you, the next best approach is to ensure proper escaping of special string characters in input parameters.

Injection attacks often rely on the attacker being able to craft an input that will prematurely close the argument string in which they appear in the SQL statement. (This is why you you will often see ' or " characters in attempted SQL injection attacks.)Typically, doubling up the quote character – replacing ' with '' – means “treat this quote as part of the string, not the end of the string”.

Escaping symbol characters is a simple way to protect against most SQL injection attacks

* Sanitizing Inputs
Sanitizing inputs is a good practice for all applications. 

Developers should always make an effort to reject inputs that look suspicious out of hand,

Check that supplied fields like email addresses match a regular expression.
Ensure that numeric or alphanumeric fields do not contain symbol characters.
Reject (or strip) out whitespace and new line characters where they are not appropriate.


### Do you have a hard time coorperating with your teammate.

* I worked closely with Ann who, for the most part, always carried her fair share of the workload. During a stressful time, working on a project with a deadline, I realized Ann's contributions to the project were almost minimal. I made the decision to wait until after the project to speak with her. I'm glad I did because I learned she'd been going through a very tough time in her personal life and she appreciated my willingness to go the extra mile so that the project could be completed on time. As a result, our ability to work well together significantly increased.

### 4. What is the time complexity of inserting an element in an array.The time complexity of Single linked list and array?
* For an array, accessing elements at a specified index has a constant time of Big O(1).
* Inserting or removing from an array can come in three different forms: inserting/removing from the being, inserting/removing from the end, or inserting/removing from the middle. In order to add an element to the beginning of an array, we must shift every other element after it to a higher index. For example, If we wanted to add 2 to the beginning of the above so that it would now be at the zeroth index, 10 would now be at the first, 9 would be at the second and so on. Time taken will be proportional to the size of the list or Big O(n), n being the size of the list.
* Adding to the end of the array is a lot simpler in terms of speed. It involves adding the element to the next highest index of the array. This means that it is constant time and Big O(1) if the array is not already full. However, if the array is full it would involve having to create a new array and then copy the contents of the original into the new array which would be O(n). The third case of insertion would be adding to a position between the beginning and end of the array which would be Big O(n). The same time complexity is also true for removing from an array.
* When accessing elements of a linked list, speed is proportional to size of the list with Big O(n). Since we must traverse the entire list in order to get to the desired element, it is more costly compared to accessing elements of an array.
* When inserting a node into the beginning of the list, it only involves creating a new node with an address that points to the old head. The time it takes to perform this is not dependent on the size of the list. This means that it will be constant time or a Big O(1). Inserting an element to the end of the list involves traversing the whole list and then creating a new node and adjusting the previous node’s address for the next node. Time taken will be proportional to the size of the list and Big O(n). When we are inserting node into a position between the beginning and end of the linked list, we will have to traverse the list up until the specific point and then adjust the pointers with Big O(n). The same time complexity is also true for removing nodes from a linked list.



### On site problmes: 1: Weather a number is a prime
```
# taking input from user
number = int(input("Enter any number: "))

# prime number is always greater than 1
if number > 1:
    for i in range(2, number):
        if (number % i) == 0:
            print(number, "is not a prime number")
            break
    else:
        print(number, "is a prime number")

# if the entered number is less than or equal to 1
# then it is not prime number
else:
    print(number, "is not a prime number")
```

### Describe a case that you don't have much resources, but you completed a hard project.
### How would you describe your role when working in a team?
I would say I'm like glue that can stick my team together. 
I have to produce because if I can not produce what I should produce then the team can not produce the team should produce. 
I'm a good listener
I'm a good communicator.
### How do your projects help you understand software engineer better?
Define Success (User Acceptance Criteria)   
Create Projects within Projects    
a retrospective and discuss what was done well, what was not done well, what can be improved, and what needs to be taken care of immediately.


### Static vs Dynamic Binding in Java


第一轮，我考的是prime number求和。自己写出来后，面试官给我说了一个方法叫我实现，就是先new一个数组，把所有的数都初始化“是prime number”，然后开始循环，从2开始，把2加到sum里面后，把所有小于n的2的倍数标记为“不是prime number”，然后继续循环；把3加到sum后，把所有小于n的3的倍数标记为“不是prime number”；然后4是2的倍数，被标记过了，不加；然后5没有被标记，加进sum，然后5的倍数标记为“不是prime number”一直循环循环。。。。。。然后我按照他的要求写了，结果叫我分析时间复杂度的时候我懵逼了，因为本来面试就比较紧张，然后又稍微没弄懂内层循环标记倍数那里的时间复杂度到底应该怎么算，就卡壳了好久，然后人家等了半天就说，算了吧。所以当时就觉得完了，要挂了。后来也没继续想这个问题，麻烦哪个大神看到了告诉告诉我，内层标记2、3、5、7……的倍数那部分的复杂度怎么算啊？

接着就是问简历，宝宝前两个project都还挺熟悉的，结果他不问，挑了个第三个project， which已经做了很久，记不太清了。扯了半天，自己都记不清，怎么给人家讲得懂嘛？！心里又想要挂了。。。

然后, 你有没有在很有限information的情况下完成一件任务的经历；你有没和队友产生过conflict，最后怎么解决的。第一轮结束，感觉到心灰意冷。。。  
  
在会议室等着第二轮的人来面我，并且调整状态，做好心理建设。  
  
第二轮，是两个年轻中国engineer来面的，估计也就比我大一两届吧最多，感觉一下就没那么紧张了。因为上一轮已经问过简历，就直接开始写题。
第二题，climbing stairs问题，一道简单DP，同样写出来后分析时间空间复杂度（https://leetcode.com/problems/climbing-stairs/）。写出来后叫我优化空间复杂度，用滚动指针，即求当前状态的值只依赖前两个状态的值,没必要保留所有状态的值。所以f[i % 2] = f[(i - 1) % 2] + f[(i - 2) % 2]即可。完成后还有时间，年轻工程师也不造问啥问题好，就临时想了几个问题问了问。比如我会不会android，答：不会；会不会数据库，答：不会（我这种弱渣啊。。。）。但是这种时候，你一定要表达出的意向是，虽然我不会，但是我有一颗愿意学习的心啊，而且学东西很快的~然后问问我abstract class和interface的区别，虽然也懂个大概，但之前没考虑过这个问题，就一边按照自己理解，一边组织语言答得七七八八吧。然后面试结束，被hr送出大楼。
Your task is to implement a findMatches function that takes a query value and a pre-sorted array of integers that may contain duplicates. It should find and return
 The index of the first instance of the query value in the array, or -1 if the query value does not exist in the array.
 The number of times the query value appears in the array (0 if the query value does not exist in the array).
As an example, if findMatches is called, and
 query = 8
 values = { 0, 1, 2, 3, 4, 4, 5, 6, 7, 8, 8, 8, 9 }
  
then it should return,
 firstMatchIndex = 9, because values[9] is the first occurrence of 8
 numberOfMatches = 3, because there are three occurrences of 8
Implement the findMatches function in C, C++ or Java according to the function signatures below, and send back the code as an attached .txt file.
Things to keep in mind:
1. Assume that a junior programmer is going to read your code. You should include comments and any other aides that you use to communicate your code to other developers.
2. The code should compile and work.
3. Your solution should scale well with large arrays.


编程题是DynamicProgramming经典题目coinchange

第二题是24点游戏： 给你一组数字和一个目标数字，判断是否能让那一组数字通过四则运算得到目标数字。因为近期leetcode看多了，一直往nlogn复杂度想，没想出来，后来和面试官表面没有想法之后，他直接就把算法给我说了让我写代码。一看算法晕了，居然是brute force。。  
  
还问了一些数据库的基础知识比如index之类的。
* Well, an index is a data structure (most commonly a B- tree) that stores the values for a specific column in a table. An index is created on a column of a table. So, the key points to remember are that an index consists of column values from one table, and that those values are stored in a data structure. The index is a data structure – remember that.
* The reason B- trees are the most popular data structure for indexes is due to the fact that they are time efficient – because look-ups, deletions, and insertions can all be done in logarithmic time. And, another major reason B- trees are more commonly used is because the data that is stored inside the B- tree can be sorted. 

### 5, Clime stairs
### How to implement a non-recurrency in-order traverse iterator class of binary tree.
### 2. Detect cycles in a singly-linked list.  
### 3. Reverse all the words in a sentence.

### 第一题，leetcode  [Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)  就是一次交易的那个。

### 第二题，一个 2-dimension 的平面上有一个点 P0 和 其他一堆点（n个），找出距离 P0 最近的 k 个点。
K nearest points on the plain


Roman to Integer
Roman Validation

