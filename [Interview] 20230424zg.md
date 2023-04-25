first linkedlist/deduplicate array/braket

fibonaci/binary tree

1. N-Queens: https://leetcode.com/problems/n-queens/
2. N-Queens II: https://leetcode.com/problems/n-queens-ii/
3. Valid Sudoku: https://leetcode.com/problems/valid-sudoku/
4. Minimum Knight Moves: https://leetcode.com/problems/minimum-knight-moves/
5. Word Search: https://leetcode.com/problems/word-search/
6. Same Tree: https://leetcode.com/problems/same-tree/
7. Symmetric Tree: https://leetcode.com/problems/symmetric-tree/
8. Subtree of Another Tree: https://leetcode.com/problems/subtree-of-another-tree/
9. Diameter of Binary Tree: https://leetcode.com/problems/diameter-of-binary-tree/
10. Binary Tree Maximum Path Sum: https://leetcode.com/problems/binary-tree-maximum-path-sum/
11. Reverse Linked List: https://leetcode.com/problems/reverse-linked-list/
12. Remove Nth Node From End of List: https://leetcode.com/problems/remove-nth-node-from-end-of-list/
13. Merge Two Sorted Lists: https://leetcode.com/problems/merge-two-sorted-lists/
14. Palindrome Linked List: https://leetcode.com/problems/palindrome-linked-list/
15. Linked List Cycle: https://leetcode.com/problems/linked-list-cycle/16. detect the cycle in the linked list 
16. Linked List Cycle II: https://leetcode.com/problems/linked-list-cycle-ii/
17. Remove Linked List Cycle: https://leetcode.com/problems/remove-linked-list-cycle/
18. https://leetcode.com/problems/convert-a-number-to-hexadecimal/
19. Remove Duplicates from Sorted List: https://leetcode.com/problems/remove-duplicates-from-sorted-list/
20. Remove Duplicates from Sorted List II: https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii/
21. Remove Duplicates from Unsorted List: https://leetcode.com/problems/remove-duplicates-from-unsorted-list/
22. Generate Parentheses: https://leetcode.com/problems/generate-parentheses/
23. Valid Parentheses: https://leetcode.com/problems/valid-parentheses/
24. Different Ways to Add Parentheses: https://leetcode.com/problems/different-ways-to-add-parentheses/
25. https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/solutions/?orderBy=most_votes
26. https://leetcode.com/problems/lru-cache/
27. https://leetcode.com/problems/sort-an-array/
28. https://leetcode.com/problems/kth-largest-element-in-an-array/
29. https://leetcode.com/problems/top-k-frequent-elements/
30. https://leetcode.com/problems/inorder-successor-in-bst/
31. https://leetcode.com/problems/inorder-successor-in-bst-ii/
32. https://leetcode.com/problems/binary-search-tree-iterator/
33. https://www.lintcode.com/problem/915/
34. https://leetcode.com/problems/kth-largest-element-in-an-array/
35. https://leetcode.com/problems/top-k-frequent-elements/
36. https://leetcode.com/problems/find-k-closest-elements/
37. https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/
38. https://leetcode.com/problems/k-closest-points-to-origin/
39. https://leetcode.com/problems/group-anagrams/
40. https://leetcode.com/problems/find-all-anagrams-in-a-string/
41. https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/.
42. https://leetcode.com/problems/sort-list/
43. https://leetcode.com/problems/sort-an-array/ 
44. https://leetcode.com/problems/longest-palindromic-substring/
45. https://leetcode.com/problems/longest-palindromic-subsequence/
46. https://leetcode.com/problems/palindromic-substrings/
47. https://leetcode.com/problems/minimum-path-sum/
48. https://leetcode.com/problems/unique-paths/
49. https://leetcode.com/problems/unique-paths-ii/
50.Given an array of integer numbers and a value K, get the count of all the combinations of the sum of any 3 numbers whichgreater than or equal to the K
def count_combinations(arr, K):
    arr.sort()
    count = 0
    for i in range(len(arr)):
        j, k = i+1, len(arr)-1
        while j < k:
            if arr[i] + arr[j] + arr[k] >= K:
                count += k-j
                k -= 1
            else:
                j += 1
    return count
