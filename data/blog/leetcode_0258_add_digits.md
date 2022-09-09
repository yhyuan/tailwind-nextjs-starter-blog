---
title: Leetcode 0258 add digits
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0258 add digits in Rust
---

## Solution in Rust

```rust
/**
 * [258] Add Digits
 *
 * Given an integer num, repeatedly add all its digits until the result has only one digit, and return it.
 *
 * Example 1:
 *
 * Input: num = 38
 * Output: 2
 * Explanation: The process is
 * 38 --> 3 + 8 --> 11
 * 11 --> 1 + 1 --> 2
 * Since 2 has only one digit, return it.
 *
 * Example 2:
 *
 * Input: num = 0
 * Output: 0
 *
 *
 * Constraints:
 *
 * 	0 <= num <= 2^31 - 1
 *
 *
 * Follow up: Could you do it without any loop/recursion in O(1) runtime?
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/add-digits/
// discuss: https://leetcode.com/problems/add-digits/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn add_digits(num: i32) -> i32 {
        if num == 0 {
            return num;
        }
        let r = num % 9;
        if r == 0 {9} else {r}
        /*
        if num < 10 {
            return num;
        }
        let mut total = 0;
        let mut num = num;
        while num > 9 {
            total += num % 10;
            num = num / 10;
        }
        total += num;
        if total > 9 {Solution::add_digits(total)} else {total}
        */
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_258() {
        assert_eq!(Solution::add_digits(1234), 1);
    }
}

```
