# [974. Subarray Sums Divisible by K](https://leetcode.com/problems/subarray-sums-divisible-by-k/description/)

|Level|Constraints|
|------|----------|
|**Easy**|1 <= nums.length <= 3 * 10<sup>4</sup>|
||-10<sup>4</sup> <= nums[i] <= 10<sup>4</sup>|
||2 <= k <= 10<sup>4</sup>|

# Description
Given an integer array nums and an integer `k`, return the number of non-empty subarrays that have a sum divisible by `k`.  

A subarray is a contiguous part of an array.

# Example
```
Input: nums = [4,5,0,-2,-3,1], k = 5
Output: 7
Explanation: There are 7 subarrays with a sum divisible by k = 5:
[4, 5, 0, -2, -3, 1], [5], [5, 0], [5, 0, -2, -3], [0], [0, -2, -3], [-2, -3]
```
```
Input: nums = [5], k = 9
Output: 0
```

# Approach
- We need to find all the subarray has sum divisible by `k`. And I still use the same algorithms with problem [523 Continuous Subarray Sum](./523_Continuous_Subarray_Sum.md). But this array will have sum can be negative so we can handle it like: `(sum % k + k) % k`.  

# Code
```C++
class Solution {
public:
    int subarraysDivByK(vector<int>& nums, int k) {
        ios_base::sync_with_stdio(false);
        cin.tie(nullptr);
        cout.tie(nullptr);

        int sum = 0, n = nums.size(), tmp = 0, rs = 0, mp[k];
        memset(mp, 0, sizeof(mp));
        mp[0] = 1;

        for (auto& num : nums) {
            sum += num;
            tmp = (sum % k + k) % k;
            
            if (!mp[tmp]) mp[tmp] = 1;
            else {
                rs += mp[tmp];
                mp[tmp]++;
            }
        }

        return rs;
    }
};
```

# Complexity
- Time: O(N)
- Space: O(N)