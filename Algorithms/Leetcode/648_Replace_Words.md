# [648. Replace Words](https://leetcode.com/problems/replace-words/?envType=daily-question&envId=2024-06-07)

|Level|Constraints|
|------|----------|
|**Medium**|1 <= dictionary.length <= 1000|
||1 <= `dictionary[i].length` <= 100|
||`dictionary[i]` consists of only lower-case letters.|
||1 <= `sentence.length` <= 10<sup>6</sup>|
||`sentence` consists of only lower-case letters and spaces.|
||The number of words in `sentence` is in the range `[1, 1000]`|
||The length of each word in `sentence` is in the range `[1, 1000]`|
||Every two consecutive words in `sentence` will be separated by exactly one space.|
||`sentence` does not have leading or trailing spaces.|

# Description
In English, we have a concept called root, which can be followed by some other word to form another longer word - let's call this word derivative. For example, when the root `"help"` is followed by the word `"ful"`, we can form a derivative `"helpful"`.  

Given a `dictionary` consisting of many roots and a `sentence` consisting of words separated by spaces, replace all the derivatives in the sentence with the root forming it. If a derivative can be replaced by more than one root, replace it with the root that has the shortest length.  

Return the `sentence` after the replacement.  

# Example
```
Input: dictionary = ["cat","bat","rat"], sentence = "the cattle was rattled by the battery"
Output: "the cat was rat by the bat"
```
```
Input: dictionary = ["a","b","c"], sentence = "aadsfasf absbs bbab cadsfafs"
Output: "a a b c"
```

# Approach
With this problems, we can easily handling it with Trie.  
Theory about [Trie](../Theory/Trie.md)

# Code
```C++
class TrieNode {
    public:
        TrieNode* children[26];
        bool isTerminal;
        string word;

        TrieNode() {
            for (int i=0; i<26; i++) {
                children[i] = NULL;
            }
            isTerminal = false;
            word = "";
        }
};

class Trie {
    public:
        TrieNode* root;
        Trie() {
            root = new TrieNode();
        }
        
        void build(vector<string> &words) {
            for (string word: words) {
                TrieNode* current = root;
                for (char c : word) {
                    if (current -> children[c - 'a'] == NULL) {
                        current->children[c-'a'] = new TrieNode();
                    }
                    current = current->children[c-'a'];
                }
                current->isTerminal = true;
                current->word = word;
            }
        }

        string findShortestPrefix(string word) {
            TrieNode* current = root;
            for (char c : word) {
                if (current -> isTerminal) return current->word;

                if (current->children[c-'a'] == NULL) return "";
                else current = current->children[c-'a'];
            }

            return "";
        }
};

class Solution {
public:
    string replaceWords(vector<string>& dictionary, string sentence) {
        Trie trie;
        trie.build(dictionary);

        std::vector<std::string> words;
        std::string word, tmp;
        
        std::stringstream ss(sentence);
        
        while (ss >> word)
            words.push_back(word);

        int n = words.size();

        for (int i = 0; i < n; i++) {
            tmp = trie.findShortestPrefix(words[i]);
            if (tmp != "")
                words[i] = tmp;
        }
        std::string rs = "";
        for (int i = 0; i < n; i++) {
            rs += words[i];
            if (i != n - 1) rs += " ";
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