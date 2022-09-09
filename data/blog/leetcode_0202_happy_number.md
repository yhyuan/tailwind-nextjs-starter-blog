---
title: Leetcode 0202 happy number
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0202 happy number in Rust
---

## Solution in Rust

```rust
/**
 * [202] Happy Number
 *
 * Write an algorithm to determine if a number n is happy.
 * A happy number is a number defined by the following process:
 *
 * 	Starting with any positive integer, replace the number by the sum of the squares of its digits.
 * 	Repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1.
 * 	Those numbers for which this process ends in 1 are happy.
 *
 * Return true if n is a happy number, and false if not.
 *
 * Example 1:
 *
 * Input: n = 19
 * Output: true
 * Explanation:
 * 1^2 + 9^2 = 82
 * 8^2 + 2^2 = 68
 * 6^2 + 8^2 = 100
 * 1^2 + 0^2 + 0^2 = 1
 *
 * Example 2:
 *
 * Input: n = 2
 * Output: false
 *
 *
 * Constraints:
 *
 * 	1 <= n <= 2^31 - 1
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/happy-number/
// discuss: https://leetcode.com/problems/happy-number/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
use std::collections::HashSet;
impl Solution {
    pub fn is_happy_helper(n: i32) -> i32 {
        let mut n = n;
        let mut res = 0;
        while n > 9 {
            let x = n % 10;
            res += x * x;
            n = n / 10;
        }
        res + n * n
    }
    pub fn is_happy(n: i32) -> bool {
        let mut n = n;
        let mut hashset: HashSet<i32> = HashSet::new();
        hashset.insert(n);
        while n != 1 {
            let new_n = Solution::is_happy_helper(n);
            if hashset.contains(&new_n) {
                return false;
            }
            hashset.insert(new_n);
            n = new_n;
        }
        true
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_202() {
        assert_eq!(Solution::is_happy(19), true);
        assert_eq!(Solution::is_happy(2), true);
    }
}

```
