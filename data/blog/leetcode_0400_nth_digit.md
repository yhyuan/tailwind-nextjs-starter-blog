---
title: Leetcode 0400 nth digit
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0400 nth digit in Rust
---

## Solution in Rust

```rust
/**
 * [400] Nth Digit
 *
 * Given an integer n, return the n^th digit of the infinite integer sequence [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, ...].
 *
 * Example 1:
 *
 * Input: n = 3
 * Output: 3
 *
 * Example 2:
 *
 * Input: n = 11
 * Output: 0
 * Explanation: The 11^th digit of the sequence 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, ... is a 0, which is part of the number 10.
 *
 *
 * Constraints:
 *
 * 	1 <= n <= 2^31 - 1
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/nth-digit/
// discuss: https://leetcode.com/problems/nth-digit/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn find_nth_digit(n: i32) -> i32 {
        if n <= 9 {
            return n;
        }
        //let nums: Vec<i32> = vec![];
        let mut n = n as u32;
        let mut i = 1u32;
        let mut limit = 9u32; //* 10u32.pow(i - 1) * i;
        while n > limit {
            n = n - limit;
            i += 1;
            let new_limit = (limit as u64 / (i - 1) as u64) * 10u64 * (i as u64);
            if new_limit > u32::MAX as u64 {
                break;
            }
            limit = new_limit as u32;
        }
        n = n - 1;
        let num = n / i;
        let num = 10u32.pow(i - 1) + num;
        let digit = n % i;
        let num = num / 10u32.pow(i - digit - 1);

        //println!("n: {}, i: {}, limit: {}", n, i, limit);
        (num % 10) as i32
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_400() {
        assert_eq!(Solution::find_nth_digit(3), 3);
        assert_eq!(Solution::find_nth_digit(11), 0);
    }
}

```
