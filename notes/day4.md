# leetcode 24

## Thinking Reocrd:
Idea:\
first, we can add a dummy_head, to keep the swap rule consistent in the whole run.\
We assume the index of the node start from 1.\
If (index % 2) == 1, change its next to index+3.\
If (index % 2) == 0, change its next to index-1.

Boundary:\
If the total index N is an odd number. Then N-2 change its next to N.\
If the total index N is an even number. Then N-1 change its next to none.

That makes it complex.
We only need to deal with in two pairs:\
prev->i->j->follow\
temp = j\
change i.next to i.next.next (in the new i.next, we delete j)\
change temp.next to i (we save node j in temp and change its next pointer)\
change the prev.next to temp.

## Submissions:
![Alt text](../__assets__/pic/lc_24_1.png?raw=true "lc_24_1")

## Summary from the Lecture:
video source: [代码随想录-两两交换链表中的节点](https://www.bilibili.com/video/BV1YT411g7br)

## Complexity Analysis:
- Time complexity: O(N) where N is the number of nodes in the linked list.
- Space complexity: O(1) since it's a constant space solution.

# leetcode 19. Remove Nth Node From End of List

## Thinking Reocrd:
Idea:\
First step: lop through the whole linked list and count the nuber of nodes=M.\
Second step: Find the (M-n) node, and change it pointer to next's pointer.

## Submissions:
![Alt text](../__assets__/pic/lc_19_1.png?raw=true "lc_19_1")

## Summary from the Lecture:
video source: [代码随想录-删除链表的倒数第N个节点](https://www.bilibili.com/video/BV1vW4y1U7Gf)

a smart way to solve this problem is to use two pointers.\
The first pointer is the dummy head.\
The second pointer is the dummy head + n.\
Then we can move both pointers at the same time.\
When the second pointer reaches the end, the first pointer is the node we want to delete.

```python
class Solution:
    def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
        dummy_head = ListNode(-1, head)

        quick = head
        slow = dummy_head

        for _ in range(n):
            quick = quick.next
        
        while quick:
            # we use quick to find the None, use the slow to find the node before last nth.
            quick = quick.next
            slow = slow.next
        
        slow.next = slow.next.next

        return dummy_head.next
```

## Complexity Analysis:
### Approach 1: Two pass algorithm
- Time complexity: O(L), the algorithm makes two traversal of the list, first to calculate list length L and second to find the (L - n)th node. There are 2L-n operations and time complexity is O(L).
- Space complexity: O(1), we only used constant extra space.

### Approach 2: One pass algorithm
- Time complexity: O(L), the algorithm makes one traversal of the list of L nodes. Therefore time complexity is O(L).
- Space complexity: O(1), we only used constant extra space.

# leetcode 160. Intersection of Two Linked Lists

## Thinking Reocrd:
Idea:\
intersect node means the nodes before it are pointed to the same node.\
Key question is how to know we are on the same node, after going through the node list?

Can i go through the ListA and ListB, and save all the address for each next pointer? Then, search for the first same address pointer?

## Submissions:
![Alt text](../__assets__/pic/lc_160_1.png?raw=true "lc_160_1")

## Summary from the Lecture:
lecture note source: [代码随想录-链表相交](https://programmercarl.com/%E9%9D%A2%E8%AF%95%E9%A2%9802.07.%E9%93%BE%E8%A1%A8%E7%9B%B8%E4%BA%A4.html)

### Approach 2: Two Pointers
If you assume the length of list A is a + c, and the length of list B is b + c, then the pointer of list A will move a + c + b steps, and pointer B will move b + c + a steps. Since a + c + b = b + c + a, it does not matter what value c is. Pointer A and pointer B will meet at the intersection node in the second iteration.\
Or in another perspect, if we combine list A and list B, one starts from head A, the other starts from head B. Then both of the combined list will have the same length. Which means they must meet at the intersection node.

```python
class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        if not headA or not headB:
            return None
        
        pA = headA
        pB = headB

        while pA != pB:
            if pA:
                pA = pA.next
            else:
                pA = headB
            
            if pB:
                pB = pB.next
            else:
                pB = headA
        
        return pA # or pB
```

## Complexity Analysis:
### Approach 1: Hash Set
- Time complexity: O(m+n), where m and n are the lengths of the two linked lists.
- Space complexity: O(m) or O(n), where m and n are the lengths of the two linked lists. That is because we need to store all the nodes of the first list in a set.

### Approach 2: Two Pointers
- Time complexity: O(m+n), where m and n are the lengths of the two linked lists. The first iteration is to connect the end of list A to the beginning of list B, which costs O(m) time; the second iteration is to find the intersection node, which costs O(n) time. Overall, the time complexity is O(m+n).
- Space complexity: O(1), we only used constant extra space.


# leetcode 142. Linked List Cycle II

## Thinking Reocrd:
Idea:

Approach1 : using a hash set

Approach2: find a O(1) memory way
use a head B to copy the whole List Link. Every time we move to the next node, we delete the former connection.\
We will finally find the pos when its next is none.\
But that will modify the Linked List!

We can change the number how many step the fast is fast than the slow. We add the step length every time, it will sometimes equal to the loop length.\
But how to find the beginning?

After we find out the length for the loop. We can try the length of none loop. We let the fast is loop length faster than the slow, to find when will they find the same node.\
But I didn't find a way to achieve that...

FAILED AT TWO POINTER solution

## Submissions:
![Alt text](../__assets__/pic/lc_142_1.png?raw=true "lc_142_1")

## Summary from the Lecture:
video source: [代码随想录-环形链表II](https://www.bilibili.com/video/BV1if4y1d7ob)

### Approach 2: Floyd's Tortoise and Hare Algorithm
Algorithm\
Let the rabbit and the tortoise move at their own pace. When the rabbit moves twice as fast as the tortoise, the rabbit will meet the tortoise at some point inside the loop.\
Assume the circle distance is c, the distance between the start node of the list and the start node of the loop is a, and the distance between the start node of the loop and the meeting point is b. The distance of the slow pointer traveled when they meet is a + b, and the distance of the fast pointer traveled is a + b + k*c + b, which is twice as much.\
a + b + k*c + b = 2*(a + b)\
k*c = a + b\
a = k*c - b\
Let two tortoises start from the start node of the list and the meeting point, respectively. The distance they need to travel to meet each other is a, which is the same as the distance the slow pointer traveled. Therefore, they will meet at the start node of the loop.

```python
class Solution:
    def detectCycle(self, head: ListNode) -> ListNode:
        if not head or not head.next:
            return None
        
        slow = head
        fast = head

        # the while will check if there is a loop
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next

            if slow == fast:
                break
        
        if slow != fast:
            return None
        
        slow = head

        while slow != fast:
            slow = slow.next
            fast = fast.next
        
        return slow
```

## Complexity Analysis:
### Approach 1: Hash Set
- Time complexity: O(n), where n is the number of nodes in the linked list.
- Space complexity: O(n), where n is the number of nodes in the linked list.
### Approach 2: Floyd's Tortoise and Hare Algorithm
- Time complexity: O(n), where n is the number of nodes in the linked list.
- Space complexity: O(1), we only used constant extra space.

