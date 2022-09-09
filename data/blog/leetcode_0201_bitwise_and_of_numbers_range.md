---
title: Leetcode 0201 bitwise and of numbers range
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0201 bitwise and of numbers range in Rust
---

## Solution in Rust

```rust
/**
 * [201] Bitwise AND of Numbers Range
 *
 * Given two integers left and right that represent the range [left, right], return the bitwise AND of all numbers in this range, inclusive.
 *
 * Example 1:
 *
 * Input: left = 5, right = 7
 * Output: 4
 *
 * Example 2:
 *
 * Input: left = 0, right = 0
 * Output: 0
 *
 * Example 3:
 *
 * Input: left = 1, right = 2147483647
 * Output: 0
 *
 *
 * Constraints:
 *
 * 	0 <= left <= right <= 2^31 - 1
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/bitwise-and-of-numbers-range/
// discuss: https://leetcode.com/problems/bitwise-and-of-numbers-range/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn get_bit(x: i32, pos: u8) -> i32 {
        x >> pos & 1i32
    }
    pub fn set_bit(x: i32, pos: u8, bit: i32) -> i32 {
        if bit == 1i32 {
            1i32 << pos | x
        } else {
            !(1i32 << pos) & x
        }
    }
    pub fn range_bitwise_and(left: i32, right: i32) -> i32 {
        let mut result = 0i32;
        let mut diff_start = false;
        for i in (0..32u8).rev() { //31, 30, 29, ....... 0
            let left_bit = Solution::get_bit(left, i);
            let right_bit = Solution::get_bit(right, i);
            if left_bit != right_bit {
                result = Solution::set_bit(result, i, 0i32);
                diff_start = true;
            } else {
                if diff_start {
                    result = Solution::set_bit(result, i, 0i32);
                } else {
                    result = Solution::set_bit(result, i, left_bit);
                }
            }
        }
        result
        /*
        let mut res = left;
        for num in left..=right {
            res = res & num;
        }
        res
        */
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_201() {
        assert_eq!(Solution::range_bitwise_and(5, 7), 4);
    }
}

```
