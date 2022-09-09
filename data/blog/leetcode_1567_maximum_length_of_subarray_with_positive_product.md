---
title: Leetcode 1567 maximum length of subarray with positive product
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 1567 maximum length of subarray with positive product in Rust
---

## Solution in Rust

```rust
/**
 * [1567] Maximum Length of Subarray With Positive Product
 *
 * Given an array of integers nums, find the maximum length of a subarray where the product of all its elements is positive.
 * A subarray of an array is a consecutive sequence of zero or more values taken out of that array.
 * Return the maximum length of a subarray with positive product.
 *
 * Example 1:
 *
 * Input: nums = [1,-2,-3,4]
 * Output: 4
 * Explanation: The array nums already has a positive product of 24.
 *
 * Example 2:
 *
 * Input: nums = [0,1,-2,-3,-4]
 * Output: 3
 * Explanation: The longest subarray with positive product is [1,-2,-3] which has a product of 6.
 * Notice that we cannot include 0 in the subarray since that'll make the product 0 which is not positive.
 * Example 3:
 *
 * Input: nums = [-1,-2,-3,0,1]
 * Output: 2
 * Explanation: The longest subarray with positive product is [-1,-2] or [-2,-3].
 *
 * Example 4:
 *
 * Input: nums = [-1,2]
 * Output: 1
 *
 * Example 5:
 *
 * Input: nums = [1,2,3,5,-6,4,0,10]
 * Output: 4
 *
 *
 * Constraints:
 *
 * 	1 <= nums.length <= 10^5
 * 	-10^9 <= nums[i] <= 10^9
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/maximum-length-of-subarray-with-positive-product/
// discuss: https://leetcode.com/problems/maximum-length-of-subarray-with-positive-product/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn get_max_len(nums: Vec<i32>) -> i32 {
        // dp_pos[n] = if nums[i] > 0 {dp_pos[n - 1] + 1}
        let n = nums.len();
        let mut dp_pos: Vec<i32> = vec![0; n];
        let mut dp_neg: Vec<i32> = vec![0; n];
        dp_pos[0] = if nums[0] > 0 {1} else if nums[0] < 0 {0} else {0};
        dp_neg[0] = if nums[0] > 0 {0} else if nums[0] < 0 {1} else {0};
        for i in 1..n {
            if nums[i] > 0 {
                dp_pos[i] = if dp_pos[i - 1] == 0 {1} else {dp_pos[i - 1] + 1};
                dp_neg[i] = if dp_neg[i - 1] == 0 {0} else {dp_neg[i - 1] + 1};
            } else if nums[i] < 0 {
                dp_pos[i] = if dp_neg[i - 1] == 0 {0} else {dp_neg[i - 1] + 1};
                dp_neg[i] = if dp_pos[i - 1] == 0 {1} else {dp_pos[i - 1] + 1};
            } else {
                dp_pos[i] = 0;
                dp_neg[i] = 0;
            }
        }
        *dp_pos.iter().max().unwrap()
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_1567() {
        assert_eq!(Solution::get_max_len(vec![1,-2,-3,4]), 4);
        assert_eq!(Solution::get_max_len(vec![0,1,-2,-3,-4]), 3);
        assert_eq!(Solution::get_max_len(vec![-1,-2,-3,0,1]), 2);
    }
}

```
