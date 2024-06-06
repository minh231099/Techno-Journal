# [846. Hand of Straights](https://leetcode.com/problems/hand-of-straights/description/)
Same with: [1296. Divide Array in Sets of K Consecutive Numbers](https://leetcode.com/problems/divide-array-in-sets-of-k-consecutive-numbers/description/)  

|Level|Constraints|
|------|----------|
|**Medium**|1 <= `hand.length` <= 10<sup>4</sup>|
||0 <= `hand[i]` <= 10<sup>9</sup>|
||1 <= `groupSize` <= `hand.length`|

# Description
Alice has some number of cards and she wants to rearrange the cards into groups so that each group is of size `groupSize`, and consists of `groupSize` consecutive cards.  

Given an integer array `hand` where `hand[i]` is the value written on the i<sup>th</sup> card and an integer `groupSize`, return `true` if she can rearrange the cards, or `false` otherwise.  

# Example
```
Input: hand = [1,2,3,6,2,3,4,7,8], groupSize = 3
Output: true
Explanation: Alice's hand can be rearranged as [1,2,3],[2,3,4],[6,7,8]
```
```
Input: hand = [1,2,3,4,5], groupSize = 4
Output: false
Explanation: Alice's hand can not be rearranged into groups of 4.
```

# Approach
For this problem, I have two ideas to handle it. For both approaches, we need to sort the cards in hand in ascending order first.  

## The first approach
We can recount the occurrences of each card and then check with the first card of each group. We need to determine how many more cards are needed to form a complete group. This can be achieved by calculating the difference between the count of the current card and the count of the next card in the sequence, up to the group size (`The number of occurrences[card + i] - The number of occurrences[card]` with `i` in range [1, groupSize))  

## The second approach
Besides the above approach, we can utilize a queue to tackle this issue. We'll store pairs consisting of the card value and its position when it's drawn. We'll still iterate through each card in hand and check if the first card in the queue can follow the currently examined card:  
- If not: We push the current card into the queue, signifying it as the first card of the next group (since the cards are sorted in ascending order, we are ensured of this).
- If yes: It means the current card is within this queue, so we need to check two possibilities:
    - First, if the head of the queue is the second card from the bottom when counted upwards, then we understand that the card being examined is the last card, and we have a complete group.
    - If not, we push the current card into the queue with the appropriate position in the group, which is the position of the head of the queue plus one.

We continue this process until we reach the last card in hand. If `the number of groups formed * groupSize` equals the initial length of the cards in hand, we return `true`; otherwise, we return `false`.



# Code
## The first approach
```C++
class Solution {
public:
    bool isNStraightHand(vector<int>& hand, int groupSize) {
        ios::sync_with_stdio(false); cin.tie(nullptr); cout.tie(nullptr);
        int n = hand.size();
        if (n % groupSize) return false;

        unordered_map<int, int> cntArr;
        sort(hand.begin(), hand.end());

        for (int i = 0; i < n; i++) {
            if (cntArr.find(hand[i]) == cntArr.end()) cntArr[hand[i]] = 0;
            cntArr[hand[i]]++;
        }

        for (int i = 0; i < n; i++) {
            int card = hand[i];
            if (cntArr[card] > 0) {
                for (int j = 1; j < groupSize; j++) {
                    if (cntArr[card] > cntArr[card + j]) return false;
                    cntArr[card + j] -= cntArr[card];
                }
                cntArr[card] = 0;
            }
        }

        return true;
    }
};
```
## The first approach
```C++
class Solution {
public:
    bool isNStraightHand(vector<int>& hand, int groupSize) {
        int n = hand.size();
        if (n % groupSize) return false;
        if (groupSize == 1) return true; 
        // we need this because if groupSize == 1 will make curCard.second == groupSize - 1 always is False or we can make it become curCard.second == groupSize - 1 || groupSize == 1, but it's not necessary

        int rs = 0;
        queue<pair<int, int>> cardQ;
        sort(hand.begin(), hand.end());
        pair<int, int> curCard;
        for (int i = 0; i < n; i++) {
            int card = hand[i];
            if (cardQ.empty() || cardQ.front().first + 1 != hand[i]) {
                cardQ.push({card, 1});
                continue;
            }

            curCard = cardQ.front();
            cardQ.pop();

            if (curCard.second == groupSize - 1) rs++;
            else cardQ.push({card, curCard.second + 1});
        }

        if (rs * groupSize == n) return true;

        return false;
    }
};
```

# Complexity
## The first approach
- Time: O(Nlog(N))
- Space: O(N)

## The second approach
- Time: O(Nlog(N))
- Space: O(N)