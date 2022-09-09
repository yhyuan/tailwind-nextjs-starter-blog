---
title: Leetcode 0313 super ugly number
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0313 super ugly number in Rust
---

## Solution in Rust

```rust
/**
 * [313] Super Ugly Number
 *
 * A super ugly number is a positive integer whose prime factors are in the array primes.
 * Given an integer n and an array of integers primes, return the n^th super ugly number.
 * The n^th super ugly number is guaranteed to fit in a 32-bit signed integer.
 *
 * Example 1:
 *
 * Input: n = 12, primes = [2,7,13,19]
 * Output: 32
 * Explanation: [1,2,4,7,8,13,14,16,19,26,28,32] is the sequence of the first 12 super ugly numbers given primes = [2,7,13,19].
 *
 * Example 2:
 *
 * Input: n = 1, primes = [2,3,5]
 * Output: 1
 * Explanation: 1 has no prime factors, therefore all of its prime factors are in the array primes = [2,3,5].
 *
 *
 * Constraints:
 *
 * 	1 <= n <= 10^6
 * 	1 <= primes.length <= 100
 * 	2 <= primes[i] <= 1000
 * 	primes[i] is guaranteed to be a prime number.
 * 	All the values of primes are unique and sorted in ascending order.
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/super-ugly-number/
// discuss: https://leetcode.com/problems/super-ugly-number/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
use std::collections::BinaryHeap;
use std::cmp::Reverse;
use std::collections::HashSet;
impl Solution {
    pub fn nth_super_ugly_number(n: i32, primes: Vec<i32>) -> i32 {
        let mut n = n;
        let mut queue: BinaryHeap<Reverse<i32>> = BinaryHeap::new();
        let mut visited: HashSet<i32> = HashSet::new();
        queue.push(Reverse(1));
        while n > 1 {
            let min = queue.pop().unwrap().0;
            for &x in primes.iter() {
                if let Some(y) = x.checked_mul(min) {
                    if visited.insert(y) {
                        queue.push(Reverse(y));
                    }
                }
            }
            n -= 1;
        }
        queue.pop().unwrap().0
        /*
        while results.len() < n as usize {
            let max_val = results[results.len() - 1];
            for &num in results.iter() {
                for &prime in primes.iter() {
                    let mul = num * prime;
                    if mul > max_val {
                        heap.push(-mul);
                    }
                }
            }
            let mut val = -heap.pop().unwrap();
            while val <= max_val {
                val = -heap.pop().unwrap();
            }
            results.push(val);
        }
        *results.last().unwrap()
        */
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_313() {
        assert_eq!(Solution::nth_super_ugly_number(12, vec![2,7,13,19]), 32);
        assert_eq!(Solution::nth_super_ugly_number(950, vec![2,11,17,19,23,29,31,41,53,59,67,73,79,89,101,127,137,139,149,163,167,179,181,191,197,223,239,241,251,263]), 12788);
    }
}

```
