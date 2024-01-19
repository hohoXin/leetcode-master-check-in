# leetcode 454. 4Sum II

## Thinking Reocrd:
Idea:\
Using Brute Force to calculate all the total sum, the time complexity is O(n^4)\
A better way is to use a map to store the sum of nums1 and nums2.\
Then iterate on the nums3 and nums4 to check the - sub_sum in the map.\
In this way, the time complexity is O(n^2 + n^2).

WRONG ANSWER:
The dictionary of sum 1 & 2 cannot tell if there multiple choice.\
So I can use the value of the key to store the times it appear.

## Submissions:
![Alt text](../__assets__/pic/lc_454_1.png?raw=true "lc_454_1")

## Summary from the Lecture:
video source: [代码随想录-四数相加 II](https://www.bilibili.com/video/BV1Md4y1Q7Yh)

## Complexity Analysis:
- Time complexity: O(n^2) where n is the length of the array.
- Space complexity: O(n^2) because of the map.

# leetcode 383. Ransom Note

## Thinking Reocrd:
Idea:\
I think I can create two set fot the ransomNote and magazine.\
Then use the operator < on their set.

WRONG: The problem means the magazin should provide enough letters for ransom note.

I can use two array to save how many letter in the ransom note is needed, how many letters the magazines can provide.

## Submissions:
![Alt text](../__assets__/pic/lc_383_1.png?raw=true "lc_383_1")

## Summary from the Lecture:
lecture note source: [代码随想录-赎金信](https://programmercarl.com/0383.%E8%B5%8E%E9%87%91%E4%BF%A1.html)

## Complexity Analysis:
### Approach 1: One Hash Table
- Time complexity: O(m) where m is the length of the magazine.
- Space complexity: O(k)/O(1).\
We build two HashMaps of counts; each with up to kkk characters in them. This means that they take up O(k) space.\
For this problem, because k is never more than 262626, which is a constant, it'd be reasonable to say that this algorithm requires O(1) space.

## Other Solutions from Python

### using array:
```python
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        ransom_count = [0] * 26
        magazine_count = [0] * 26
        for c in ransomNote:
            ransom_count[ord(c) - ord('a')] += 1
        for c in magazine:
            magazine_count[ord(c) - ord('a')] += 1
        return all(ransom_count[i] <= magazine_count[i] for i in range(26))
```
### using counter
```python
from collections import Counter
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        return not Counter(ransomNote) - Counter(magazine)
```

### using built-in count
```python
class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        return all(ransomNote.count(c) <= magazine.count(c) for c in set(ransomNote))
```

# leetcode 15. 3Sum

## Thinking Reocrd:
Idea:\
Use two numbers to create the dictionary. The key is the sum and the value is the list of index i,j.\
What if there multiple choice for a single sum?\
Instead of a list consist of one pair of index. I can use a 2D list to append new possibility into the dictionary.\
After that, we can check the third number with the value. Then loop through the value of the dictionary.\
Which means in a list of list to check the element if equals to the new index.

FAILED:\
I don't know how to delete the duplicate triplets in the final nested list.\
Even if I use a sorted method suggested by chatgpt, there will be a Time Limit Exceeded error.

## Submissions:
1. FAILED TO let i != j != k
2. FAILED TO delete the duplicate triplets.
3. FAILED, Time Limit Exceeded

## Summary from the Lecture:
video source: [代码随想录-三数之和](https://www.bilibili.com/video/BV1GW4y127qo)

Instead of the hash table, we can use the two pointer method.\
The problem of the hash table is that it cannot delete the duplicate triplets.\
The two pointer method sort the array first. Then use the two pointer to find the sum.\

## Complexity Analysis:

### Approach 1: Two Pointers
- Time complexity: O(n^2). The twoSumII is O(n) and we call it n times.\
Sorting the array takes O(nlogn), so overall complexity is O(nlogn+n^2). This is asymptotically equivalent to O(n^2).\
- Space complexity: O(logn) to O(n), depending on the implementation of the sorting algorithm. For the purpose of complexity analysis, we ignore the memory required for the output.
### Approach 2: Hashset
- Time complexity: O(n^2). We have outer and inner loops, each going through n elements.\
Sorting the array takes O(nlogn), so overall complexity is O(nlogn+n^2). This is asymptotically equivalent to O(n^2).\
- Space complexity: O(n) for the hashset.\
### Approach 3: no-sort
- Time complexity: O(n^2). We have outer and inner loops, each going through n elements.\
While the asymptotic complexity is the same, this algorithm is noticeably slower than the previous approach. Lookups in a hashset, though requiring a constant time, are expensive compared to the direct memory access.
- Space complexity: O(n) for the hashset/hashmap.
For the purpose of complexity analysis, we ignore the memory required for the output. However, in this approach we also store output in the hashset for deduplication. In the worst case, there could be O(n^2) triplets in the output, like for this example: [-k, -k + 1, ..., -1, 0, 1, ... k - 1, k]. Adding a new number to this sequence will produce n / 3 new triplets.

# leetcode 18. 4Sum

## Thinking Reocrd:
Idea:\
This problem ask us to return all of the unqique quadruplets with distinct a,b,c,d very similar to 3sum problem.\
We can seperate it into two 2sum problem. But we deal with it with 4 pointer.\
We still need to do the sort first so that we can avoid some duplicated situation.

Code:
```python
class Solution:
    def twosum(self, nums: List[int], a: int, b: int, sub_sum: int, res: List[List[int]]):
        c = b + 1
        d = len(nums) - 1
        while(c < d):
            if (nums[c] + nums[d]) < sub_sum:
                c += 1
            elif (nums[c] + nums[d]) > sub_sum:
                d -= 1
            else:
                res.append([nums[a], nums[b], nums[c], nums[d]])
                c += 1
                d -= 1
                while(c < d and nums[c-1] == nums[c]):
                    c += 1
                while(c < d and nums[d] == nums[d+1]):
                    d -= 1
            

    
    def fourSum(self, nums: List[int], target: int) -> List[List[int]]:
        nums.sort()
        res = []
        for a in range(len(nums)-3):
            if a == 0 or nums[a-1] != nums[a]:
                for b in range(a+1, (len(nums)-2)):
                    if b == a+1 or nums[b-1] != nums[b]:
                        sub_sum1 = nums[a] + nums[b]
                        sub_sum2 = target - sub_sum1
                        self.twosum(nums, a, b, sub_sum2, res)
                    b += 1
            a += 1
    
        return res
```

## Submissions:
![Alt text](../__assets__/pic/lc_18_1.png?raw=true "lc_18_1")

## Summary from the Lecture:
video source: [代码随想录-四数之和](https://www.bilibili.com/video/BV1DS4y147US)

## Complexity Analysis:
### Approach 1: Two Pointers
- Time complexity: O(n^(k-1)) where k is the number of elements in the array. For k = 4, the complexity is O(n^3). We have two nested loops and twoSum is O(n).\
Note that for k > 2, sorting the array does not change the overall time complexity.\
- Space complexity: O(n). We need O(k) space for the recursion. k can be the same as n in the worst case for the generalized algorithm.\
Note that, for the purpose of complexity analysis, we ignore the memory required for the output.
### Approach 2: Hashset
- Time complexity: O(n^(k-1)) where k is the number of elements in the array. For k = 4, the complexity is O(n^3). We have two nested loops and twoSum is O(n).\
Note that for k > 2, sorting the array does not change the overall time complexity.\
- Space complexity: O(n). The space needed for the recursion will not exceed O(n).