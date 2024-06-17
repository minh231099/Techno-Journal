# [1122 Relative Sort Array](https://leetcode.com/problems/relative-sort-array/description/)

|Level|Constraints|
|------|----------|
|**Easy**|1 <= `arr1.length, arr2.length` <= 1000|
||0 <= `arr1[i], arr2[i]` <= 1000|
||All the elements of `arr2` are distinct.|
||Each arr2[i] is in arr1.|

# Description
Given two arrays arr1 and arr2, the elements of arr2 are distinct, and all elements in arr2 are also in arr1.  

Sort the elements of arr1 such that the relative ordering of items in arr1 are the same as in arr2. Elements that do not appear in arr2 should be placed at the end of arr1 in ascending order.  
# Example
```
Input: arr1 = [2,3,1,3,2,4,6,7,9,2,19], arr2 = [2,1,4,3,9,6]
Output: [2,2,2,1,4,3,3,9,6,7,19]
```
```
Input: arr1 = [28,6,22,8,44,17], arr2 = [22,28,8,6]
Output: [22,28,8,6,17,44]
```

# Approach
First, we will store all the times each element in `arr1` appear. The we will traverse throught `arr2` and add to `result` value of `arr2[i]` with times is the times that element appear in `arr1`. And in the end, we will traverse throught `arr1` again, to push the remain element not appear in `arr2`. And remember, sort `arr1` first.

# Code
```C++
class Solution {
public:
    vector<int> relativeSortArray(vector<int>& arr1, vector<int>& arr2) {
        int n = arr1.size(), m = arr2.size();
        unordered_map<int, int> mp;
        vector<int> ans;
        sort(arr1.begin(), arr1.end());
        
        for (auto& num : arr1) {
            if (mp.find(num) == mp.end()) mp[num] = 0;
            mp[num]++;
        }

        for (auto& num: arr2) {
            while (mp[num] > 0) {
                ans.push_back(num);
                mp[num]--;
            }
        }

        for (auto & num: arr1) {
            while(mp[num] > 0) {
                ans.push_back(num);
                mp[num]--;
            }
        }

        return ans;
    }
};
```

# Complexity
- Time: O(N*M)
- Space: O(N)