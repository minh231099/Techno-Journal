# [330. Patching Array](https://leetcode.com/problems/patching-array/description/)

|Level|Constraints|
|------|----------|
|**Medium**|1 <= `nums.length` <= 1000|
||0 <= `nums[i]` <= 10<sup>4</sup>|
||nums is sorted in **ascending order**.|
||n == `capital.length`|
||1 <= `n` <= 2<sup>31</sup> - 1|

# Description
Given a sorted integer array `nums` and an integer n, add/patch elements to the array such that any number in the range `[1, n]` inclusive can be formed by the sum of some elements in the array.

Return *the minimum number of patches required*.

# Example
```
Input: nums = [1,3], n = 6
Output: 1
Explanation:
Combinations of nums are [1], [3], [1,3], which form possible sums of: 1, 3, 4.
Now if we add/patch 2 to nums, the combinations are: [1], [2], [3], [1,3], [2,3], [1,2,3].
Possible sums are 1, 2, 3, 4, 5, 6, which now covers the range [1, 6].
So we only need 1 patch.
```
```
Input: nums = [1,5,10], n = 20
Output: 2
Explanation: The two patches can be [2, 4].
```
```
Input: nums = [1,2,2], n = 5
Output: 0
```

# Approach
- **Initialize Variables**: Start with miss set to 1, added to 0, and index i at 0 to track the smallest sum that cannot be formed and the number of patches added.

- **Iterate with Condition**: Continue the loop as long as miss (the target sum we need to achieve) is less than or equal to n.

- **Use Existing Numbers**: Check if the current number in the list (nums[i]) can be used to form miss. If yes, add it to miss to extend the range of formable sums and increment the index i.

- **Add Necessary Numbers**: If nums[i] is greater than miss or all numbers are already used, add miss itself to cover the smallest unformable sum, and double the value of miss.

- **Increment Added Count**: Whenever a number is added to cover a gap, increase the added counter.

- **Return Total Patches**: Once miss exceeds n, return the total count of added numbers as the result, representing the minimum patches needed to form every number up to n.

# Code
```C++
class Solution {
public:
    int minPatches(vector<int>& nums, int n) {
        long miss = 1; 
        int patches = 0;
        int i = 0;
        
        while (miss <= n) {
            if (i < nums.size() && nums[i] <= miss) {
                miss += nums[i];
                i++;
            } else {
                miss += miss;
                patches++;
            }
        }
        
        return patches;
    }
};
```

# Complexity
- Time: O(n)
- Space: O(n)