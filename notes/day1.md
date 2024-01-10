# leetcode 704. Binary Search

## Thinking Reocrd:
We can have two pointer: one start from 0 and one start from the last address.\
Everytime, we compare the absolute distance between the two pointer's value to the target value.\
We change the further index = int((higher index + lower index)/2).\
If the new pointer's value == target, return the index.\
If in the end, higher == lower, return -1.

if target < smallest or target > max, return -1.

!!!WRONG!!! The array is not a continuous array!

Change a little:

We can have two pointer: one start from 0 and one start from the last address.\
Everytime we compare the temp_index = int((lower index + higher index)/2) with the target.

If target == temp value:\
return temp_index\
elif target < temp value:\
higher index = temp_index\
else low index = temp_index

new problem: BOUDARY CONDITION!

I meet a dead loop when the target is not in the array.

## Submissions:
![Alt text](../__assets__/pic/lc_704_1.png?raw=true "lc_704_1")

## Summary from the Lecture:
video source: [代码随想录-二分法](https://www.bilibili.com/video/BV1fA4y1o715/)

### Boundary Condition:

1. [left, right]

    When left == right, we still need to check if the value is the target value. So we need to use <=.
    Pay attention to the (left + right) // 2, it may cause overflow. So we can use left + (right - left) // 2.

    ```python
    while left <= right:
        mid = left + (right - left) // 2
        if nums[mid] == target:
            return mid
        elif nums[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    ```

2. [left, right)
    
    ```python
    while left < right:
        mid = left + (right - left) // 2
        if nums[mid] == target:
            return mid
        elif nums[mid] < target:
            left = mid + 1
        else:
            right = mid
    ```

## Complexity Analysis:

Let n be the size of the input array nums.

### Time complexity: O(log⁡n)
nums is divided into half each time. In the worst-case scenario, we need to cut nums until the range has no element, and it takes logarithmic time to reach this break condition.
### Space complexity: O(1)
During the loop, we only need to record three indexes, left, right, and mid, they take constant space.

# leetcode 27. Remove Element

## Thinking Record:

Idea:
Use a pointer to check the value from the begining.\
Use another pointer to indicate the address of the last element.\
Loop the first poiter from from 0 to second pointer's index.\
When the first pointer match the delete value, change its value with the second pointer. Move the second pointer forward.\
Loop on the remove value set.

WRONG: When the second pointer also has a value == val. The switch operation is meaningless.

wrong case:

nums = [3,3], val = 3 \
nums = [2,2,3], val = 2

## Submissions:
![Alt text](../__assets__/pic/lc_27_1.png?raw=true "lc_27_1")

## Summary from the Lecture:
video source: [代码随想录-移除元素](https://www.bilibili.com/video/BV12A4y1Z7LP)

### Two Pointers:
```python
slow = 0
for fast in range(len(nums)):
    if nums[fast] != val:
        nums[slow] = nums[fast]
        slow += 1
return slow
```

## Complexity Analysis:
### Time complexity : O(n).
Assume the array has a total of n elements, both slow and fast traverse at most 2n steps.\
Using a swap method:  we can swap the current element out with the last element and dispose the last one. As my original idea, we may reduce the time in rare remove cases, but the time complexity is still O(n).

### Space complexity : O(1).