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
solution having a time complexity of `O(n)`.

The intuitive approach will be to **iterate** on each element and do a forward
lookup to check the value of the next element. **If** both are equal we can
remove them along with the elements behind them with the same value.
Each time we remove an element we will have to link the *previous element* in
the list to the successor of the current one.

As we can just re-order the links of the list we are not technically in need to
allocate any extra space for that.

Beware about loosing your head because the first element might be a duplicate.
To leverage that we can create an "sentinel" which is basically a node poiting
points to the first element.
The space complexity will then be `O(1)`.

## Algorithm

1. Store the current node's value
2. If the current element is not equal to the next one then skip the
current element
3. Otherwise: While the current element's value is equal to the stored value
we link the previous element to the next one in the list
4. Loop until the next element is null

Note: `V` points to the current element
```
Step 1: find a duplicate
		      V
sentinel -> 1 -> 2 -> 3 -> 3 -> 4
found duplicate=3

Step2: remove all nodes with the duplicate value
		     V
sentinel -> 1 -> 2   3 -> 3 -> 4
		 |________^

		      V
sentinel -> 1 -> 2 -> 3 -> 4

		      V
sentinel -> 1 -> 2 -> 3 -> 4
		 |_________^

		      V
sentinel -> 1 -> 2 -> 4
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
