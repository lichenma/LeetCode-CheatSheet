## 24-Swap Nodes in Pairs 

Given a linked list, swap every two adjacent nodes and return its head. 

You may **not** modify the values in the list's nodes, only nodes itself may be changed 

```
Example: 

Input:  1 -> 2 -> 3 -> 4 
Output: 2 -> 1 -> 4 -> 3 
```

<br><br>
## Recursion 

```java 
public class Solution {
    public ListNode swapPairs(ListNode head) {
       
        if ((head==null)||(head.next==null))
            return head; 
        
        ListNode n = head.next; 
        head.next = swapPairs(head.next.next);
        n.next = head; 

        return n; 
    }
}
```


