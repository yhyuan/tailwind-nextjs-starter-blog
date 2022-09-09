---
title: Leetcode 0050 powx n
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0050 powx n in Rust
---

## Solution in Rust

```rust
/**
 * [50] Pow(x, n)
 *
 * Implement <a href="http://www.cplusplus.com/reference/valarray/pow/" target="_blank">pow(x, n)</a>, which calculates x raised to the power n (i.e., x^n).
 *
 * Example 1:
 *
 * Input: x = 2.00000, n = 10
 * Output: 1024.00000
 *
 * Example 2:
 *
 * Input: x = 2.10000, n = 3
 * Output: 9.26100
 *
 * Example 3:
 *
 * Input: x = 2.00000, n = -2
 * Output: 0.25000
 * Explanation: 2^-2 = 1/2^2 = 1/4 = 0.25
 *
 *
 * Constraints:
 *
 * 	-100.0 < x < 100.0
 * 	-2^31 <= n <= 2^31-1
 * 	-10^4 <= x^n <= 10^4
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/powx-n/
// discuss: https://leetcode.com/problems/powx-n/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn my_pow(x: f64, n: i32) -> f64 {
        if n == 0 {
            return 1.0f64;
        }
        if n < 0 {
            if n == i32::MIN {
                return (1.0f64 / Solution::my_pow(x, -(n+ 1))) / x;
            }
            return 1.0f64 / Solution::my_pow(x, -n);
        }
        let mut n = n;
        let mut x_mul = x;
        let mut result = 1.0f64;
        while n > 0 {
            if (n & 1i32) == 1 {
                result = result * x_mul;
            }
            x_mul = x_mul * x_mul;
            n = n >> 1;
        }
        result
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_50() {
        assert_eq!(Solution::my_pow(2.0, -2), 0.25);
        assert_eq!(Solution::my_pow(1.0, i32::MIN), 1.0);

        /*
        assert_eq!(Solution::my_pow(2.0, 4), 16.0);
        assert_eq!(Solution::my_pow(2.0, 5), 32.0);
        assert_eq!(Solution::my_pow(2.0, 1), 2.0);
        assert_eq!(Solution::my_pow(2.0, -1), 0.5);
        assert_eq!(Solution::my_pow(2.0, 10), 1024.0);
        */
    }
}

```
