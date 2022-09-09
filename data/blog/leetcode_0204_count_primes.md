---
title: Leetcode 0204 count primes
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0204 count primes in Rust
---

## Solution in Rust

```rust
/**
 * [204] Count Primes
 *
 * Count the number of prime numbers less than a non-negative number, n.
 *
 * Example 1:
 *
 * Input: n = 10
 * Output: 4
 * Explanation: There are 4 prime numbers less than 10, they are 2, 3, 5, 7.
 *
 * Example 2:
 *
 * Input: n = 0
 * Output: 0
 *
 * Example 3:
 *
 * Input: n = 1
 * Output: 0
 *
 *
 * Constraints:
 *
 * 	0 <= n <= 5 * 10^6
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/count-primes/
// discuss: https://leetcode.com/problems/count-primes/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn is_prime(num: i32, primes: &Vec<i32>) -> bool {
        let max_factor = (num as f64).sqrt().floor() as i32;
        for &prime in primes.iter() {
            if prime > max_factor {
                break;
            }
            if num % prime == 0 {
                return false;
            }
        }
        true
    }

    pub fn count_primes(n: i32) -> i32 {
        if n < 2 {
            return 0;
        }
        let mut primes: Vec<i32> = vec![];
        for i in 2..n {
            if Solution::is_prime(i, &primes) {
                //println!("is prime: {}", i);
                primes.push(i);
            }
        }
        primes.len() as i32
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_204() {
        assert_eq!(Solution::count_primes(5000000), 348513);
        assert_eq!(Solution::count_primes(10), 4);
        assert_eq!(Solution::count_primes(5), 2);
    }
}

```
