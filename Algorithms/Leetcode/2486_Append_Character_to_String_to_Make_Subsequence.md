# [2486. Append Characters to String to Make Subsequence](https://leetcode.com/problems/append-characters-to-string-to-make-subsequence/description/)

|Level|Constraints|
|------|----------|
|**Medium**|1 <= `s.length`, `t.length` <= 10<sup>5</sup>|
||`s` and `t` consist only of lowercase English letters.|

# Description
You are given two strings `s` and `t` consisting of only lowercase English letters.

Return the minimum number of characters that need to be appended to the end of `s` so that t becomes a subsequence of `s`.

A subsequence is a string that can be derived from another string by deleting some or no characters without changing the order of the remaining characters.

# Example
```
Input: s = "coaching", t = "coding"
Output: 4
Explanation: Append the characters "ding" to the end of s so that s = "coachingding".
Now, t is a subsequence of s ("coachingding").
It can be shown that appending any 3 characters to the end of s will never make t a subsequence.
```
```
Input: s = "abcde", t = "a"
Output: 0
Explanation: t is already a subsequence of s ("abcde").
```
```
Input: s = "z", t = "abcde"
Output: 5
Explanation: Append the characters "abcde" to the end of s so that s = "zabcde".
Now, t is a subsequence of s ("zabcde").
It can be shown that appending any 4 characters to the end of s will never make t a subsequence.
```

# Approach
With this problem, I will use Two Pointer to solve it.  
We will have `n` is length of `s` and `m` is length of `t`.  
We can easily see that as we traverse linearly thorugh string `s`, if we encounter any character of string `t` in the order they appear in `s`, we can decrement the count of characters needs to be added by one.  
In other words:  
- Use two pointer, one is `i` for `s` and `j` for `t`. Additional, I will create a variable name `rs` to store the last result and and it will be equal to the length of `t` at initialization;  
- Traverse through string `s`, if `s[i] == t[j]`, we will increase `j` by one and `rs` will minus one;
- After we finish traversing, we can return `rs` or we can observe that `rs` decreases each time `j` increases by one unit, so we just need return `m - j` which `m` is length of `t` like I said before.

## Explain with example
```
Input: s = "coaching", t = "coding"
i = 0 and j = 0
'c' matches 'c': i = 1, j = 1
'o' matches 'o': i = 2, j = 2
'a' does not match 'd': i = 3, j = 2
'c' does not match 'd': i = 4, j = 2
and so on with i = 7, j = 2 and 'g' does not match 'd' 
```
- So remaining characters in `t` is "ding" and the length is 4.

# Code
```C++
class Solution {
public:
    int appendCharacters(string s, string t) {
        int n = s.length(), m = t.length();
        int j = 0;
        for (int i = 0; i < n && j < m; i++)
            if (s[i] == t[j]) j++;
        
        return m - j;
    }
};
```

# Complexity
- Time: O(N)  
- Space: O(1)