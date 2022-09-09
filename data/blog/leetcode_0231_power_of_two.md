---
title: Leetcode 0231 power of two
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0231 power of two in Rust
---

## Solution in Rust

```rust
/**
 * [231] Power of Two
 *
 * Given an integer n, return true if it is a power of two. Otherwise, return false.
 * An integer n is a power of two, if there exists an integer x such that n == 2^x.
 *
 * Example 1:
 *
 * Input: n = 1
 * Output: true
 * Explanation: 2^0 = 1
 *
 * Example 2:
 *
 * Input: n = 16
 * Output: true
 * Explanation: 2^4 = 16
 *
 * Example 3:
 *
 * Input: n = 3
 * Output: false
 *
 * Example 4:
 *
 * Input: n = 4
 * Output: true
 *
 * Example 5:
 *
 * Input: n = 5
 * Output: false
 *
 *
 * Constraints:
 *
 * 	-2^31 <= n <= 2^31 - 1
 *
 *
 * Follow up: Could you solve it without loops/recursion?
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/power-of-two/
// discuss: https://leetcode.com/problems/power-of-two/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
/*
impl Solution {
    pub fn is_power_of_two(n: i32) -> bool {
        if n <= 0 {
            return false;
        }
        let mut count_one = 0;
        for i in 0..32 {
            let r = (n >> i) & 1i32;
            if r == 1 {
                count_one += 1;
            }
        }
        count_one == 1
    }
}
*/
impl Solution {
    pub fn is_power_of_two(n: i32) -> bool {
        if n <= 0 {
            return false;
        }
        let mut count_one = 0;
        let mut n = n;
        for i in 0..32 {
            if n & 1i32 != 0 {
                count_one += 1;
            }
            n = n >> 1;
        }
        count_one == 1
    }
}
// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_231() {
        assert_eq!(Solution::is_power_of_two(1), true);
        assert_eq!(Solution::is_power_of_two(16), true);
        assert_eq!(Solution::is_power_of_two(3), false);
        assert_eq!(Solution::is_power_of_two(4), true);
        assert_eq!(Solution::is_power_of_two(i32::MIN), false);
        assert_eq!(Solution::is_power_of_two(i32::MIN + 1), false);
    }
}

```
