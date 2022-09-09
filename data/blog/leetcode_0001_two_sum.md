---
title: Leetcode 0001 two sum
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0001 two sum in Rust
---

## Solution in Rust

```rust
/**
 * [1] Two Sum
 *
 * Given an array of integers, return indices of the two numbers such that they
 * add up to a specific target.
 *
 * You may assume that each input would have exactly one solution, and you may
 * not use the same element twice.
 *
 * Example:
 *
 *
 * Given nums = [2, 7, 11, 15], target = 9,
 *
 * Because nums[0] + nums[1] = 2 + 7 = 9,
 * return [0, 1].
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/two-sum/
// discuss: https://leetcode.com/problems/two-sum/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

use std::collections::HashMap;
impl Solution {
    pub fn two_sum(nums: Vec<i32>, target: i32) -> Vec<i32> {
        let mut hashmap: HashMap<i32, usize> = HashMap::with_capacity(nums.len());
        for (i, num) in nums.iter().enumerate() {
            match hashmap.get(&num) {
                None => {
                    hashmap.insert(target - num, i);
                }
                Some(&index) => {
                    return vec![index as i32, i as i32];
                }
            }
        }
        vec![]
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_1() {
        assert_eq!(vec![0, 1], Solution::two_sum(vec![2, 7, 11, 15], 9));
        assert_eq!(vec![1, 2], Solution::two_sum(vec![3, 2, 4], 6));
    }
}

```
