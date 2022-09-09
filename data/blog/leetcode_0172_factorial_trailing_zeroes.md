---
title: Leetcode 0172 factorial trailing zeroes
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0172 factorial trailing zeroes in Rust
---

## Solution in Rust

```rust
/**
 * [172] Factorial Trailing Zeroes
 *
 * Given an integer n, return the number of trailing zeroes in n!.
 * Follow up: Could you write a solution that works in logarithmic time complexity?
 *
 * Example 1:
 *
 * Input: n = 3
 * Output: 0
 * Explanation: 3! = 6, no trailing zero.
 *
 * Example 2:
 *
 * Input: n = 5
 * Output: 1
 * Explanation: 5! = 120, one trailing zero.
 *
 * Example 3:
 *
 * Input: n = 0
 * Output: 0
 *
 *
 * Constraints:
 *
 * 	0 <= n <= 10^4
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/factorial-trailing-zeroes/
// discuss: https://leetcode.com/problems/factorial-trailing-zeroes/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn trailing_zeroes(n: i32) -> i32 {
        let mut n = n;
        let mut result = 0;
        while n >= 5 {
            result += n / 5;
            n = n / 5;
        }
        result
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_172() {
        assert_eq!(Solution::trailing_zeroes(3), 0);
        assert_eq!(Solution::trailing_zeroes(5), 1);
        assert_eq!(Solution::trailing_zeroes(20), 4);
        assert_eq!(Solution::trailing_zeroes(1808548329), 452137076);
    }
}

```
