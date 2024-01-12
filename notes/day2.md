# leetcode 977. Squares of a Sorted Array

## Thinking Reocrd:
Idea:
1.  find the value near 0;
2.  use two index to move and compare;
3.  save the smaller abs value in a new arrary;
4.  square the new array.

WRONG CASE:\
nums = [-1, 0]\
nums = [-5,-3,-2,-1]

Improve the code:\
Two pointer.\
Since we know the total length of the final array. We can save the square nums from the biggest.

## Submissions:
![Alt text](../__assets__/pic/lc_977_1.png?raw=true "lc_977_1")

## Summary from the Lecture:
video source: [代码随想录-双指针](https://www.bilibili.com/video/BV1QB4y1D7ep)

## Complexity Analysis:

### Approach 1: Sort
Time complexity : O(NlogN), where N is the length of A.
Space complexity : O(N) or O(logN), depending on the built-in sorting implementation.

### Approach 2: Two Pointer
Time complexity : O(N), where N is the length of A.
Space complexity : O(N) if you take output into account and O(1) otherwise.

# leetcode 209. Minimum Size Subarray Sum

## Thinking Reocrd:
idea:
bruce force:\
i use as the starter of the subarray. Try all the sum with the starter i. If the sum is larger than target. Save it as temp count and try next starter.\
compare the temp count with the old one, refresh the count.\
WRONG: Time Limit Exceeded, 18 / 21 testcases passed


fixed window size:\
main loop is the j in last try.\
we can fix the window and change the starter index. Stop the loop when we match the target.\
WRONG: Time Limit Exceeded, 18 / 21 testcases passed

FAILED TO ANSWER

Try after learning:\
Use a right pointer point to the end of the window.\
loop over the whole array and do the sum.\
Use a left pointer point to the start of the window.\
while the sum bigger than target, loop from the beginning of the array and do the element removal.\
compare the final length to the saved one. Return the value.

## Submissions:
FAILED, Time Limit Exceeded, 18 / 21 testcases passed.

## Summary from the Lecture:
video source: [代码随想录-滑动窗口](https://www.bilibili.com/video/BV1tZ4y1q7XE)

## Complexity Analysis:
### Approach #1 Brute force [Time Limit Exceeded]
Time complexity: O(n^3)\
For each element of array, we find all the subarrays starting from that index which is O(n^2)\
Time complexity to find the sum of each subarray is O(n)\
Thus, the total time complexity : O(n^2 * n) = O(n^3)\
Space complexity: O(1) extra space.
### Approach #4 Using 2 pointers [Accepted]
Time complexity: O(n). Single iteration of O(n).\
Each element can be visited at most twice, once by the right pointer(i) and (at most)once by the left pointer.\
Space complexity: O(1) extra space. Only constant space required for left, sum, ans and i.

TBD: There is a O(nlogn) solution. Need to learn. Using binary search.

# leetcode 59. Spiral Matrix II

## Thinking Reocrd:
Idea:
we need to generate a two dimension matrix.
1^2 1\
2^2 2+1+1\
3^2 3+2+2+1+1\
4^2 4+3+3+2+2+1+1\
5^2 5+4+4+3+3+2+2+1+1\
6^2 6+5+5+4+4+3+3+2+2+1+1\
7^2 7+6+6+5+5+4+4+3+3+2+2+1+1\
In the first try, I write the wrong number for the last two step for odd n.

## Submissions:
![Alt text](../__assets__/pic/lc_59_1.png?raw=true "lc_59_1")

code:
```python
class Solution:
    def generateMatrix(self, n: int) -> List[List[int]]:
        step_dict = {}
        nums = [[0 for j in range(n)] for i in range(n)]
        
        row = 0
        col = -1
        fill = 0

        for i in range(1, 2*n):
            for step in range(n - i//2):
                # nums[row][col] = fill
                if i % 4 == 1:
                    col += 1
                elif i % 4 == 2:
                    row += 1
                elif i % 4 == 3:
                    col -= 1
                elif i % 4 == 0:
                    row -= 1
                fill += 1
                nums[row][col] = fill

        return nums
```

## Summary from the Lecture:
video source: [代码随想录-螺旋矩阵](https://www.bilibili.com/video/BV1SL4y1N7mV)

For each edge of our layers, we fill from the [start, end) of the edge.\
Which can make all the edge fill in the same way.\
Then we can use a loop to fill the matrix.

code:
```python
class Solution:
    def generateMatrix(self, n: int) -> List[List[int]]:
        nums = [[0] * n for _ in range(n)]
        startx, starty = 0, 0               # 起始点
        loop, mid = n // 2, n // 2          # 迭代次数、n为奇数时，矩阵的中心点
        count = 1                           # 计数

        for offset in range(1, loop + 1) :      # 每循环一层偏移量加1，偏移量从1开始
            for i in range(starty, n - offset) :    # 从左至右，左闭右开
                nums[startx][i] = count
                count += 1
            for i in range(startx, n - offset) :    # 从上至下
                nums[i][n - offset] = count
                count += 1
            for i in range(n - offset, starty, -1) : # 从右至左
                nums[n - offset][i] = count
                count += 1
            for i in range(n - offset, startx, -1) : # 从下至上
                nums[i][starty] = count
                count += 1                
            startx += 1         # 更新起始点
            starty += 1

        if n % 2 != 0 :			# n为奇数时，填充中心点
            nums[mid][mid] = count 
        return nums
```