51. https://leetcode.com/problems/combination-sum-ii/
52. https://leetcode.com/problems/combination-sum/
53. https://leetcode.com/problems/4sum/
54. https://leetcode.com/problems/3sum/
35. https://leetcode.com/problems/evaluate-division/
56. https://leetcode.com/problems/accounts-merge/
57. https://leetcode.com/problems/longest-consecutive-sequence/
58. https://leetcode.com/problems/number-of-provinces/.
59. https://leetcode.com/problems/insert-into-a-binary-search-tree/
60. https://leetcode.com/problems/search-in-a-binary-search-tree/
61. https://leetcode.com/problems/kth-smallest-element-in-a-bst/
62. https://leetcode.com/problems/partition-equal-subset-sum/
63. https://leetcode.com/problems/combination-sum/
64. https://leetcode.com/problems/jump-game-ii/
65. https://leetcode.com/problems/jump-game-iii/
66. https://leetcode.com/problems/jump-game-iv/
67. https://leetcode.com/problems/design-tic-tac-toe/
68. https://leetcode.com/problems/valid-tic-tac-toe-state/ 
69. https://leetcode.com/problems/find-winner-on-a-tic-tac-toe-game/
70. https://leetcode.com/problems/min-stack/ 
71. https://leetcode.com/problems/shortest-path-in-a-grid-with-obstacles-elimination/
72. https://leetcode.com/problems/angle-between-hands-of-a-clock/
73. https://leetcode.com/problems/the-maze/
74. https://leetcode.com/problems/shortest-path-in-a-grid-with-obstacles-elimination/
75. https://leetcode.com/problems/the-maze-ii/
76. https://leetcode.com/problems/the-maze-iii/
77. https://leetcode.com/problems/gas-station/
78. https://leetcode.com/problems/first-unique-character-in-a-string/
79. https://leetcode.com/problems/design-circular-queue/
80. https://leetcode.com/problems/design-hashmap/
81. https://leetcode.com/problems/course-schedule-ii/
82. Generate Parentheses (https://leetcode.com/problems/generate-parentheses/)
83. Letter Combinations of a Phone Number (https://leetcode.com/problems/letter-combinations-of-a-phone-number/)
84. Permutations (https://leetcode.com/problems/permutations/)
85. Word Search (https://leetcode.com/problems/word-search/)
86. https://leetcode.com/problems/average-of-levels-in-binary-tree/
87. https://leetcode.com/problems/cousins-in-binary-tree/
88. https://leetcode.com/problems/binary-tree-right-side-view/
89. https://leetcode.com/problems/plus-one-linked-list/
90. https://leetcode.com/problems/design-linked-list/
91. https://leetcode.com/problems/intersection-of-two-arrays/
92.https://leetcode.com/problems/insertion-sort-list/
93. https://leetcode.com/problems/top-k-frequent-elements/
94. https://leetcode.com/problems/top-k-frequent-words/
95. https://leetcode.com/problems/find-median-from-data-stream/
96.https://leetcode.com/problems/roman-to-integer/
97.To find the mirror image path of a given node in a binary tree, we need to traverse the tree to find the given node, and then traverse the mirror image of the tree to find the mirror image of the node. Here's one way to implement this algorithm in Python:
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
98.Invert Binary Tree (https://leetcode.com/problems/invert-binary-tree/)
99. Symmetric Tree (https://leetcode.com/problems/symmetric-tree/)
100. Binary Tree Level Order Traversal (https://leetcode.com/problems/binary-tree-level-order-traversal/)
101. Path Sum (https://leetcode.com/problems/path-sum/)
102. Lowest Common Ancestor of a Binary Tree (https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)
103. Word search in matrix. https://leetcode.com/problems/word-search/
104. https://leetcode.com/problems/insert-into-a-binary-search-tree/
105. https://leetcode.com/problems/complete-binary-tree-inserter/
106. https://leetcode.com/problems/course-schedule-ii/
107. https://leetcode.com/problems/path-with-maximum-gold/
108. https://leetcode.com/problems/shortest-path-in-a-grid-with-obstacles-elimination/
109. https://leetcode.com/problems/walls-and-gates/
110.https://leetcode.com/problems/binary-tree-postorder-traversal/
111. https://leetcode.com/problems/binary-tree-inorder-traversal/
112. https://leetcode.com/problems/binary-tree-postorder-traversal/
113. https://leetcode.com/problems/binary-tree-preorder-traversal/
114. https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/
115. https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/
116. https://leetcode.com/problems/flatten-binary-tree-to-linked-list/
117.https://leetcode.com/problems/maximum-subarray/
118.https://leetcode.com/problems/letter-combinations-of-a-phone-number
119. https://leetcode.com/problems/permutations/
120. https://leetcode.com/problems/palindrome-permutation-ii/
121. https://leetcode.com/problems/maximum-subarray/
122. Print Words Vertically (https://leetcode.com/problems/print-words-vertically/)
123.  Immutable Linked List in Reverse (https://leetcode.com/problems/print-immutable-linked-list-in-reverse/)
124.  Output Contest Matches (https://leetcode.com/problems/output-contest-matches/)
125.  Print Zero Even Odd (https://leetcode.com/problems/print-zero-even-odd/)
126.  Beautiful Arrangement II (https://leetcode.com/problems/beautiful-arrangement-ii/)
127. The Dining Philosophers problem (https://leetcode.com/problems/the-dining-philosophers/)
128. Dining Philosophers II (https://leetcode.com/problems/dining-philosophers/)
129. Building H2O (https://leetcode.com/problems/building-h2o/)
130. Print in Order (https://leetcode.com/problems/print-in-order/)
131. Print FooBar Alternately (https://leetcode.com/problems/print-foobar-alternately/)
132. https://leetcode.com/problems/divide-two-integers/
133. 26. https://leetcode.com/problems/string-to-integer-atoi/
134. https://leetcode.com/problems/random-pick-with-weight/
135. https://leetcode.com/problems/shuffle-an-array/
136. https://leetcode.com/problems/reverse-words-in-a-string/
137. https://leetcode.com/problems/middle-of-the-linked-list/
138. 45. binary search.
139. 50. trie
140. the complexity of this algorithm
public String getString()
String s= "";
for(int i =0 ; i < LARGE_NUMBER ; i++) {s += "a";}
return s;
}
4. https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/
5. https://leetcode.com/problems/first-unique-number/
6. https://leetcode.com/problems/reverse-linked-list-ii/
7. https://leetcode.com/problems/reverse-nodes-in-k-group/
8. https://leetcode.com/problems/nim-game/
9. https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/
17.https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/
18. https://leetcode.com/problems/maximum-depth-of-binary-tree/
19. https://leetcode.com/problems/minimum-depth-of-binary-tree/
20. https://leetcode.com/problems/insert-into-a-sorted-circular-linked-list/


21. Design MVC for a leaderboard design leaderboard
22. design a scrabble game
23. Amazon Recommendation System related design
24. thread1 {for (int i = 0; i<100; i++) {a++;}} thread2{for (int i = 0; i<100; i++) {a++;}} suppose a is initialized to 0, what's the final value of a
25.What is the memory necessary for this iterative block of code?
61. w.r.t. memory, explain why arrays are preferred over Linked lists (other than the get() method)
28. Conceptual questions on hibernate, and DB locks.
29. design the news feed of facebook
30. Design and code minesweeper game.
31. design a job machine class which can have various kinds of jobs executed.
32. 5 Pirate & 100 gold...need to be divided perfectly.exact problem details & answer : http://en.wikipedia.org/wiki/Pirate_game
33. Design a card game, what data structure to use to minimize pattern searching operation in Poker.
34. If an iphone app crashes often how will you debug
35. Design a real time MMO game. Resource sharing and locking
36. How would you find 10 suitably matched opponents in Mafia Wars without taking a DB hit.
37. How do you deal with a big network of servers... how would you plan your upgrades, etc.
38. if you have a big text file on unix, how would you parse it if it has a predefined table format... how would you get data from a particular column?
39. How would you design a Leadership board for a game so that it shows stats for the user at the game level, and also position in his/her social circle.
44. Design chess game and scale it
42. 16 Sync a huge file between nodes 
43.atomicity 
44. deadlocks
45. How would you proactively provision hardware for a large LAMP application? Discuss monitoring, cost planning, and how you'd estimate the marginal value.
46. Suppose you need a chat system with 10M concurrent users, with a dense adjacency matrix. How would you implement a presence and chat system that scales to that kind of user base? Discuss tradeoffs among reliability, complexity, latency, and cost.
47. Implement a distributed hash table. Discuss how it will scale horizontally, how it might implement redundancy, and what kind of reliability guarantees it can make.
48. bank of webservers
49. tiers of caching
50. design an autocomplete feature, when user types text, give the list of all words in dictionary starting with that text. optimize time complexity and write 'full code'
51. Many Server architecture comparisons.
52. dealing with connection fan-in, etc (do you understand systems and networks, not just FOR loops).
53. Huge bank of servers is pounding data layer with connections. What do we do?
Chaining / Linear-probing
Blocking queue implementation. 
Design an OO model for the elevator.
2. Implement the btree algorithm.
3. design rate limiter with high level design and discussed few core components
33. design Minesweeper with low level design
17. You have two variable lengths of rope with different widths at different sections of the rope and both of them burn at different rates and speeds, they only thing you know about them is that they both burn in exactly 30 minutes. You also have two matches. Just using these two things, how would you measure 45 minutes of time? (Also, you have no way to measure time, i.e. - no wrist watch, etc.)

This is a classic puzzle known as the "Burning Ropes" puzzle. Here's one way to solve it:

Light one end of the first rope and both ends of the second rope.
After 15 minutes, the second rope will have burned completely, indicating that 15 minutes have elapsed.
At this point, light the other end of the first rope.
When the first rope has burned completely, 30 minutes will have elapsed (since it was already burning for 15 minutes before the other end was lit).
Therefore, the total time elapsed is 15 + 30 = 45 minutes.
Note that the key insight here is that the ropes don't burn at a constant rate, so simply splitting each rope in half and lighting both halves won't work. By lighting both ends of one rope and one end of the other, we can ensure that they both burn at the same rate and cancel out the variable burning rates of each rope.

36. What's the difference between polling and interrupts
37. 如何处理100万个用户访问？如何query server端？
38. Talk about what happens when a browser makes a request, 
40. Give one challenging project story.
41. Implement tinyurl
43. How do you open a website without a browser?
44. If there was a feature you only wanted to show to 10% of the users, how would you do so?
47. Thread? Deadlock?
48. what is the difference between an object and an interface
49. what is the difference between overriding and overloading a method.
51. Find the minimum number of races required to find out the winner between 25 horses, in one race you can race 5 horses.
52. Stack grows up or down?stack vs heap
