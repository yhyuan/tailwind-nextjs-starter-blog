---
title: Leetcode 0009 palindrome number
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0009 palindrome number in Rust
---

## Solution in Rust

```rust
/**
 * [9] Palindrome Number
 *
 * Given an integer x, return true if x is palindrome integer.
 * An integer is a palindrome when it reads the same backward as forward. For example, 121 is palindrome while 123 is not.
 *
 * Example 1:
 *
 * Input: x = 121
 * Output: true
 *
 * Example 2:
 *
 * Input: x = -121
 * Output: false
 * Explanation: From left to right, it reads -121. From right to left, it becomes 121-. Therefore it is not a palindrome.
 *
 * Example 3:
 *
 * Input: x = 10
 * Output: false
 * Explanation: Reads 01 from right to left. Therefore it is not a palindrome.
 *
 * Example 4:
 *
 * Input: x = -101
 * Output: false
 *
 *
 * Constraints:
 *
 * 	-2^31 <= x <= 2^31 - 1
 *
 *
 * Follow up: Could you solve it without converting the integer to a string?
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/palindrome-number/
// discuss: https://leetcode.com/problems/palindrome-number/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn is_palindrome(x: i32) -> bool {
        if x < 0 {
            return  false;
        }
        let mut v = x;
        let mut digits: Vec<i32> = vec![];
        while v > 0 {
            digits.push(v % 10);
            v = v / 10;
        }
        let mut v = 0;
        for i in 0..digits.len() {
            v = v * 10 + digits[i];
        }
        v == x
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_9() {
        assert_eq!(Solution::is_palindrome(-32), false);
        assert_eq!(Solution::is_palindrome(10), false);
        assert_eq!(Solution::is_palindrome(0), true);
        assert_eq!(Solution::is_palindrome(9), true);
        assert_eq!(Solution::is_palindrome(121), true);
        assert_eq!(Solution::is_palindrome(2222), true);
        assert_eq!(Solution::is_palindrome(11222211), true);
    }
}

```
