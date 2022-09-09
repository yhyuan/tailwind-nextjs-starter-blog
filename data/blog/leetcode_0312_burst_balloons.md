---
title: Leetcode 0312 burst balloons
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0312 burst balloons in Rust
---

## Solution in Rust

```rust
/**
 * [312] Burst Balloons
 *
 * You are given n balloons, indexed from 0 to n - 1. Each balloon is painted with a number on it represented by an array nums. You are asked to burst all the balloons.
 * If you burst the i^th balloon, you will get nums[i - 1] * nums[i] * nums[i + 1] coins. If i - 1 or i + 1 goes out of bounds of the array, then treat it as if there is a balloon with a 1 painted on it.
 * Return the maximum coins you can collect by bursting the balloons wisely.
 *
 * Example 1:
 *
 * Input: nums = [3,1,5,8]
 * Output: 167
 * Explanation:
 * nums = [3,1,5,8] --> [3,5,8] --> [3,8] --> [8] --> []
 * coins =  3*1*5    +   3*5*8   +  1*3*8  + 1*8*1 = 167
 * Example 2:
 *
 * Input: nums = [1,5]
 * Output: 10
 *
 *
 * Constraints:
 *
 * 	n == nums.length
 * 	1 <= n <= 500
 * 	0 <= nums[i] <= 100
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/burst-balloons/
// discuss: https://leetcode.com/problems/burst-balloons/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn helper(nums: &Vec<i32>, start: usize, end: usize, memo: &mut Vec<Vec<i32>>) -> i32 {
        if memo[start][end] != 0 {
            return memo[start][end];
        }
        if start == end {
            memo[start][end] = nums[start - 1] * nums[start] * nums[start + 1];
            return memo[start][end];
        }

        let mut max = 0;
        for i in start..=end {
            let left_max = if i > start {Solution::helper(nums, start, i - 1, memo)} else {0};
            let right_max = if i < end {Solution::helper(nums, i + 1, end, memo)} else {0};
            let result = left_max + nums[start - 1] * nums[i] * nums[end + 1] + right_max; // Last removed
            max = i32::max(max, result);
        }
        memo[start][end] = max;
        max
    }
    pub fn max_coins(nums: Vec<i32>) -> i32 {
        let size = nums.len();
        if size == 0 {
            return 0;
        }
        if size == 1 {
            return nums[0];
        }
        let mut nums: Vec<i32> = (0..nums.len()).into_iter()
            .filter(|&i| nums[i] != 0)
            .map(|i| nums[i]).collect();
        let len = nums.len();
        nums.insert(0, 1);
        nums.push(1);
        //let len = nums.len() + 2;
        let mut memo = vec![vec![0; len + 1]; len + 1];
        //println!("{:?}", nums);
        Solution::helper(&nums, 1, len, &mut memo)
        //0
        /*
        let mut max_result = i32::MIN;
        for i in 0..nums.len() {
            let left = if i == 0 {1} else {nums[i - 1]};
            let right = if i == (size - 1) {1} else {nums[i + 1]};
            let product = left * nums[i]  * right;
            let indices: Vec<usize> = (0..nums.len()).into_iter().filter(|&index| index != i).collect();
            let new_nums: Vec<i32> = indices.iter().map(|&index| nums[index]).collect();
            max_result = i32::max(product + Solution::max_coins(new_nums), max_result);
        }
        max_result
        */
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_312() {
        assert_eq!(Solution::max_coins(vec![3, 1, 5, 8]), 167);
    }
}

```
