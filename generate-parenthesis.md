# Generate Parenthesis

## Problem

Given n pairs of parentheses, write a function to generate all combinations of
well-formed parentheses.

For example, given n = 3, a solution set is:
```
[
  "((()))",
  "(()())",
  "(())()",
  "()(())",
  "()()()"
]
```

## Intuition

Generating conbinations rhymes with tree creation. As parenthesis are going by
2 we can think of the generation of each solution as the creation of a binary
tree on which each right children is adding a right parenthsis and each left a
left parenthesis.

Then all nodes at the height `n*2` in the tree are potential solutions.

In order to have each leaf node representing a valid solution we need to
ensure, at any time during the tree creation that: `the number of left
parenthesis written >= the number of right parenthesis`

## Algorithm

Instead of creating a tree of each possibility we can use recursion to
simulate it.

```cpp
class Solution {
public:
    vector<string> res;
    void helper(string s, int o, int c) {
        if (o > 0)
            helper(s + "(", o-1, c);
        if (c > 0 and c > o)
            helper(s + ")", o, c-1);
        if (o == 0 and c == 0)
            res.push_back(s);
    }
    vector<string> generateParenthesis(int n) {
        helper("", n, n);
        cout << res.size();
        return res;
    }
};
```
