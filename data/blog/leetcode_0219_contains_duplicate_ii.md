---
title: Leetcode 0219 contains duplicate ii
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0219 contains duplicate ii in Rust
---

## Solution in Rust

```rust
/**
 * [219] Contains Duplicate II
 *
 * Given an integer array nums and an integer k, return true if there are two distinct indices i and j in the array such that nums[i] == nums[j] and abs(i - j) <= k.
 *
 * Example 1:
 *
 * Input: nums = [1,2,3,1], k = 3
 * Output: true
 *
 * Example 2:
 *
 * Input: nums = [1,0,1,1], k = 1
 * Output: true
 *
 * Example 3:
 *
 * Input: nums = [1,2,3,1,2,3], k = 2
 * Output: false
 *
 *
 * Constraints:
 *
 * 	1 <= nums.length <= 10^5
 * 	-10^9 <= nums[i] <= 10^9
 * 	0 <= k <= 10^5
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/contains-duplicate-ii/
// discuss: https://leetcode.com/problems/contains-duplicate-ii/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
use std::collections::HashMap;
impl Solution {
    pub fn contains_nearby_duplicate(nums: Vec<i32>, k: i32) -> bool {
        let mut hashmap: HashMap<i32, usize> = HashMap::new();
        for (i, num) in nums.iter().enumerate() {
            if hashmap.contains_key(num) && (i as i32 - hashmap[num] as i32).abs() <= k {
                return true;
            } else {
                hashmap.insert(*num, i);
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
    fn test_219() {
        assert_eq!(Solution::contains_nearby_duplicate(vec![1,2,3,1], 3), true);
        assert_eq!(Solution::contains_nearby_duplicate(vec![1,0,1,1], 1), true);
        assert_eq!(Solution::contains_nearby_duplicate(vec![1,2,3,1,2,3], 2), false);
    }
}

```
