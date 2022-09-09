---
title: Leetcode 0217 contains duplicate
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0217 contains duplicate in Rust
---

## Solution in Rust

```rust
/**
 * [217] Contains Duplicate
 *
 * Given an integer array nums, return true if any value appears at least twice in the array, and return false if every element is distinct.
 *
 * Example 1:
 * Input: nums = [1,2,3,1]
 * Output: true
 * Example 2:
 * Input: nums = [1,2,3,4]
 * Output: false
 * Example 3:
 * Input: nums = [1,1,1,3,3,4,3,2,4,2]
 * Output: true
 *
 * Constraints:
 *
 * 	1 <= nums.length <= 10^5
 * 	-10^9 <= nums[i] <= 10^9
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/contains-duplicate/
// discuss: https://leetcode.com/problems/contains-duplicate/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
use std::collections::HashSet;
impl Solution {
    pub fn contains_duplicate(nums: Vec<i32>) -> bool {
        let mut hashset: HashSet<i32> = HashSet::new();
        for num in nums.iter() {
            if hashset.contains(num) {
                return true;
            } else {
                hashset.insert(*num);
            }
        }
        false
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_217() {
        assert_eq!(Solution::contains_duplicate(vec![1,2,3,1]), true);
        assert_eq!(Solution::contains_duplicate(vec![1,2,3,4]), false);
    }
}

```
