---
title: Leetcode 0152 maximum product subarray
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0152 maximum product subarray in Rust
---

## Solution in Rust

```rust
/**
 * [152] Maximum Product Subarray
 *
 * Given an integer array nums, find a contiguous non-empty subarray within the array that has the largest product, and return the product.
 * It is guaranteed that the answer will fit in a 32-bit integer.
 * A subarray is a contiguous subsequence of the array.
 *
 * Example 1:
 *
 * Input: nums = [2,3,-2,4]
 * Output: 6
 * Explanation: [2,3] has the largest product 6.
 *
 * Example 2:
 *
 * Input: nums = [-2,0,-1]
 * Output: 0
 * Explanation: The result cannot be 2, because [-2,-1] is not a subarray.
 *
 *
 * Constraints:
 *
 * 	1 <= nums.length <= 2 * 10^4
 * 	-10 <= nums[i] <= 10
 * 	The product of any prefix or suffix of nums is guaranteed to fit in a 32-bit integer.
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/maximum-product-subarray/
// discuss: https://leetcode.com/problems/maximum-product-subarray/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn max_product(nums: Vec<i32>) -> i32 {
        /*
        let n = nums.len();
        let mut min_dp: Vec<i32> = vec![0; n];
        let mut max_dp: Vec<i32> = vec![0; n];
        min_dp[0] = nums[0];
        max_dp[0] = nums[0];
        let mut result = nums[0];
        for i in 1..n {
            max_dp[i] = i32::max(i32::max(min_dp[i - 1] * nums[i], max_dp[i - 1] * nums[i]), nums[i]);
            min_dp[i] = i32::min(i32::min(min_dp[i - 1] * nums[i], max_dp[i - 1] * nums[i]), nums[i]);
            result = i32::max(result, max_dp[i]);
        }
        result
        */
        let n = nums.len();
        let mut dp = (nums[0], nums[0]);
        let mut result = nums[0];
        for i in 1..n {
            dp = (
                i32::max(i32::max(dp.1 * nums[i], dp.0 * nums[i]), nums[i]),
                i32::min(i32::min(dp.1 * nums[i], dp.0 * nums[i]), nums[i])
            );
            result = i32::max(result, dp.0);
        }
        result
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_152() {
        //assert_eq!(Solution::max_product(vec![2,3,-2,4]), 6);
        //;lkassert_eq!(Solution::max_product(vec![-2,0,-1]), 0);
        assert_eq!(Solution::max_product(vec![-4,-3,-2]), 12);
    }
}

```
