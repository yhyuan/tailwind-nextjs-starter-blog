---
title: Leetcode 0069 sqrtx
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0069 sqrtx in Rust
---

## Solution in Rust

```rust
/**
 * [69] Sqrt(x)
 *
 * Given a non-negative integer x, compute and return the square root of x.
 * Since the return type is an integer, the decimal digits are truncated, and only the integer part of the result is returned.
 * Note: You are not allowed to use any built-in exponent function or operator, such as pow(x, 0.5) or x ** 0.5.
 *
 * Example 1:
 *
 * Input: x = 4
 * Output: 2
 *
 * Example 2:
 *
 * Input: x = 8
 * Output: 2
 * Explanation: The square root of 8 is 2.82842..., and since the decimal part is truncated, 2 is returned.
 *
 * Constraints:
 *
 * 	0 <= x <= 2^31 - 1
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/sqrtx/
// discuss: https://leetcode.com/problems/sqrtx/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn helper(x: i32, start: i32, end: i32) -> i32 {
        if start == end {
            return start;
        }
        if end - start == 1 {
            return start;
        }
        //println!("start: {}, end: {}", start, end);
        let middle = (start + end) / 2;
        let middle_square = middle as i64 * middle as i64;
        if middle_square == x as i64 {
            middle
        } else if middle_square > x as i64 {
            Solution::helper(x, start, middle)
        } else {
            Solution::helper(x, middle, end)
        }
    }

    pub fn my_sqrt(x: i32) -> i32 {
        if x == 1 {
            return 1;
        }
        Solution::helper(x, 0, x)
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_69() {
        assert_eq!(Solution::my_sqrt(8), 2);

        assert_eq!(Solution::my_sqrt(16), 4);
        assert_eq!(Solution::my_sqrt(17), 4);
        assert_eq!(Solution::my_sqrt(81), 9);
        assert_eq!(Solution::my_sqrt(82), 9);
        assert_eq!(Solution::my_sqrt(100480577), 10024);
        assert_eq!(Solution::my_sqrt(100480575), 10023);
        assert_eq!(Solution::my_sqrt(100480575), 10023);
        assert_eq!(Solution::my_sqrt(80), 8);
        assert_eq!(Solution::my_sqrt(2), 1);

    }
}

```
