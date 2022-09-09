---
title: Leetcode 0007 reverse integer
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0007 reverse integer in Rust
---

## Solution in Rust

```rust
/**
 * [7] Reverse Integer
 *
 * Given a signed 32-bit integer x, return x with its digits reversed. If reversing x causes the value to go outside the signed 32-bit integer range [-2^31, 2^31 - 1], then return 0.
 * Assume the environment does not allow you to store 64-bit integers (signed or unsigned).
 *
 * Example 1:
 * Input: x = 123
 * Output: 321
 * Example 2:
 * Input: x = -123
 * Output: -321
 * Example 3:
 * Input: x = 120
 * Output: 21
 * Example 4:
 * Input: x = 0
 * Output: 0
 *
 * Constraints:
 *
 * 	-2^31 <= x <= 2^31 - 1
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/reverse-integer/
// discuss: https://leetcode.com/problems/reverse-integer/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn reverse(x: i32) -> i32 {
        let is_neg = x < 0;
        let mut x = if is_neg {-x} else {x};
        let mut digits: Vec<i32> = vec![];
        while x > 0 {
            digits.push(x % 10);
            x = x / 10;
        }
        //println!("{:?}", digits);
        let mut x:i64 = 0;
        for i in 0..digits.len() {
            x = x * 10 + digits[i] as i64;
        }
        if x > i32::MAX as i64 {
            return 0;
        }
        let x = x as i32;
        if is_neg {-x} else {x}
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_7() {
        assert_eq!(Solution::reverse(123), 321);
        assert_eq!(Solution::reverse(-123), -321);
        assert_eq!(Solution::reverse(120), 21);
        assert_eq!(Solution::reverse(0), 0);
        assert_eq!(Solution::reverse(1534236469), 0);
    }
}



```
