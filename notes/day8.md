# leetcode 344. Reverse String

## Thinking Reocrd:
Idea:\
in-place modifying is the key point for this problem.\
The first thought is to swap between the first and the last data. (like using two pointer in C++ and change their value.)

## Submissions:
![Alt text](../__assets__/pic/lc_344_1.png?raw=true "lc_344_1")

## Summary from the Lecture:
video source: [代码随想录-反转字符串](https://www.bilibili.com/video/BV1fV4y17748)

```python
class Solution:
    def reverseString(self, s):
        s.reverse()
```

## Complexity Analysis:
- Time complexity: O(n) to swap n/2 element.
- Space complexity: O(1), it's a constant space solution.

# leetcode 541. Reverse String II

## Thinking Reocrd:
Idea:\
Use two pointer, to swap the data.\
If 2k < length, right = 2k - 1; If 2k >= length, right = length -1

WRONG: in this question, s is a string not a list

WRONG: i wrongly understand the problem. This question means for each 2k charaters we should do the reverse operation.

## Submissions:
![Alt text](../__assets__/pic/lc_541_1.png?raw=true "lc_541_1")

## Summary from the Lecture:
video source: [代码随想录-反转字符串 II](https://www.bilibili.com/video/BV1dT411j7NN)

```python
class Solution:
    def reverseStr(self, s: str, k: int) -> str:
        # Two pointers. Another is inside the loop.
        p = 0
        while p < len(s):
            p2 = p + k
            # Written in this could be more pythonic.
            s = s[:p] + s[p: p2][::-1] + s[p2:]
            p = p + 2 * k
        return s
```

## Complexity Analysis:
- Time complexity: O(n) where n is the size of s. We build a helper array, plus reverse about half the characters in s.
- Space complexity: O(n) to store the helper array.

# leetcode 151. Reverse Words in a String

## Thinking Reocrd:
Idea:\
Use two pointer to search for the word from the end.\
right pointer search for the right edge. (lette[right] != space, lette[right+1] != space)\
left pointer search for the left edge.\
Then append the new list with letter[left:right] and a space.\
right pointer start from len(s)-1.\
left pointer stop at 0.

## Submissions:
![Alt text](../__assets__/pic/lc_151_1.png?raw=true "lc_151_1")

## Summary from the Lecture:
video source: [代码随想录-翻转字符串里的单词](https://www.bilibili.com/video/BV1uT41177fX)

```python
# pythonic
class Solution:
    def reverseWords(self, s: str) -> str:
        # 删除前后空白
        s = s.strip()
        # 反转整个字符串
        s = s[::-1]
        # 将字符串拆分为单词，并反转每个单词
        s = ' '.join(word[::-1] for word in s.split())
        return s
```

```python
# two pointers
class Solution:
    def reverseWords(self, s: str) -> str:
        # 将字符串拆分为单词，即转换成列表类型
        words = s.split()

        # 反转单词
        left, right = 0, len(words) - 1
        while left < right:
            words[left], words[right] = words[right], words[left]
            left += 1
            right -= 1

        # 将列表转换成字符串
        return " ".join(words)
```

## Complexity Analysis:
- Time complexity: O(n) where n is the size of s. We build a helper array, plus reverse about half the characters in s.
- Space complexity: O(n) to store the helper array.