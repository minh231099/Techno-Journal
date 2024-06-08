# [2091. Remove Minium and Maxium From Array](https://leetcode.com/problems/removing-minimum-and-maximum-from-array/description/)

|Level|Constraints|
|------|----------|
|**Medium**|1 <= nums.length <= 10<sup>5</sup>|
||0 <= nums[i] <= 10<sup>9</sup>|
||0 <= sum(nums[i]) <= 2<sup>31</sup> - 1|
||1 <= k <= 2<sup>31</sup> - 1|

# Description
Given an integer array nums and an integer `k`, return `true` if `nums` has a good subarray or `false` otherwise.

A **good subarray** is a subarray where:

its length is **at least two**, and
the sum of the elements of the subarray is a multiple of `k`.
Note that:

- A subarray is a contiguous part of the array.
- An integer `x` is a multiple of `k` if there exists an integer n such that `x = n * k`. `0` is always a multiple of `k`.

# Example
```
Input: nums = [23,2,4,6,7], k = 6
Output: true
Explanation: [2, 4] is a continuous subarray of size 2 whose elements sum up to 6.
```
```
Input: nums = [23,2,6,4,7], k = 6
Output: true
Explanation: [23, 2, 6, 4, 7] is an continuous subarray of size 5 whose elements sum up to 42.
42 is a multiple of 6 because 42 = 7 * 6 and 7 is an integer.
```
```
Input: nums = [23,2,6,4,7], k = 13
Output: false
```

# Approach
The goal is to determine if there exists a subarray of at least size 2 such that the sum of it is a multiple of a `k`.
## First Approach
This is the first thing I think when see problems like this: **Sliding Window**.  
We will traverse through `nums` and calculate the sum up to current element. Then we uses a nested loop to consider various subarrays ending at the current element by adjusting the sum and checking if this sum is a multiple of `k`. 

## Second Approach
Key concepts is: Prefix Sum with Hash map.  
A prefix sum is the cumulative sum of elements of an array up to a certain index. For an array nums, the prefix sum up to index `i` is `sum(i) = nums[0] + nums[1] + ... + nums[i]`.   
And we have a theorem in modular arithmetic like:
> When you take the modulo `k` of a sum, its mean you get the remainder when that sum is divided by `k`. This is usefull because if two prefix sums have the same remainder when divided by `k`, the elements between these two sums form a subarray whose sum is a multiple of `k`.

So we will use Hashmap to store last index of the sum and its remainder. And find any other sum has the same remainder.  


# Code
```C++
class Solution {
public:
    bool checkSubarraySum(vector<int>& nums, int k) {
        int n = nums.size();
        if (n == 1) return false;
        if (k == 0 || k == 1) return true;

        int currentSum = nums[0], index = 0, tmpSum = 0;

        for (int i = 1; i < n; i++) {
            if (nums[i] == nums[i - 1] && nums[i] == 0) return true;
            currentSum += nums[i];

            if (currentSum % k == 0) return true;

            tmpSum = currentSum;
            index = 0;

            while (index + 1 < i && tmpSum > k) {
                tmpSum -= nums[index];
                index++;

                if (tmpSum % k == 0) return true;
            }
        }

        return false;
    }
};
```

```C++
class Solution {
public:
    bool checkSubarraySum(vector<int>& nums, int k) {
        int n = nums.size();
        if (n == 1) return false;
        if (k == 0 || k == 1) return true;

        unordered_map<int,int> mp = {{0,-1}};

        int sum = 0;
        int rem = 0;
        for (int i = 0; i < n; i++) {
            sum += nums[i];
            rem = sum%k;
            if (mp.find(rem) == mp.end()) {
                mp[rem] = i;
                continue;
            }
            if (i - mp[rem] > 1) return true;
        }

        return false;
    }
};
```

# Complexity
## First Approach
- Time: O(n^2)
- Space: O(1)

## Second Approach
- Time: O(n)
- Space: O(n)