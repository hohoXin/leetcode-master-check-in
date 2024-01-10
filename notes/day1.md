# leetcode 704. Binary Search

## Thinking Reocrd:
We can have two pointer: one start from 0 and one start from the last address.
Everytime, we compare the absolute distance between the two pointer's value to the target value.
We change the further index = int((higher index + lower index)/2).
If the new pointer's value == target, return the index.
If in the end, higher == lower, return -1.

if target < smallest or target > max, return -1.

!!!WRONG!!! The array is not a continuous array!

Change a little:
We can have two pointer: one start from 0 and one start from the last address.
everytime we compare the temp_index = int((lower index + higher index)/2) with the target.
If target == temp value:
return temp_index
elif target < temp value:
higher index = temp_index
else low index = temp_index

new problem: BOUDARY CONDITION!

## Submissions:
![Alt text](../__assets__/lc_704_1.png?raw=true "lc_704_1")

## Summary from the Lecture:
video source: [代码随想录-二分法](https://www.bilibili.com/video/BV1fA4y1o715/)

### Boundary Condition:

1. [left, right]

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


