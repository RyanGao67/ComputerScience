# Insertion sort 
### Python implementation
```python
def selection_sort(arr):
    for i in range(1,len(arr)):
        curNum = arr[i]
        j = i-1
        while j>=0 and arr[j]>curNum:
            arr[j+1] = arr[j]
            j-=1
        arr[j+1] = curNum
```
### Explanation
*1: Basic case*
arr = [1], in this case , arr is already sorted 

*2: Now assume arr=[4,5,7,8,9,2]*
Induction: The part of arr to the left of index i is sorted, the part of arr to the right of i is unsorted. 
i = 1
The left part is [4], which is sorted. The right part is [7,8,9,2], which is unsorted. Now, we consider the entry of index i(now i == 1) and expand the left part(sorted part) to include arr[i]. j starts from i-1, so j starts 0. Because arr[j] == 4, which is smaller than arr[i] (also curNum, now is 5), the program does not enter the while loop. arr[j+1] doesn't change. 

i = 2
The left part is [4,5], which is sorted. The right part is [8,9,2], which is unsorted. Now, let's expand the sorted part to include arr[i]. j starts form i-1, so j starts from 1. Because arr[j] == 5, which is smaller than curNum, the program does not enter the while loop. arr[j+1] doesn't change. 

i=3 similar to previous discussion
i=4 similar to previous discussion

i=5
The left part is [4,5,7,8,9],which is sorted.  the right part is []. Now let's expand the sorted part to include arr[i] (arr[i]==2).  j starts from i-1, so j starts from 4. Now, j>=0 and arr[j]>curNum, thus the program enter the while loop. 

curNum==2
After the first time of entering while loop, j becomes 3, arr becomes [4,5,7,8,9,9]. (j>=0 and arr[j]==8 which is > curNum, while loop continues)

Aftert the second time of entering while loop, j become 2, arr become [4,5,7,8,8,9]. (j>=0 and arr[j]>curNum)

After the third time of entering while loop, j become 1, arr become [4,5,7,7,8,9]. (j>=0 and arr[j]>curNum)

After the fourth time of entering while loop, j become 0, arr become [4,5,5,7,8,9] (j>=0 and arr[j]>curNum)

After the fifth time of entering while loop, j become -1, arr become [4,4,5,7,8,9] (j<0, while loop ends.) 

j+1==0, arr[j+1]=curNum, arr becomes [2,4,5,7,8,9].

## Time complexity
O(n*n)
