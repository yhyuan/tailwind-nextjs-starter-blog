---
title: Leetcode 0053 maximum subarray
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0053 maximum subarray in Rust
---

## Solution in Rust

```rust
/**
 * [53] Maximum Subarray
 *
 * Given an integer array nums, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.
 * A subarray is a contiguous part of an array.
 *
 * Example 1:
 *
 * Input: nums = [-2,1,-3,4,-1,2,1,-5,4]
 * Output: 6
 * Explanation: [4,-1,2,1] has the largest sum = 6.
 *
 * Example 2:
 *
 * Input: nums = [1]
 * Output: 1
 *
 * Example 3:
 *
 * Input: nums = [5,4,-1,7,8]
 * Output: 23
 *
 *
 * Constraints:
 *
 * 	1 <= nums.length <= 3 * 10^4
 * 	-10^5 <= nums[i] <= 10^5
 *
 *
 * Follow up: If you have figured out the O(n) solution, try coding another solution using the divide and conquer approach, which is more subtle.
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/maximum-subarray/
// discuss: https://leetcode.com/problems/maximum-subarray/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
impl Solution {
    pub fn max_sub_array(nums: Vec<i32>) -> i32 {
        let n = nums.len();
        let mut dp: Vec<i32> = vec![0; n];
        dp[0] = nums[0];
        for i in 1..n {
            dp[i] = i32::max(dp[i - 1] + nums[i], nums[i]);
        }
        let max_value = dp.iter().max().unwrap();
        *max_value
    }
}

/*
impl Solution {
    pub fn max_sub_array(nums: Vec<i32>) -> i32 {
        let mut max_sum = i32::MIN;
        let mut cur_sum = 0;
        for num in nums {
            cur_sum += num;
            if cur_sum > max_sum {
                max_sum = cur_sum;
            }
            if cur_sum <0 {
                cur_sum = 0;
            }
        }
        return max_sum;
    }
}
*/
// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_53() {
        assert_eq!(6, Solution::max_sub_array(vec![-2,1,-3,4,-1,2,1,-5,4]));
        assert_eq!(1, Solution::max_sub_array(vec![1]));
        assert_eq!(23, Solution::max_sub_array(vec![5,4,-1,7,8]));
    }
}

```
