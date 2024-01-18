# leetcode 242. Valid Anagram

## Thinking Reocrd:
Idea:\
Anagram means rerange all the charaters and using original letters exactly once.\
we can create a hash table for all the letters in s.\
The key  is the letter and the value is the number of this letter in this table.

For the string t. We check the total length. Then check every letter if they are in the table. If in the table decrease the count of the value.\
When all the value are 0, that means anagram string.

## Submissions:
![Alt text](../__assets__/pic/lc_242_1.png?raw=true "lc_242_1")

## Summary from the Lecture:
video source: [代码随想录-有效的字母异位词](https://www.bilibili.com/video/BV1YG411p7BA)

## Complexity Analysis:
### Approach 1: Sorting
- Time complexity: O(NlogN) where N is the length of s or t.
- Space complexity: O(logN) because of sorting.

### Approach 2: Hash Table
- Time complexity: O(N) where N is the length of s or t.
- Space complexity: O(1) because the table's size is fixed.

# leetcode 349. Intersection of Two Arrays

## Thinking Reocrd:
Idea:\
Since it didn't care about the frequecies of the element. So, we can use a hash set for this problem.\
Create a hashset for nums1. Then check if the element in nums2 is in the nums1.\
If in, return to a new hash set to store those unique element.

## Submissions:
![Alt text](../__assets__/pic/lc_349_1.png?raw=true "lc_349_1")

## Summary from the Lecture:
video source: [代码随想录-两个数组的交集](https://www.bilibili.com/video/BV1ba411S7wu)


## Complexity Analysis:
### Approach 1: Two Sets
```python
def set_intersection(self, set1, set2):
    return [x for x in set1 if x in set2]
```
- Time complexity: O(n+m) where n and m are the lengths of the arrays. We iterate through the first, and then through the second array; insert and lookup operations in the hash set take a constant time.
- Space complexity: O(m+n) in the worst case when all elements in the arrays are different.

### Approach 2: Built-in Intersection
```python
def intersection(self, nums1, nums2):
    return list(set(nums1).intersection(set(nums2)))
```
- Time complexity: O(n+m) in the average case and O(n×m) in the worst case when load factor is high enough.
- Space complexity: O(m+n) in the worst case when all elements in the arrays are different.

# leetcode 202. Happy Number

## Thinking Reocrd:
Idea:\
We can design an iterative function, the inputs are original number, past number set.\
If the new calculation based on the original number don't belong to the past number set, we add the new calculation to the number set and return -1.\
If the new calculation belongs to the past number set, we return the calcuation.

## Submissions:
![Alt text](../__assets__/pic/lc_202_1.png?raw=true "lc_202_1")

## Summary from the Lecture:
lecture note source: [代码随想录-快乐数](https://programmercarl.com/0202.%E5%BF%AB%E4%B9%90%E6%95%B0.html)
sloution source: [leetcode-editorial-solution](https://leetcode.com/problems/happy-number/solution/)

## Complexity Analysis:
### Approach 1: Detect Cycles with a HashSet
- Time complexity: O(logn)
- Space complexity: O(logn)

### Approach 2: Floyd's Cycle-Finding Algorithm
- Time complexity: O(logn)
- Space complexity: O(1)

### Approach 3: Mathematical
- Time complexity: O(logn)
- Space complexity: O(1)

# leetcode 1. Two Sum

## Thinking Reocrd:
Idea:\
use a hash map to store the (target - num) as the key and the num as the value.\
Check the num and add element into the map if not found.

## Submissions:
![Alt text](../__assets__/pic/lc_1_1.png?raw=true "lc_1_1")

## Summary from the Lecture:
video source: [代码随想录-两数之和](https://www.bilibili.com/video/BV1aT41177mK)

## Complexity Analysis:
### Approach 1: Brute Force
- Time complexity: O(n^2)
- Space complexity: O(1)

### Approach 2: Two-pass Hash Table
- Time complexity: O(n)
- Space complexity: O(n)

### Approach 3: One-pass Hash Table
- Time complexity: O(n)
- Space complexity: O(n)
