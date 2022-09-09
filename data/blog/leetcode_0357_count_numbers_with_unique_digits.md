---
title: Leetcode 0357 count numbers with unique digits
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0357 count numbers with unique digits in Rust
---

## Solution in Rust

```rust
/**
 * [357] Count Numbers with Unique Digits
 *
 * Given an integer n, return the count of all numbers with unique digits, x, where 0 <= x < 10^n.
 *
 * Example 1:
 *
 * Input: n = 2
 * Output: 91
 * Explanation: The answer should be the total numbers in the range of 0 &le; x < 100, excluding 11,22,33,44,55,66,77,88,99
 *
 * Example 2:
 *
 * Input: n = 0
 * Output: 1
 *
 *
 * Constraints:
 *
 * 	0 <= n <= 8
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/count-numbers-with-unique-digits/
// discuss: https://leetcode.com/problems/count-numbers-with-unique-digits/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn count_numbers_with_unique_digits(n: i32) -> i32 {
        if n == 0 {
            return 1;
        }
        if n == 1 {
            return 10;
        }
        let mut result = 9;
        for i in 0..n - 1 {
            result = result * (9 - i);
        }
        result + Self::count_numbers_with_unique_digits(n - 1)
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_357() {
        assert_eq!(Solution::count_numbers_with_unique_digits(2), 91);
        assert_eq!(Solution::count_numbers_with_unique_digits(1), 10);
        assert_eq!(Solution::count_numbers_with_unique_digits(0), 1);
    }
}

```
