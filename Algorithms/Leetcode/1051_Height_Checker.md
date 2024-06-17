# [1051. Height Checker](https://leetcode.com/problems/replace-words/?envType=daily-question&envId=2024-06-07)

|Level|Constraints|
|------|----------|
|**Easy**|1 <= `heights.length` <= 100|
||1 <= `heights[i]` <= 100|  

# Description
A school is trying to take an annual photo of all the students. The students are asked to stand in a single file line in **non-decreasing order** by height. Let this ordering be represented by the integer array expected where `expected[i]` is the expected height of the ith student in line.

You are given an integer array heights representing the current order that the students are standing in. Each `heights[i]` is the height of the **i<sup>th</sup>** student in line (0-indexed).

Return the number of indices where `heights[i] != expected[i]`.  

# Example
```
Input: heights = [1,1,4,2,1,3]
Output: 3
Explanation: 
heights:  [1,1,4,2,1,3]
expected: [1,1,1,2,3,4]
Indices 2, 4, and 5 do not match.
```
```
Input: heights = [5,1,2,3,4]
Output: 5
Explanation:
heights:  [5,1,2,3,4]
expected: [1,2,3,4,5]
All indices do not match.
```

# Approach
We just create an copy of `heights`, then sort it and compare with `heights`.

# Code
```C++
class Solution {
public:
    int heightChecker(vector<int>& heights) {
        int rs = 0, n = heights.size();
        vector<int> tmp = heights;
        
        sort(heights.begin(), heights.end());

        for (int i = 0; i < n; i++) {
            if (tmp[i] != heights[i]) rs++;
        }

        return rs;
    }
};
```

# Complexity
- Time: O(N * M) with:
    - N is the number of words in `sentence`;  
    - M is length of the longest word in `sentence`;

- Space: O(N)