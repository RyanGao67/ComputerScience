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

### The Antisocial Club meets every week at Jim's Bar. Since they are so antisocial, however, everyone always sits as far as possible from the other members, and no one ever sits right next to another member. Because of this, the 25-stool bar is almost always less than half full and unfortunately for Jim the members that don't sit at the bar don't order any drinks. Jim, however, is pretty smart and makes up a new rule: The first person to sit at the bar has to sit at one of two particular stools. If this happens, then the maximum number of members will sit at the bar. Which stools must be chosen? Assume the stools are numbered 1 to 25 and are arranged in a straight line.

The first person must take either stool 9 or 17 (because of symmetry, it doesn't matter which). Assume they pick seat 9. The next person will pick seat 25, since it is the furthest from seat 9. The next two people will take Seats one and 17. The next three will occupy 5, 13, and 21. The next six will occupy 3, 7, 11, 15, 19, and 23. This seats the maximum of 13 people, and no one is sitting next to another person. If a seat other than 9 or 17 is chosen first, the total bar patrons will be less than 13.


### How to implement a non-recurrency in-order traverse iterator class of binary tree.

### What't the difference between stack and queue

### What techniques are used to counter sql injection.

### Do you have a hard time coorperating with your teammate.

### Was pretty much on why Laserfiche and other behavorial questions.

### 1. What is polymorphism  
### 2. What't the difference between stack and queue  
### 3. What is virtual distructor  
### 4. What is the time complexity of inserting an element in an array.


### 1. Sum all the primes up to N.  
### 2. Detect cycles in a singly-linked list.  
### 3. Reverse all the words in a sentence.

### What do you like most about computer science?

### The time complexity of Single linked list and array?


### On site problmes: 1: Weather a number is a prime
###  2, Count the times of a number in an ordered array.  
### 3, 25 bar seats
### 4, whether 3 int String range"1-5, 6, 7-8"; 
### 5, Clime stairs
### Describe a case that you don't have much resources, but you completed a hard project.
### How would you describe your role when working in a team?
### How do your projects help you understand software engineer better?

### # Static vs Dynamic Binding in Java

### 第一题，leetcode  [Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)  就是一次交易的那个。

### 第二题，一个 2-dimension 的平面上有一个点 P0 和 其他一堆点（n个），找出距离 P0 最近的 k 个点。

### 第三题就是他家常规的 25 个座位问题。

Preparation
1. 25-bar stools problem.
There are twenty-five bar stools in a row. Customers who enter the bar follow these two
rules:
The customer will always sit in the seat farthest away from any other customer. A
customer will never sit right next to another customer. Using these two rules, where
should you place the first customer so that the maximum number of customers could sit
at the bar?
-> first person should be put to position 9. (index starting from 1)
ref: http://stackoverflow.com/questions/12572112/optimal-seating-arrangement-
algorithm
2. Output (the sum of) prime numbers to n.

Roman to Integer
Roman Validation
Best Time to Buy and Sell Stock I
K nearest points on the plain
Personal Onsite Questions
Sum of prime numbers till n.
25 bar stools problem.


### 第二道就是dp上楼梯经典题

三、exercise提交了，几天后就打电话叫我面试去，就开始研究他家onsite面经。地里面经不多，再加上glassdoor上看了下，他家就这几道题也不换的，就稍微准备了下。先说说我自己看到的面经里高频的当然就是【25 bar stool】和【求所有小于n的prime number和】的问题。虽然第一题我自己面试时没考，但是准备的时候还是很认真准备了下，因为地里或者glassdoor上都没有好的答案。在我的百般努力下，终于找到个我认为是正确的答案。  
题意：酒吧有25个吧台椅一排，每进来一位新顾客，会自动坐到离所有其他顾客最远的座位，且相邻座位不坐人，那么你要怎么安排第一位客人的位置，使得能坐的顾客最多呢？  
分析：25个座位，隔一个坐一个肯定是最优的坐法，能最多坐13个人。（这里麻烦拿出纸笔画一画啊，座位序号从1开始）如果先安排人坐了1，下个人就会坐25，再下个人坐13，接着就会坐7（or 19），接着就是坐19（or 7），然后就是4，10，16，22。由此可见这种方法肯定不是最优的。因为你先安排人坐了1，最后3号座位就没法坐了。所以为了保证3号位有人坐，那就的保证5号位有人坐；为了保证5号位有人坐，就得保证9号位有人坐；为了保证9号位有人坐，就得保证17号位有人坐。17再翻倍大于25了，所以循环结束，返回17号座位是第一个被安排的座位。所以每次循环从3开始，直到数字不能再翻倍，并且此方法适用于任何非25的数字，所以follow up也解决了。伪代码如下：  
int findFirstSeat(int totalSeats){  
if (totalSeats <= 2) return 1;  
int firstSeat = 3；  
while (firstSeat * 2 - 1 < totalSeats){  
firstSeat = firstSeat * 2 - 1;  
}  
return firstSeat;  
}  
写了这么多，然而我面试并没有考这题（无奈）。面试主要三环节：hr介绍公司然后一起吃午饭，第一轮面试1小时，第二轮面试1小时。


第一轮，我考的是prime number求和。自己写出来后，面试官给我说了一个方法叫我实现，就是先new一个数组，把所有的数都初始化“是prime number”，然后开始循环，从2开始，把2加到sum里面后，把所有小于n的2的倍数标记为“不是prime number”，然后继续循环；把3加到sum后，把所有小于n的3的倍数标记为“不是prime number”；然后4是2的倍数，被标记过了，不加；然后5没有被标记，加进sum，然后5的倍数标记为“不是prime number”一直循环循环。。。。。。然后我按照他的要求写了，结果叫我分析时间复杂度的时候我懵逼了，因为本来面试就比较紧张，然后又稍微没弄懂内层循环标记倍数那里的时间复杂度到底应该怎么算，就卡壳了好久，然后人家等了半天就说，算了吧。所以当时就觉得完了，要挂了。后来也没继续想这个问题，麻烦哪个大神看到了告诉告诉我，内层标记2、3、5、7……的倍数那部分的复杂度怎么算啊？接着就是问简历，宝宝前两个project都还挺熟悉的，结果他不问，挑了个第三个project， which已经做了很久，记不太清了。扯了半天，自己都记不清，怎么给人家讲得懂嘛？！心里又想要挂了。。。然后就问behavior了，你有没有在很有限information的情况下完成一件任务的经历；你有没和队友产生过conflict，最后怎么解决的。第一轮结束，感觉到心灰意冷。。。  
  
在会议室等着第二轮的人来面我，并且调整状态，做好心理建设。  
  
第二轮，是两个年轻中国engineer来面的，估计也就比我大一两届吧最多，感觉一下就没那么紧张了。因为上一轮已经问过简历，就直接开始写题。第一题，给你一个排过序的数组，里面有duplicates，你怎么找到某个target数字在这个数组中第一次出现的地方e.g. nums[] = {1112223335567899}，target == 3，则return 6。binary search解决，然后叫分析时间空间复杂度。 第二题，climbing stairs问题，一道简单DP，同样写出来后分析时间空间复杂度（https://leetcode.com/problems/climbing-stairs/）。写出来后叫我优化空间复杂度，用滚动指针，即求当前状态的值只依赖前两个状态的值,没必要保留所有状态的值。所以f[i % 2] = f[(i - 1) % 2] + f[(i - 2) % 2]即可。完成后还有时间，年轻工程师也不造问啥问题好，就临时想了几个问题问了问。比如我会不会android，答：不会；会不会数据库，答：不会（我这种弱渣啊。。。）。但是这种时候，你一定要表达出的意向是，虽然我不会，但是我有一颗愿意学习的心啊，而且学东西很快的~然后问问我abstract class和interface的区别，虽然也懂个大概，但之前没考虑过这个问题，就一边按照自己理解，一边组织语言答得七七八八吧。然后面试结束，被hr送出大楼。
\




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

  
很简单，用两次binary search分别找到第一次出现的index和最后一次出现的index，求出number of occurrences。时间复杂度是O(logn)  
  
problem solving exercise就是地理经常说的Auto Manufacturing Case，花了挺长时间写这个，我觉得分析的也够细致了。然而挂了，不知道为啥。。。难得找到一个面试相对简单的公司。。。而且在LA。。。  
  
如果是coding部分出问题可能就是题目也没说sorted array就是升序，我的代码就是按照升序来写的，如果是降序就出错。但是我在github上也找到往年这个题的代码，那个人也是按照升序来写的，先找到array[mid] == query的index，再分别往前和往后判断，直到不等于query值。他这个方法实际上如果array很大而且都是query值的时候时间复杂度是n，还没我的方法好。  
  
如果是problem solving exercise出问题的话那我真是没办法了，我已经尽力了，这种没有标准答案的题目不是我说对就是对。而且我还参考了以前面试的人写的答案，他们这个OA是过了的，所以我更不明白为啥了。。。


什么是polymorphism？stack和queue的区别，从头部插入单链表和数组的时间复杂度。


编程题是DynamicProgramming经典题目coinchange



第二题是24点游戏： 给你一组数字和一个目标数字，判断是否能让那一组数字通过四则运算得到目标数字。因为近期leetcode看多了，一直往nlogn复杂度想，没想出来，后来和面试官表面没有想法之后，他直接就把算法给我说了让我写代码。一看算法晕了，居然是brute force。。  
  
还问了一些数据库的基础知识比如index之类的。


restful soap 智力题
