---
title: Leetcode 0287 find the duplicate number
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0287 find the duplicate number in Rust
---

## Solution in Rust

```rust
/**
 * [287] Find the Duplicate Number
 *
 * Given an array of integers nums containing n + 1 integers where each integer is in the range [1, n] inclusive.
 * There is only one repeated number in nums, return this repeated number.
 * You must solve the problem without modifying the array nums and uses only constant extra space.
 *
 * Example 1:
 * Input: nums = [1,3,4,2,2]
 * Output: 2
 * Example 2:
 * Input: nums = [3,1,3,4,2]
 * Output: 3
 * Example 3:
 * Input: nums = [1,1]
 * Output: 1
 * Example 4:
 * Input: nums = [1,1,2]
 * Output: 1
 *
 * Constraints:
 *
 * 	1 <= n <= 10^5
 * 	nums.length == n + 1
 * 	1 <= nums[i] <= n
 * 	All the integers in nums appear only once except for precisely one integer which appears two or more times.
 *
 *
 * Follow up:
 *
 * 	How can we prove that at least one duplicate number must exist in nums?
 * 	Can you solve the problem in linear runtime complexity?
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/find-the-duplicate-number/
// discuss: https://leetcode.com/problems/find-the-duplicate-number/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn is_bit_set (item: u128, pos: i32) -> bool {
        let mut value = 1u128;
        value = value << pos;
        value = value & item;
        value > 0
    }
    pub fn set_bit (item: u128, pos: i32) -> u128 {
        let mut value = 1u128;
        value = value << pos;
        value = value | item;
        value
    }
    pub fn find_duplicate(nums: Vec<i32>) -> i32 {
        let mut v = vec![0u128; 782]; // 100096
        for num in nums {
            let index = (num / 128)  as usize;
            let pos = num % 128;
            if Solution::is_bit_set (v[index], pos) {
                return num;
            } else {
                v[index] = Solution::set_bit(v[index], pos);
            }
        }
        0i32
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_287() {
        assert_eq!(Solution::find_duplicate(vec![1, 3, 4, 2, 2]), 2);
        assert_eq!(Solution::find_duplicate(vec![3, 1, 3, 4, 2]), 3);
        assert_eq!(Solution::find_duplicate(vec![1, 2, 3, 4, 5, 5]), 5);
        assert_eq!(Solution::find_duplicate(vec![5, 1, 2, 3, 4, 5]), 5);
    }
}

```
