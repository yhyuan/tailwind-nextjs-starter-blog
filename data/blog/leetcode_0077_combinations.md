---
title: Leetcode 0077 combinations
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0077 combinations in Rust
---

## Solution in Rust

```rust
/**
 * [77] Combinations
 *
 * Given two integers n and k, return all possible combinations of k numbers out of the range [1, n].
 * You may return the answer in any order.
 *
 * Example 1:
 *
 * Input: n = 4, k = 2
 * Output:
 * [
 *   [2,4],
 *   [3,4],
 *   [2,3],
 *   [1,2],
 *   [1,3],
 *   [1,4],
 * ]
 *
 * Example 2:
 *
 * Input: n = 1, k = 1
 * Output: [[1]]
 *
 *
 * Constraints:
 *
 * 	1 <= n <= 20
 * 	1 <= k <= n
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/combinations/
// discuss: https://leetcode.com/problems/combinations/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
/*
impl Solution {
    pub fn combine_helper(n: i32, k: i32, i: i32) -> Vec<Vec<i32>> {
        if i > n {
            return vec![];
        }
        if k == 1 {
            return (i..=n).into_iter().map(|j| vec![j]).collect();
        }
        let mut results: Vec<Vec<i32>> = vec![];
        let mut single_results = Solution::combine_helper(n, k - 1, i + 1);
        for result in single_results.iter_mut() {
            result.insert(0, i);
        }
        for result in single_results {
            results.push(result);
        }
        let mut single_results = Solution::combine_helper(n, k, i + 1);
        for result in single_results {
            results.push(result);
        }
        results
    }
    pub fn combine(n: i32, k: i32) -> Vec<Vec<i32>> {
        Solution::combine_helper(n, k, 1)
    }
}
*/
impl Solution {
    pub fn helper(vect: &Vec<i32>, k: i32)  -> Vec<Vec<i32>> {
        if vect.len() == 0 {
            return vec![];
        }
        if k == 1 {
            return vect.iter().map(|&num| vec![num]).collect::<Vec<_>>();
        }
        let n = vect.len();
        let mut results: Vec<Vec<i32>> = vec![];
        for i in 0..n {
            let num = vect[i];
            let new_vect = &vect[i + 1..].to_vec();
            let mut result = Self::helper(new_vect, k - 1);
            for j in 0..result.len() {
                result[j].insert(0, num);
                results.push(result[j].clone());
            }
        }
        results
    }
    pub fn combine(n: i32, k: i32) -> Vec<Vec<i32>> {
        let vect = (1..=n).into_iter().collect::<Vec<_>>();
        Self::helper(&vect, k)
    }
}
// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_77() {
        assert_eq!(
            Solution::combine(4, 2),
            vec![
                vec![1, 2],
                vec![1, 3],
                vec![1, 4],
                vec![2, 3],
                vec![2, 4],
                vec![3, 4]
            ]
        );
        assert_eq!(Solution::combine(1, 1), vec![vec![1]]);
        let empty: Vec<Vec<i32>> = vec![];
        assert_eq!(Solution::combine(0, 1), empty);
        assert_eq!(Solution::combine(2, 1), vec![vec![1], vec![2]]);
    }
}

```
