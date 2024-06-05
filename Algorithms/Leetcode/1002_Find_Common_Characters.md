|Level|Constraints|
|------|----------|
|**Easy**|1 <= `words.length` <= 100|
||1 <= `words[i].length` <= 100|
||`words[i]` consists of lowercase English letters|

# Description
Given a string array `words`, return *an array of all characters that show up in all strings within the* `words` *(including duplicates)*. You may return the answer in any order.

# Example
```
Input: words = ["bella","label","roller"]
Output: ["e","l","l"]
```
```
Input: words = ["cool","lock","cook"]
Output: ["c","o"]
```

# Approach
## Goal
- We need to search for the repeated characters in all the words.
- The characters may appear multiple times with the number of repetitions varying across the words.

## The characters that may appear
- We have the condition that `words[i]` consists of lowercase English letters. Therefore, only the characters from 'a' to 'z' will appear.

## Methodology
- We will iterate through each element in the `words` array, and record the occurrences of each character in that word.
- And we need an array to store the final occurrences of the characters in all words, here I call it rs with a length of 26, and the initial values of the elements in the array are 100 or INT_MAX depending on personal preference (as long as it is greater than or equal to the maximum number of occurrences a word can have according to the condition `1 <= words[i].length <= 100`).
- And as we iterate through each word in "words", we can compare the occurrences of the characters in the temporary counting array for that word, which I call tmp, with the final rs array. We only need to take the minimum of the corresponding elements in the array (`rs[i] = min(rs[i], tmp[i])`).

# Code
```C++
class Solution {
public:
    vector<string> commonChars(vector<string>& words) {
        vector<int> cnt(26, 100);
        vector<string> rs;
        int n = words.size();

        for (int i = 0; i < n; i++) {
            vector<int> tmp(26, 0);
            for (char c : words[i])
                tmp[c - 'a'] += 1;

            for (int j = 0; j < 26; j++)
                cnt[j] = min(cnt[j], tmp[j]);
        }

        for (int i = 0; i < 26; i++)
            for (int j = 0; j < cnt[i]; j++)
                rs.push_back(string(1, 'a' + i));

        return rs;
    }
};
```

# Complexity
- Time: We can see that the first loop for `(int i = 0; i < n; i++)` with n being the length of `words` (at most 100), so the complexity of this loop is 100. With for `(char c : words[i])`, the complexity is also 100. Therefore, the time complexity will be O(100 * 100) = O(10000) or **O(N*M)** with N being the length of words and M being the length of the longest word.
- Space: We have the length of `cnt` as 26, and `tmp` as 26 for each word in `words`, and at most `words` has 100 words, so tmp will need a maximum length of 26 * 100, and with `rs`, we need a maximum length of 100 because a word in `words` has a maximum of 100 characters (the worst case being 100 words with 100 identical characters each). Therefore, the space complexity will be O(26 + 26 * 100 + 100) = **O(2726)**.