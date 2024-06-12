# [75. Sort Colors](https://leetcode.com/problems/sort-colors/description/)

|Level|Constraints|
|------|----------|
|**Medium**|`n == nums.length`|
||1 <= `n` <= 300|
||`nums[i]` is either 0, 1, or 2|

# Description
Given an array nums with n objects colored red, white, or blue, sort them in-place so that objects of the same color are adjacent, with the colors in the order red, white, and blue.  

We will use the integers 0, 1, and 2 to represent the color red, white, and blue, respectively.  

You must solve this problem without using the library's sort function.  

**Follow up**: Could you come up with a one-pass algorithm using only constant extra space?
# Example
```
Input: nums = [2,0,2,1,1,0]
Output: [0,0,1,1,2,2]
```
```
Input: nums = [2,0,1]
Output: [0,1,2]
```

# Approach
We just need to traverse throught `nums` two times.  
First, we will move all `0` to top of array. The second, we will move all `2` to the bottom of array. We do not need to care about `1`, because when `0` and `2` in the correct postition, so the same with `1`.

# Code
```C++
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int n = nums.size();
        int fIR = 0, fIB = n - 1, tmp;

        for (int i = 0; i < n; i++) {
            if (nums[i] == 0) {
                if (i > fIR) {
                    tmp = nums[i];
                    nums[i] = nums[fIR];
                    nums[fIR] = tmp;
                }
                fIR++;
            }
        }

        for (int i = n - 1; i >= 0; i--) {
            if (nums[i] == 2) {
                if (i < fIB) {
                    tmp = nums[i];
                    nums[i] = nums[fIB];
                    nums[fIB] = tmp;
                }
                fIB--;
            }
        }
    }
};
```

# Complexity
- Time: O(N)
- Space: O(1)