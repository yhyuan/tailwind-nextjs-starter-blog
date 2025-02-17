---
title: Leetcode 0338 counting bits
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0338 counting bits in Rust
---

## Solution in Rust

```rust
/**
 * [338] Counting Bits
 *
 * Given an integer n, return an array ans of length n + 1 such that for each i (0 <= i <= n), ans[i] is the number of 1's in the binary representation of i.
 *
 * Example 1:
 *
 * Input: n = 2
 * Output: [0,1,1]
 * Explanation:
 * 0 --> 0
 * 1 --> 1
 * 2 --> 10
 *
 * Example 2:
 *
 * Input: n = 5
 * Output: [0,1,1,2,1,2]
 * Explanation:
 * 0 --> 0
 * 1 --> 1
 * 2 --> 10
 * 3 --> 11
 * 4 --> 100
 * 5 --> 101
 *
 *
 * Constraints:
 *
 * 	0 <= n <= 10^5
 *
 *
 * Follow up:
 *
 * 	It is very easy to come up with a solution with a runtime of O(n log n). Can you do it in linear time O(n) and possibly in a single pass?
 * 	Can you do it without using any built-in function (i.e., like __builtin_popcount in C++)?
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/counting-bits/
// discuss: https://leetcode.com/problems/counting-bits/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn count_bits_helper(i: u32) -> i32 {
        let mut i = i;
        let mut count = 0;
        while i > 0 {
            if i % 2 == 1 {
                count += 1;
            }
            i = i / 2;
        }
        count
    }
    pub fn count_bits(n: i32) -> Vec<i32> {
        let n = n as usize;
        let mut result: Vec<i32> = vec![0; n + 1];
        for i in 0..=n {
            result[i] = i.count_ones() as i32; //Self::count_bits_helper(i as u32);
        }
        result
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_338() {
        assert_eq!(Solution::count_bits(2), vec![0, 1, 1]);
        assert_eq!(Solution::count_bits(5), vec![0, 1, 1, 2, 1, 2]);
    }
}

```
