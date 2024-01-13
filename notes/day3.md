# leetcode 203. Remove Linked List Elements

## Thinking Reocrd:
Idea:
The way to delete a node from a linked list is to change the pointer of the former to the next.\
So we can inquire the Listnode.val from the beginning and change the former Listnode.next to this Listnode.next.\

Traversing the List:\
To go through the list, you start at the head and keep moving to the next node until you reach a node that points to None.

```python
current = head
while current:
    print(current.val)
    current = current.next
```

Inquiring a Specific Node:\
To find a specific node (say the 2nd node), you can traverse the list and stop at the desired position:

```python
position = 2  # Example position
current = head
for _ in range(1, position):
    if current.next is not None:
        current = current.next
    else:
        break  # Position is out of the list's range
# 'current' now refers to the node at the specified position
```

## Submissions:
FAILED!

code:
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeElements(self, head: Optional[ListNode], val: int) -> Optional[ListNode]:
        current = head
        
        if current is not None:
            nextone = current.next
            while nextone:
                if nextone.val == val:
                    current.next = nextone.next
                if current.next is None:
                    break
                else:
                    current = current.next
        
            if head.val == val:
                head = head.next
        
        return head
```

I am confused about what am I opearete about. What should I return as the result Linked List.

## Summary from the Lecture:
video source: [代码随想录-移除链表元素](https://www.bilibili.com/video/BV18B4y1s7R9)

Method 1:\
Do different operations for the head and the other nodes.\
```python
class Solution:
    def removeElements(self, head: Optional[ListNode], val: int) -> Optional[ListNode]:
        
        # leetcode-master method 1 delete element in original Linked List

        while (head is not None) and (head.val == val):
            head = head.next

        current = head
        while (current is not None) and (current.next is not None):
            if current.next.val == val:
                current.next = current.next.next
            else:
                current = current.next
        
        return head
```

Improvement:\
We can ignore the head first, and check the head after the run.

Method 2:\
Use a dummy node to avoid the special case of the head.\
```python
class Solution:
    def removeElements(self, head: Optional[ListNode], val: int) -> Optional[ListNode]:
        
        # leetcode-master method 2 virtual new head

        dummy_head = ListNode()
        dummy_head.next = head

        # Keep in mind here, we make the current equals to dummy_head, not dummy_head.next

        current = dummy_head
        while current.next:
            if current.next.val == val:
                current.next = current.next.next
            else:
                current = current.next
        
        return dummy_head.next
```

## Complexity Analysis:
- Time complexity: O(n), it's one pass solution.
- Space complexity: O(1), it's a constant space solution.

# leetcode 707. Design Linked List

## Thinking Reocrd:
No idea, failed at the first step, I don't know how to initiate a Linked List as required.

## Summary from the Lecture:
video source: [代码随想录-设计链表](https://www.bilibili.com/video/BV1FU4y1X7WD)

We need to design a Linked List class. Which means the ListNode class is already given...\
I didn't notice that in my first try.

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next
```

To initiate a Linked List, we can use a dummy head node.

```python
class MyLinkedList:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.dummy_head = ListNode()
        self.size = 0
```

Then everything is straight forward.\
My former confusion was I mixed the ListNode initiation and Linked List initiation. 

```python
    def get(self, index: int) -> int:
        """
        Get the value of the index-th node in the linked list. If the index is invalid, return -1.
        """
        if index < 0 or index >= self.size:
            return -1
        
        current = self.dummy_head
        for _ in range(index + 1):
            current = current.next
        
        return current.val
```

### Retry Submissions:
![Alt text](../__assets__/pic/lc_707_1.png?raw=true "lc_707_1")

### TBD:
Approach 2: Doubly Linked List

# leetcode 206. Reverse Linked List

## Thinking Reocrd:
Idea:
While loop through the linked list to get the value.\
Use a new linked list with a virtual head to add the value into the begin index.

## Submissions:
![Alt text](../__assets__/pic/lc_206_1.png?raw=true "lc_206_1")

## Summary from the Lecture:
video source: [代码随想录-反转链表](https://www.bilibili.com/video/BV1nB4y1i7eL)

## Complexity Analysis:
- Time complexity: O(n), it's one pass solution.
- Space complexity: O(1), it's a constant space solution.