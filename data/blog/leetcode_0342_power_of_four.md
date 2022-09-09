---
title: Leetcode 0342 power of four
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0342 power of four in Rust
---

## Solution in Rust

```rust
/**
 * [342] Power of Four
 *
 * Given an integer n, return true if it is a power of four. Otherwise, return false.
 * An integer n is a power of four, if there exists an integer x such that n == 4^x.
 *
 * Example 1:
 * Input: n = 16
 * Output: true
 * Example 2:
 * Input: n = 5
 * Output: false
 * Example 3:
 * Input: n = 1
 * Output: true
 *
 * Constraints:
 *
 * 	-2^31 <= n <= 2^31 - 1
 *
 *
 * Follow up: Could you solve it without loops/recursion?
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/power-of-four/
// discuss: https://leetcode.com/problems/power-of-four/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn is_power_of_four(n: i32) -> bool {
        if n <= 0 {
            return false;
        }
        let r = (n as f64).log10() / 4f64.log10();
        4u32.pow(r.ceil() as u32) == n as u32 || 4u32.pow(r.floor() as u32) == n as u32
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_342() {
        assert_eq!(Solution::is_power_of_four(16), true);
        //assert_eq!(Solution::is_power_of_four(5), false);
        //assert_eq!(Solution::is_power_of_four(1), true);
    }
}

```
