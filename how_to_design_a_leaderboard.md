## Problem:
How to design a game leaderboard?

## Assumptions
1. To solve the problem in this scenario, instead of using database, the information of interest(rows in the given leaderboard) is stored in a faster memory scheme, e.g. RAM.  
2. In the final solution, I try to implement all the APIs with time complexity better than log(n).
3. The userId is unique for every user (Each LeaderboardRow object has a different userId). 
4. Different users always have different scores. (In real production scenario, different users may have the same score, however, we can always take into consideration factors other than score to get a specific ranking, eg. time. In this task, I make this assumption to make it easy to clarify the core idea.)
5. The score variable is a numeric value and the higher the better. 

## Final solution and analysis
Binary search tree(BST) is the major data structure that I'll use for the problem. However, although BST is quick at searching, it is not quick at ranking. So, I'll modify BST a little bit to make it suitable for the given scenario. Here're the two modifications:
1. Each node in BST stores the size of subtree that has itself as the subtree's root
2. Each node has access to their parent node. 

After the modification, each node in BST stores the following information: userId, score, size of subtree that has itself as the subtree's root, reference to parent, reference to left sub node, reference to right sub node. Thus, we can easily get the following equation:
node.size = node.left.size + node.right.size + 1

Here is an example, 
[img]

Before I discuss the three required APIs, I'll first define a function `search(userId, score)`, which returns the node we want. 
```python
# Algorithm 1
search(userId, score):
    currentNode = root
    while score!=currentNode.score:
        if score>currentNode.score:
            currentNode = currentNode.right
        else:
            currentNode = currentNode.left
    return currentNode
```
In order to take advantage of the BST, both userId and score are needed. It is often the case that the only information provided for search is userId. To get both userId and score, a hash table is necessary. In my design, userId is stored in hash table as key while the corresponding score is stored in hash table as value. I'll name the hash table `idScore`. 

As I've discussed the data structure that I chose for the problem, I'll then discuss the three APIs. 

**void updateByUserId(userId, newScore)**
```python
# Algorithm 2
void updateByUserId(userId, newScore):
    # First utilize the hash table to get the old score
    oldScore = idScore.get(userId)
    # Update the hash table
    idScore.set(userId, newScore)
    # Get the original Node in BST
    originalNode = search(userId, oldScore)
    # Delete the original node in BST
    # For simplicity I omit details here for deleteNode() for now, 
    # We can easily delete a node in BST with time complexity of O(log(n))
    deleteNode(originalNode)
    # Insert new node to BST
    # For simplicity I omit details here for insertNode() for now, 
    # We can easily insert a node in BST with time complexity of O(log(n))     
    insertNode(Node(userId, newScore))
```
Algorithm 2 dose two things, first one is updating the hash table idScore and the second one is updating the BST. Updating BST can be seperated into two steps, first deleting the original node, and then insert the new node. Updating the hash table has a time complexity of O(1). Searching the original node and deleting it, inserting new node to BST have a time complexity of O(log(n)). Thus **updateByUserId** has a overall time complexity of O(log(n)). 

**LeaderboardRow fetchByUserId(userId)**
```python
Algorithm 3
LeaderboardRow fetchByUserId(userId):
    # Utilize the hash table to get score
    score = idScore(userId)
    # Find the node of given userId
    node = search(userId, score)
    # Here I assume the userId is valid, thus I didn't handle the exception of invalid node
    # null.size == 0
    rank = node.right.size+1
    while node!=root:
        if node is node.parent.left:
            rank = rank+node.parent.right.size+1
        node = node.parent
    return [rank, userId, score]
```

The idea here is as following:
We can easily get the userId and score from given information and hash table. The only hard part is getting the rank. It is obvious that: rank = number of players who have higher score + 1. So I initialized rank as `node.right.size+1`, where `node.right.size` count the right sub tree nodes and all these nodes have a higher score.  
Then the while loop go up through the BST until the node is the root. When the node is its parent's left sub node, it means that its parent and all nodes in its parent's right subtree have a larger score. In this case, `rank = rank+node.parent.right.size+1`.
`LeaderboardRow` has a time complexity of O(log(n)), because `search()` and `while loop` both have a time complexity of O(log(n)). 

**LeaderboardRow fetchByRank(targetRank)**
```python
# Algorithm 4
LeaderboardRow fetchByRank(targetRank):
    # I assume targetRank is always valid
    # targetRank is an integer and 0 < targetRank <= root.size 
    node = root
    count = targetRank
    while targetRank!=0:
        # null.size == 0
        whiles node.size >= targetRank:
            node = node.right
        targetRank = targetRank - node.size
        if targetRank > 1:
            targetRank = targetRank-1
            node = node.parent.left
        else:
            return [count, node.parent.id, node.parent.score]
```
The idea here is first always going down to the right sub node through the BST from root, until the size of current node is smaller than targetRank. I get the following inference:
*(1)node.size < targetRank*
*(2)node.parent.size >= targetRank*
Beause `node` is the sub right node of `node.parent`, the scores of nodes in the subtree rooted at `node` are all bigger than the score that we are looking for. Thus the node we are looking for can only be `node.parent` or in the sub tree rooted at `node.parent.left`.

Algorithm 4 starts from root, and only goes down along the BST, thus it is obvious the time complexity is O(log(n)). 

## Rejected solution and analysis
An alternative approach is using linked list. Each node in linked list has userId, score. Here linked list is sorted by score of each node. 

For update, we need to first locate the node(time expense is O(n)). Then we need to delete the old node from the linked list. Then we can update the deleted node with new score and insert the updated node to the linked list(time expense is O(n)). It is obvious that the time complexity is O(n) for **void updateByUserId(userId, newScore)**.

For **LeaderboardRow fetchByUserId(userId)**, we need to go through the linked list to find the node with given userId. Also we need to count how many nodes are ahead of target node to get the rank.(time expense is O(n))

For **LeaderboardRow fetchByRank(targetRank)**, we need to maintain a `count` variable, initialized as 0. Then we go through the linked list and we add 1 to `count` for each node until `count==targetRank`. It is obvious that the time complexity is O(n). 

As the size of leaderboard is huge, O(n) time complexity is unacceptable for the given scenario. 

## Limitations
As there are up to 10 million rows per leaderboard, there could be large expense spent on RAM because we need to store additional information in BST node.

The data structure and algorithm are more efficient when the BST is balanced. When the BST is very unbalanced, the performance could be poor. However, we can use Red-Black Tree scheme to always self-balance of the binary search tree. 
