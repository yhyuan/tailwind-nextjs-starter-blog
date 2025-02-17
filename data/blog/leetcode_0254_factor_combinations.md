---
title: Leetcode 0254 factor combinations
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0254 factor combinations in Rust
---

## Solution in Rust

```rust
/**
 254. Factor Combinations
Medium

888

47

Add to List

Share
Numbers can be regarded as the product of their factors.

For example, 8 = 2 x 2 x 2 = 2 x 4.
Given an integer n, return all possible combinations of its factors. You may return the answer in any order.

Note that the factors should be in the range [2, n - 1].



Example 1:

Input: n = 1
Output: []
Example 2:

Input: n = 12
Output: [[2,6],[3,4],[2,2,3]]
Example 3:

Input: n = 37
Output: []


Constraints:

1 <= n <= 107
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/factor-combinations/
// discuss: https://leetcode.com/problems/factor-combinations/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
use std::collections::HashMap;
impl Solution {
    pub fn helper(n: usize, memo: &mut HashMap<usize, Vec<Vec<i32>>>) -> Vec<Vec<i32>> {
        if memo.contains_key(&n) {
            return memo[&n].clone();
        }
        let sqrt_n = (n as f64).sqrt() as usize;
        let mut res: Vec<Vec<i32>> = vec![];
        for i in 2..=sqrt_n {
            if n % i == 0 {
                //i, Self::get_factors(n / i)
                res.push(vec![i as i32 , (n / i) as i32]);
                let pre_res = Self::helper(n / i, memo);
                for j in 0..pre_res.len() {
                    if pre_res[j][0] >= i as i32 {
                        let mut row = pre_res[j].clone();
                        row.insert(0, i as i32);
                        res.push(row);
                    }
                }
            }
        }
        memo.insert(n, res.clone());
        res
    }
    pub fn get_factors(n: i32) -> Vec<Vec<i32>> {
        let n = n as usize;
        let mut memo: HashMap<usize, Vec<Vec<i32>>> = HashMap::new();
        Self::helper(n, &mut memo)
    }
}
// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_254() {
        assert_eq!(Solution::get_factors(1), vec![]);
        assert_eq!(Solution::get_factors(12), vec![vec![2,6],vec![3,4],vec![2,2,3]]);
        assert_eq!(Solution::get_factors(37), vec![]);
    }
}

```
