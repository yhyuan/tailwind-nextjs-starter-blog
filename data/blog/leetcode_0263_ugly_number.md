---
title: Leetcode 0263 ugly number
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0263 ugly number in Rust
---

## Solution in Rust

```rust
/**
 * [263] Ugly Number
 *
 * An ugly number is a positive integer whose prime factors are limited to 2, 3, and 5.
 * Given an integer n, return true if n is an ugly number.
 *
 * Example 1:
 *
 * Input: n = 6
 * Output: true
 * Explanation: 6 = 2 &times; 3
 * Example 2:
 *
 * Input: n = 8
 * Output: true
 * Explanation: 8 = 2 &times; 2 &times; 2
 *
 * Example 3:
 *
 * Input: n = 14
 * Output: false
 * Explanation: 14 is not ugly since it includes the prime factor 7.
 *
 * Example 4:
 *
 * Input: n = 1
 * Output: true
 * Explanation: 1 has no prime factors, therefore all of its prime factors are limited to 2, 3, and 5.
 *
 *
 * Constraints:
 *
 * 	-2^31 <= n <= 2^31 - 1
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/ugly-number/
// discuss: https://leetcode.com/problems/ugly-number/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn is_ugly(n: i32) -> bool {
        if n == 0 {
            return false;
        }
        let mut n = n;
        while n % 5 == 0 {
            n = n / 5;
        }
        while n % 3 == 0 {
            n = n / 3;
        }
        while n % 2 == 0 {
            n = n / 2;
        }

        n == 1
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_263() {
        assert_eq!(Solution::is_ugly(6), true);
        assert_eq!(Solution::is_ugly(8), true);
        assert_eq!(Solution::is_ugly(14), false);
        assert_eq!(Solution::is_ugly(1), true);
    }
}

```
