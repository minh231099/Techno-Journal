|Level|Constraints|
|------|----------|
|**Easy**|1 <= `s.length` <= 2000|
||`s` consists of lowercase and/or uppercase English letters only.|

# Description
Given a string `s` which consists of lowercase or uppercase letters, return the length of the longest **palindrome** that can be built with those letters.  

Letters are case sensitive, for example, `"Aa"` is not considered a palindrome.

# Example
```
Input: s = "abccccdd"
Output: 7
Explanation: One longest palindrome that can be built is "dccaccd", whose length is 7.
```
```
Input: s = "a"
Output: 1
Explanation: The longest palindrome that can be built is "a", whose length is 1.
```

# Approach
This is a basic counting problem.  
We can easily see that If the length of the string is even, then the number of occurrences of each character is even. If the length is odd, then only one character will appear an odd number of times.  
Therefore, we just need to traverse through the characters of the string and count each character. If the count of a character is greater than 1, we will take the integer part of the division `number of occurrences / 2`. Instead, we can observe that during the counting process, if the number of occurrences of a character reaches 2, we can directly add 2 to the length of the resulting string and set the count of that character back to 0.  
And if, in the end, the length of the resulting string is still less than the length of s, we can add 1 to make the length of the string odd, with one character having an odd number of occurrences.

## Explain with example
```
Input: s = "abccccdd"
We will have the number of occurrences of each character as follows:
a: 1
b: 1
c: 4
d: 2
And we see that the characters c and d have an even number of occurrences. Therefore, we have 4 + 2 = 6 as the temporary length of the resulting string.
At that point, the resulting string will have a form like ccddcc or something similar.  
And the length of the resulting string is still less than the length of s, so we can insert one character, a or b, in the middle of it, the resulting string will become ccdaddcc or ccdbddcc. And the length become 7, and it is the longest length that we can build with s.
```
# Code
```C++
class Solution {
public:
    int longestPalindrome(string s) {
        unordered_map<char, int> mp;
        int n = s.length();
        int rs = 0;
        for (char c : s) {
            if (mp.find(c) == mp.end()) {
                mp[c] = 0;
            }
            mp[c]++;

            if (mp.at(c) == 2) {
                rs+=2;
                mp.erase(c);
            }
        }

        if (rs < n) rs++;
        return rs;
    }
};
```

# Complexity
- Time: O(N)
- Space: O(N)