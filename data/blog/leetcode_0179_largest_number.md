---
title: Leetcode 0179 largest number
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0179 largest number in Rust
---

## Solution in Rust

```rust
/**
 * [179] Largest Number
 *
 * Given a list of non-negative integers nums, arrange them such that they form the largest number.
 * Note: The result may be very large, so you need to return a string instead of an integer.
 *
 * Example 1:
 *
 * Input: nums = [10,2]
 * Output: "210"
 *
 * Example 2:
 *
 * Input: nums = [3,30,34,5,9]
 * Output: "9534330"
 *
 * Example 3:
 *
 * Input: nums = [1]
 * Output: "1"
 *
 * Example 4:
 *
 * Input: nums = [10]
 * Output: "10"
 *
 *
 * Constraints:
 *
 * 	1 <= nums.length <= 100
 * 	0 <= nums[i] <= 10^9
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/largest-number/
// discuss: https://leetcode.com/problems/largest-number/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn largest_number(nums: Vec<i32>) -> String {
        let zero_count = nums.iter().filter(|&&num| num == 0i32).count();
        if zero_count == nums.len() {
            return "0".to_string();
        }
        let mut nums = nums;
        nums.sort_by(|a, b| {
            let a_b_string = format!("{}{}", a, b);
            let b_a_string = format!("{}{}", b, a);
            a_b_string.partial_cmp(&b_a_string).unwrap()
        });
        nums.reverse();
        let mut result: String = String::new();
        for num in nums.iter() {
            result = format!("{}{}", result, num);
        }
        result
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_179() {
        assert_eq!(
            Solution::largest_number(vec![3, 30, 34, 5, 9]),
            "9534330".to_owned()
        );
        assert_eq!(Solution::largest_number(vec![121, 12]), "12121".to_owned());
        assert_eq!(Solution::largest_number(vec![0, 0]), "0".to_owned());
    }
}

```
