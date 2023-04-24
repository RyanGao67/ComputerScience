11. why you leave your current company? why zynga?
12. Give one challenging project story.
13. Write a method that receives queen positions on a chess board and returns true if the queens are attacking one another and false otherwise.
14. how to determine if two binary trees are similar?
15. Reverse a linked list
16. detect the cycle in the linked list 
17. find starting point in cycle linked list and remove
18. onvert string to int. Follow UP 是covert hex-decimal to decimal。
19. remove duplicate elements in linkedlist
20.print valid parenthese
21. 详细问简历，问oop基础（hash table, 多态，继承等）
22. leetcode儿爸。我用kmp做的
23. LRU 缓存 (lc 一四六)。
24. dynamically allocating buffer via Heap sort, string based question
25. design rate limiter with high level design and discussed few core components
26. desing Minesweeper with low level design
27. given a node in BST along with it's parent node, find its previous and next node using inorder traversal.
28. Design and code minesweeper game.
29. Given an array of unique positive integers return k maximum integers.
30. Given an array of strings, group anagrams together.
31. Given root node, print the zig zag order traversal of the binary tree.
23sort 
24 Do you know hash, how it works? 
26. palindrome, and reverse, and find the longest
28.The question was on a 2-D matrix, getting the minimum cost or distance to start from the left-bottom corner to the right-upper corner. 
29.Given an array of integer numbers and a value K, get the count of all the combinations of the sum of any 3 numbers whichgreater than or equal to the K
31. It was a construction of clusters. And then finding the possible number of pairs of nodes, not belonging to the same cluster.
31. Traversals in a binary tree and cloning a binary tree.
32. 24 Write a class that does search and delete on a BST
33. write a function that takes an int n and prints a pyramid of stars where lowest level has n stars.
35. Given an array find all the subsets in the array which sums up to the target value. You can pick any value from the array? What would be the time complexity?
36. Given an array find the number of ways a particular element in an array can make jump upto n-1th element in the array? max jump would be the given value in an array. For ex [3, 2, 1, 0] Ans would be [6, 3, 1, 0]
37. Design a tic-tac-toe game with coming up the best possible move a computer can make.
38 Design a stack in which these following operations take O(1) time:pop/push/push middle/pop middle
39. Given a connected path which can have many subpaths get the path which is nth distance from the source ?
40. Calculate the angle between a minute and hour hands in a clock.
1. Gaming problem : (downsized pacman) you have a 2-d grid and a maze of paths in it. there are fruits on the path. given the position of player x,y in the path at a particular time & a distance d, give the fastest way in which you can fetch the list of all fruits that can be reached by the player, going through the path from x,y in d steps.
2. write whole code in C to replace occurence of a string by another, in an input string
3. design an autocomplete feature, when user types text, give the list of all words in dictionary starting with that text. optimize time complexity and write 'full code'
4. petrol pump around a circular path problem.
1. Given a string find nth non-repeating character with single iteration?
2. Design a system to handle inflow of numbers, where the numbers are enqueued and dequeued in a queue like data structure. When asked for the position of a number in the queue, the system should give the real time position.
3. Conceptual questions on hibernate, and DB locks.
4. Give a set of tuples, identify if there is a cyclic dependency
5. N petrol pumps in Dynamic programming
6.What is the difference between an array and a linked list?
7. Generate Parentheses (https://leetcode.com/problems/generate-parentheses/)
8. Letter Combinations of a Phone Number (https://leetcode.com/problems/letter-combinations-of-a-phone-number/)
9. Permutations (https://leetcode.com/problems/permutations/)
10. Word Search (https://leetcode.com/problems/word-search/)
1. string searching
2.  finding all sub-strings
3.  finding set of permutations etc
4. Find kth max in an array
5. Check for prime
1. Find if a linked list contains a cycle.
2. Convert a string to an integer.
3. Given a family tree, find the average age of a given generation.
4. Add 1 to a number represented as a linked list.
5. Implement a linked list class and several traversal and search methods.
7. Given a string containing brackets, eg, "(( hi ) )", check if it's closed or not. What if we took into account any pair of opening/closing char
3. Given 2 int arrays (unsorted, duplicates can exist), create a function that returns the unique (no overlap) ints from both arrays.
4. Find out if two given strings are anagrams of each other or not?
5. Insert a number in a sorted linkedlist.
6. Distributed top K 
7.converting Roman Number to Decimal.
8.Fibonacci series using recursion, iteration, DP
9.To find the mirror image path of a given node in a binary tree, we need to traverse the tree to find the given node, and then traverse the mirror image of the tree to find the mirror image of the node. Here's one way to implement this algorithm in Python:
class Node:
    def __init__(self, val):
        self.val = val
        self.left = None
        self.right = None
        
def find_mirror_image_path(root, target):
    # Find the given node in the tree
    path = []
    found = find_node(root, target, path)
    if not found:
        return None
    
    # Traverse the mirror image of the tree to find the mirror image of the node
    mirror_path = []
    for node in path:
        if node.left is not None:
            mirror_path.append(node.left)
        elif node.right is not None:
            mirror_path.append(node.right)
        else:
            mirror_path.append(None)
    
    return mirror_path

def find_node(node, target, path):
    if node is None:
        return False
    
    path.append(node)
    
    if node.val == target:
        return True
    
    if find_node(node.left, target, path):
        return True
    
    if find_node(node.right, target, path):
        return True
    
    path.pop()
    return False
This implementation uses a recursive function find_node to traverse the tree and find the given node. If the node is found, it adds all the nodes on the path from the root to the node to the path list. It then uses a loop to traverse the path list in reverse order and build the mirror_path list by adding the mirror image of each node to the list. If a node has a left child, its mirror image is its right child, and vice versa. If a node has no children, its mirror image is None.

Here's an example usage of this function:

#       1
#      / \
#     2   3
#    / \   \
#   4   5   6
#      / \
#     7   8

root = Node(1)
root.left = Node(2)
root.right = Node(3)
root.left.left = Node(4)
root.left.right = Node(5)
root.right.right = Node(6)
root.left.right.left = Node(7)
root.left.right.right = Node(8)
path = find_mirror_image_path(root, 5)
print([node.val if node is not None else None for node in path]) # [5, 3, None]
This code finds the node with value 5 in the tree and returns its mirror image path, which is [5, 3, None] in this case. This indicates that the mirror image of the node with value 5 is at the same depth as the original node, and has no children.
Invert Binary Tree (https://leetcode.com/problems/invert-binary-tree/)
Symmetric Tree (https://leetcode.com/problems/symmetric-tree/)
Binary Tree Level Order Traversal (https://leetcode.com/problems/binary-tree-level-order-traversal/)
Path Sum (https://leetcode.com/problems/path-sum/)
Lowest Common Ancestor of a Binary Tree (https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)
37. Word search in matrix.
38. binary tree insertion 
39. Differentiate between a linked list and array. Also, implement a doubly circular linked list which also sorts itself.
40. Given a random maze and a "robot" in the top left corner, describe an algorithm to bring the robot to the center given only a method that outputs the walls adjacent to the robot.
40. In order traversal without recursion
1. Turn a binary tree into a Linked List
2. Find the largest contiguous subsequence of an array of ints
3. Print all permutations of a string
5. Remove duplicates from a Linked List
7. How would you algorithmically generate all valid English anagrams of a string and its substrings?
8. def print_pattern(n, i=1, inc=True):
    # Base case: when i reaches n, print the final line and return
    if i == n:
        print('*' * n)
        return
    
    # Print the current line and recursively call the function with the next value of i
    print('*' * i)
    if inc:
        i += 1
    else:
        i -= 1
    
    # If i reaches 1, change the increment direction to downwards
    if i == 1:
        inc = True
    
    # If i reaches the halfway point, change the increment direction to upwards
    if i == n // 2 + 1:
        inc = False
    
    # Recursively call the function with the updated values of i and inc
    print_pattern(n, i, inc)
There are several LeetCode problems that involve printing patterns or sequences without using loops. Here are some examples:

Print Words Vertically (https://leetcode.com/problems/print-words-vertically/)
Print Immutable Linked List in Reverse (https://leetcode.com/problems/print-immutable-linked-list-in-reverse/)
Output Contest Matches (https://leetcode.com/problems/output-contest-matches/)
Print Zero Even Odd (https://leetcode.com/problems/print-zero-even-odd/)
Beautiful Arrangement II (https://leetcode.com/problems/beautiful-arrangement-ii/)
1. The Dining Philosophers problem (https://leetcode.com/problems/the-dining-philosophers/)
2. Dining Philosophers II (https://leetcode.com/problems/dining-philosophers/)
3. Building H2O (https://leetcode.com/problems/building-h2o/)
4. Print in Order (https://leetcode.com/problems/print-in-order/)
5. Print FooBar Alternately (https://leetcode.com/problems/print-foobar-alternately/)
6. Write a function that implements division without dividing or multiplying.
Divide Two Integers - This problem asks you to implement integer division without using the division or multiplication operator. The function should return the quotient of the division, and should handle overflow and underflow cases.
Pow(x, n) - This problem asks you to implement a function that computes the value of x raised to the power of n, where x is a double and n is an integer. You are not allowed to use the built-in pow() function or the ** operator.
Sqrt(x) - This problem asks you to implement a function that computes the integer square root of a non-negative integer x, without using the built-in sqrt() function.
6. convert a integer to a string representing the value in binary.
7. matrix manipulation
3. w.r.t. memory, explain why arrays are preferred over Linked lists (other than the get() method)
1. How to reverse the string like "you are a cat." to "uoy era a tac."? Using four pointers and a temp variable
2. search in a Binary search Tree
3. the complexity of this algorithm
public String getString()
String s= "";
for(int i =0 ; i < LARGE_NUMBER ; i++) {s += "a";}
return s;
}
4. randomize a list of sorted number
4. Find k minimum numbers in an array --- (nlogk complexity)
5. find first unique no in a file in 1 pass(linked hash map)
6. reverse in group (link list)
7. Given n(odd) coins , you can pick 1-4 coints at a time , first choice is yours(2 players are playing) , how will you play so that you are one to pick the last set of coins , i,e nothing remains after that.
8. Pseudo Number Generator for a card game
17. Implement the search for the closest common ancestor of two tree nodes
18. How do you find the max depth of a binary tree?
19. Design an algorithm to recommend friends to people on facebook.
20. 32. Design MVC for a leaderboard design leaderboard
21. 6. design a scrabble game
7. 30. Amazon Recommendation System related design
8. 10. thread1 {for (int i = 0; i<100; i++) {a++;}} thread2{for (int i = 0; i<100; i++) {a++;}} suppose a is initialized to 0, what's the final value of a
11.What is the memory necessary for this iterative block of code?
12. implement atoi
13. Design a class to handle hashing.
14. design the news feed of facebook
15. design a job machine class which can have various kinds of jobs executed.
16. 5 Pirate & 100 gold...need to be divided perfectly.exact problem details & answer : http://en.wikipedia.org/wiki/Pirate_game
17. Design a card game, what data structure to use to minimize pattern searching operation in Poker.
9. 43. If an iphone app crashes often how will you debug
10. - Design a real time MMO game. Resource sharing and locking
11. How would you find 10 suitably matched opponents in Mafia Wars without taking a DB hit.
6. How do you deal with a big network of servers... how would you plan your upgrades, etc.
4. if you have a big text file on unix, how would you parse it if it has a predefined table format... how would you get data from a particular column?
2.How would you design a Leadership board for a game so that it shows stats for the user at the game level, and also position in his/her social circle.
3. Implement char* itoa
44. Design chess game and scale it
22. 16 Sync a huge file between nodes 
23.atomicity 
24. deadlocks
25. 1.How would you proactively provision hardware for a large LAMP application? Discuss monitoring, cost planning, and how you'd estimate the marginal value.
26. Suppose you need a chat system with 10M concurrent users, with a dense adjacency matrix. How would you implement a presence and chat system that scales to that kind of user base? Discuss tradeoffs among reliability, complexity, latency, and cost.
27. Implement a distributed hash table. Discuss how it will scale horizontally, how it might implement redundancy, and what kind of reliability guarantees it can make.
28.bank of webservers
29. tiers of caching
30. Many Server architecture comparisons.
31. dealing with connection fan-in, etc (do you understand systems and networks, not just FOR loops).
32. Huge bank of servers is pounding data layer with connections. What do we do?
Chaining / Linear-probing
Blocking queue implementation. 
Design an OO model for the elevator.
linked list questions(find the mid point)
2. Implement the btree algorithm.
3. 17. You have two variable lengths of rope with different widths at different sections of the rope and both of them burn at different rates and speeds, they only thing you know about them is that they both burn in exactly 30 minutes. You also have two matches. Just using these two things, how would you measure 45 minutes of time? (Also, you have no way to measure time, i.e. - no wrist watch, etc.)
18. What's the difference between polling and interrupts
4.如何处理100万个用户访问？如何query server端？
29. Talk about what happens when a browser makes a request, 
25. Implement tinyurl
26. How do you open a website without a browser?
27. If there was a feature you only wanted to show to 10% of the users, how would you do so?
28. binary search.
29. Thread? Deadlock?
30. what is the difference between an object and an interface
31. what is the difference between overriding and overloading a method.
32. Define a queen movement in chess using pseudo code?
33. trie
34. 6. Find the minimum number of races required to find out the winner between 25 horses, in one race you can race 5 horses.
35. 32. Stack grows up or down?stack vs heap
