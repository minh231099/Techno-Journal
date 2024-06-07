# Trie Data Structure

The Trie data structure is a tree-like data structure used for storing a dynamic set of strings. It is commonly used for efficient `retrieval` and `storage` of keys in a large dataset.

With any data structure, we always need to insertion, search and deletion. We will explore those methods together, but first, we need to know how to initialize a Trie class and its constructor.

## Create A Trie
```C++
struct TrieNode {
    TrieNode* childNode[26]; // We have pointer for any childNode (characters in English alphabet)
    bool wordEnd; // determine whether it's a complete word or not

    TrieNode() // contructor
    {
        wordEnd = false;
        for (int i = 0; i < 26; i++) {
            childNode[i] = NULL;
        }
    }
};
```

## Insertion in Trie Data Structure

![Insertion in Trie](../../util/images/trie_insert.png)  

```C++
void insert_key(TrieNode* root, string& key)
{
    TrieNode* currentNode = root; // Start with root of Trie

    for (auto c : key) {
        if (currentNode->childNode[c - 'a'] == NULL) { // Check if the node exist for the current character in Trie 
            TrieNode* newNode = new TrieNode();
            currentNode->childNode[c - 'a'] = newNode;
        }
        currentNode = currentNode->childNode[c - 'a']; // Move to the current key node to add the next key.
    }
    currentNode->wordEnd = 1; // Make the last key node is the new word
}
```

- Time complexity: O(number of words * maxLengthOfWord)
- Space complexity: O(number of words * maxLengthOfWord)

# Searching in Trie Data Structure:
![Search in Trie](../../util/images/trie_search.png)  

```C++
bool search_key(TrieNode* root, string& key)
{
    TrieNode* currentNode = root; // Start with root of Trie

    for (auto c : key) {
        // Check if the node exist for the current character in the Trie.
        if (currentNode->childNode[c - 'a'] == NULL) { 
            return false;
        }
        currentNode = currentNode->childNode[c - 'a'];
    }

    return (currentNode->wordEnd == true);
}
```

- Time complexity: O(number of words * maxLengthOfWord)
- Space complexity: O(number of words * maxLengthOfWord)

# Remove from Trie:

```C++
const int ALPHABET_SIZE = 26;

bool isEmpty(TrieNode* root)
{
    for (int i = 0; i < ALPHABET_SIZE; i++)
        if (root->children[i])
            return false;
    return true;
}

TrieNode* remove(TrieNode* root, string key, int depth = 0)
{
    // if trie is empty
    if (!root)
        return NULL;

    // if current key length is equal to current depth 
    // (its mean last word in key is being precessed)
    if (depth == key.size()) {
        // if this Node is end of a word, then make it become not
        if (root->isEndOfWord)
            root->isEndOfWord = false;

        // if current node is not prefix of any other word, remove it from memory
        if (isEmpty(root)) {
            delete (root);
            root = NULL;
        }
 
        return root;
    }

    // start recursion to the last character
    int index = key[depth] - 'a';
    root->children[index] = remove(root->children[index], key, depth + 1);
 
    // if current node is not prefix of any other word, remove it from memory
    if (isEmpty(root) && root->isEndOfWord == false) {
        delete (root);
        root = NULL;
    }
 
    return root;
}
```
- Time Complexity: O(n) where n is the key length.
- Auxiliary Space: O(n*m), where n is the key length of the longest word and m is the total number of words.