---
title: Leetcode 0191 number of 1 bits
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0191 number of 1 bits in Rust
---

## Solution in Rust

```rust
/**
 * [191] Number of 1 Bits
 *
 * Write a function that takes an unsigned integer and returns the number of '1' bits it has (also known as the <a href="http://en.wikipedia.org/wiki/Hamming_weight" target="_blank">Hamming weight</a>).
 * Note:
 *
 * 	Note that in some languages, such as Java, there is no unsigned integer type. In this case, the input will be given as a signed integer type. It should not affect your implementation, as the integer's internal binary representation is the same, whether it is signed or unsigned.
 * 	In Java, the compiler represents the signed integers using <a href="https://en.wikipedia.org/wiki/Two%27s_complement" target="_blank">2's complement notation</a>. Therefore, in Example 3, the input represents the signed integer. -3.
 *
 *
 * Example 1:
 *
 * Input: n = 00000000000000000000000000001011
 * Output: 3
 * Explanation: The input binary string 00000000000000000000000000001011 has a total of three '1' bits.
 *
 * Example 2:
 *
 * Input: n = 00000000000000000000000010000000
 * Output: 1
 * Explanation: The input binary string 00000000000000000000000010000000 has a total of one '1' bit.
 *
 * Example 3:
 *
 * Input: n = 11111111111111111111111111111101
 * Output: 31
 * Explanation: The input binary string 11111111111111111111111111111101 has a total of thirty one '1' bits.
 *
 *
 * Constraints:
 *
 * 	The input must be a binary string of length 32.
 *
 *
 * Follow up: If this function is called many times, how would you optimize it?
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/number-of-1-bits/
// discuss: https://leetcode.com/problems/number-of-1-bits/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn hamming_weight (n: u32) -> i32 {
        /*
        let mut n = n;
        let mut res = 0;
        for i in 0..32 {
            if n == 0 {
                break;
            }
            let last_bit = n & 1u32;
            n = n >> 1;
            if last_bit > 0 {
                res += 1;
            }
        }
        res
        */
        n.count_ones() as i32
    }
}
/**
 impl Solution {
    pub fn hammingWeight (n: u32) -> i32 {
        let mut n = n;
        let mut count = 0i32;
        for _ in 0..32 {
            if n & 1u32 == 1 {
                count += 1;
            }
            n = n >> 1;
        }
        count as i32
    }
}
 */
// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_191() {
        assert_eq!(Solution::hamming_weight(0b00000000000000000000000000001011), 3);
        assert_eq!(Solution::hamming_weight(0b00000000000000000000000010000000), 1);
        assert_eq!(Solution::hamming_weight(0b11111111111111111111111111111101), 31);
    }
}

```
