# Removing duplicates from sorted list

## Problem

Given a sorted linked list, delete all nodes that have duplicate numbers, leaving only distinct numbers from the original list.

Return the linked list sorted as well.

Example 1:

```
Input: 1->2->3->3->4->4->5
Output: 1->2->5
```

## Intuition

As the problem states that the list is sorted we can easily iterate over each
element and remove any element which have a value equal to it's successor.
To do so we have no other choice but iterating on the list which makes the
solution having a time complexity of O(n).
As we can just re-order the links of the list we are not technically in need to
allocate any extra space for that.

The intuitive approach will be to **iterate** on each element and **if** the
current element and it's successor have the same value **then** we store it's
value and **loop** to remove the current element **until** it's value is different
than the duplicate value found previously.
Each time we remove an element we will have to link the *previous element* in
the list to the successor of the current one.

Beware about loosing your head because the first element might be a duplicate.
To leverage that we can create an "sentinel" which is basically a node which
points to the first element.

## Algorithm

1. Loop until the next element is null
  a. Store the current node's value
  b. If the current element is not equal to the next one then skip the
  current element
  c. Otherwise: While the current element's value is equal to the stored value
  we link the previous element to the next one in the list

```
sentinel -> 1 -> 2 -> 3 -> 3 -> 4

sentinel -> 1 -> 2   3 -> 3 -> 4
		 |________^
```

Code:

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        auto s = ListNode(-1, head);
        auto prev = &s;
        auto tmp = head;
        while (tmp != nullptr and tmp->next != nullptr) {
            auto dup = tmp->val;
            if (not (tmp->next->val == tmp->val)) {
                prev = tmp;
                tmp = tmp->next;
                continue;
            }
            while (tmp != nullptr and tmp->val == dup) {
                    tmp = tmp->next;
                    prev->next = tmp;
            }
        }
        return s.next;
    }
};
```
