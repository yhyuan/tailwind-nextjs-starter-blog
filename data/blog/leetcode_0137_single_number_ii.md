---
title: Leetcode 0137 single number ii
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0137 single number ii in Rust
---

## Solution in Rust

```rust
/**
 * [137] Single Number II
 *
 * Given an integer array nums where every element appears three times except for one, which appears exactly once. Find the single element and return it.
 * You must implement a solution with a linear runtime complexity and use only constant extra space.
 *
 * Example 1:
 * Input: nums = [2,2,3,2]
 * Output: 3
 * Example 2:
 * Input: nums = [0,1,0,1,0,1,99]
 * Output: 99
 *
 * Constraints:
 *
 * 	1 <= nums.length <= 3 * 10^4
 * 	-2^31 <= nums[i] <= 2^31 - 1
 * 	Each element in nums appears exactly three times except for one element which appears once.
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/single-number-ii/
// discuss: https://leetcode.com/problems/single-number-ii/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn single_number(nums: Vec<i32>) -> i32 {
        //!("nums: {:?}", nums);
        let mut a: i32 = 0i32;
        let mut b: i32 = 0i32;
        for &num in nums.iter() {
            a = a ^ num & !b;
            b = b ^ num & !a;
        }
        a | b
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_137() {
        assert_eq!(Solution::single_number(vec![0, 0, 0, 1, 1, 1, 5]), 5);
    }
}

```
