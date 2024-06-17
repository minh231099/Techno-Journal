# [633. Sum of Square Numbers](https://leetcode.com/problems/sum-of-square-numbers/description)

|Level|Constraints|
|------|----------|
|**Medium**|0 <= `c` <= 2<sup>31</sup> - 1|

# Description
Given a non-negative integer `c`, decide whether there're two integers `a` and `b` such that a<sup>2</sup> + `b<sup>2</sup>` = `c`.

# Example
```
Input: c = 5
Output: true
Explanation: 1 * 1 + 2 * 2 = 5
```
```
Input: c = 3
Output: false
```

# Approach
In this problems, I use Two Pointer to achive.  
First I need a variable left (start from 0) and right (start from $\sqrt{c}$).  
We will check from left to right to know we can have a sum of square numbers from them or not (`tmp = left*left + right*right`). We have 3 cases:    
- If `tmp` is equal to `c`, we just need return `true`.  
- If `tmp` > c, we need decrease `right` by one.  
- If `tmp` < c, we need increase `left` by one.  

# Code
```C++
class Solution {
public:
    bool judgeSquareSum(int c) {
        long left = 0, right = sqrt(c), tmp;

        while (left <= right) {
            tmp = left*left + right*right;
            if (tmp == c) return true;
            else if (tmp > c) right--;
            else left++;
        }

        return false;
    }
};
```

# Complexity
- Time: O($\sqrt{c}$)
- Space: O(1)