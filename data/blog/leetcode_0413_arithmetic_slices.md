---
title: Leetcode 0413 arithmetic slices
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0413 arithmetic slices in Rust
---

## Solution in Rust

```rust
/**
 * [413] Arithmetic Slices
 *
 * An integer array is called arithmetic if it consists of at least three elements and if the difference between any two consecutive elements is the same.
 *
 * 	For example, [1,3,5,7,9], [7,7,7,7], and [3,-1,-5,-9] are arithmetic sequences.
 *
 * Given an integer array nums, return the number of arithmetic subarrays of nums.
 * A subarray is a contiguous subsequence of the array.
 *
 * Example 1:
 *
 * Input: nums = [1,2,3,4]
 * Output: 3
 * Explanation: We have 3 arithmetic slices in nums: [1, 2, 3], [2, 3, 4] and [1,2,3,4] itself.
 *
 * Example 2:
 *
 * Input: nums = [1]
 * Output: 0
 *
 *
 * Constraints:
 *
 * 	1 <= nums.length <= 5000
 * 	-1000 <= nums[i] <= 1000
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/arithmetic-slices/
// discuss: https://leetcode.com/problems/arithmetic-slices/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn number_of_arithmetic_slices(nums: Vec<i32>) -> i32 {
        let n = nums.len();
        // dp[i][j] is &nums[i..=j] is a arithmetic slice or not.
        let mut count = 0;
        for i in 0..n {
            let mut dp = false;
            for j in i + 1..n {
                dp = if j == i + 1 {
                    true
                } else {
                    nums[j] - nums[j - 1] == nums[j-1] - nums[j -2] && dp
                };
                if j - i + 1 >= 3 && dp {
                    count += 1;
                }
            }
        }
        count
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_413() {
        assert_eq!(Solution::number_of_arithmetic_slices(vec![1,2,3,8,9,10]), 2);
        assert_eq!(Solution::number_of_arithmetic_slices(vec![1, 2, 3, 4]), 3);
        assert_eq!(Solution::number_of_arithmetic_slices(vec![1]), 0);
    }
}


```
