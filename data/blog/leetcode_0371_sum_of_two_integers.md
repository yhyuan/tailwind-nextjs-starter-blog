---
title: Leetcode 0371 sum of two integers
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0371 sum of two integers in Rust
---

## Solution in Rust

```rust
/**
 * [371] Sum of Two Integers
 *
 * Given two integers a and b, return the sum of the two integers without using the operators + and -.
 *
 * Example 1:
 * Input: a = 1, b = 2
 * Output: 3
 * Example 2:
 * Input: a = 2, b = 3
 * Output: 5
 *
 * Constraints:
 *
 * 	-1000 <= a, b <= 1000
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/sum-of-two-integers/
// discuss: https://leetcode.com/problems/sum-of-two-integers/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn get_sum(a: i32, b: i32) -> i32 {
        if b == 0 {
            return a;
        }
        //println!("a: {:b}, b: {:b}", a, b);
        let x = a ^ b; //adding without carry.
        let y = (a & b) << 1; //adding with carry
        Self::get_sum(x, y)
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_371() {
        assert_eq!(Solution::get_sum(1, 2), 3);
        assert_eq!(Solution::get_sum(-2, 3), 1);
    }
}

```
