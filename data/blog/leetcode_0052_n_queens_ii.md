---
title: Leetcode 0052 n queens ii
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0052 n queens ii in Rust
---

## Solution in Rust

```rust
/**
 * [52] N-Queens II
 *
 * The n-queens puzzle is the problem of placing n queens on an n x n chessboard such that no two queens attack each other.
 * Given an integer n, return the number of distinct solutions to the n-queens puzzle.
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2020/11/13/queens.jpg" style="width: 600px; height: 268px;" />
 * Input: n = 4
 * Output: 2
 * Explanation: There are two distinct solutions to the 4-queens puzzle as shown.
 *
 * Example 2:
 *
 * Input: n = 1
 * Output: 1
 *
 *
 * Constraints:
 *
 * 	1 <= n <= 9
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/n-queens-ii/
// discuss: https://leetcode.com/problems/n-queens-ii/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
use std::collections::HashMap;
impl Solution {
    pub fn total_n_queens_helper(positions: &mut Vec<usize>) -> i32 {
        let n = positions.len();
        let new_x = positions.iter().position(|&y| y == usize::MAX);
        if new_x.is_none() {
            return 1i32;
        }
        let new_x = new_x.unwrap();
        let mut unsafe_coors: HashMap<usize, usize> = HashMap::with_capacity(n);
        for (x, &y) in positions.iter().enumerate() {
            if y != usize::MAX {
                unsafe_coors.insert(y, x);
                if y + new_x >= x {
                    unsafe_coors.insert(y + new_x - x, x);
                }
                if y + x >= new_x {
                    unsafe_coors.insert(y + x - new_x, x);
                }
            }
        }
        let new_ys: Vec<usize> = (0usize..n).into_iter().filter(|y| !unsafe_coors.contains_key(y)).collect();
        //let mut results: Vec<Vec<usize>> = vec![];
        let mut total = 0i32;
        for &new_y in new_ys.iter() {
            positions[new_x] = new_y;
            total += Solution::total_n_queens_helper(positions);
        }
        positions[new_x] = usize::MAX;
        total
    }

    pub fn total_n_queens(n: i32) -> i32 {
        let n = n as usize;
        let mut positions: Vec<usize> = vec![usize::MAX; n]; // (index, positions[index]) is a queen position.         //let mut board: Vec<Vec<bool>> = vec![vec![false; n]; n];
        Solution::total_n_queens_helper(&mut positions)
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_52() {
        assert_eq!(Solution::total_n_queens(4), 2);
        assert_eq!(Solution::total_n_queens(8), 92);
        assert_eq!(Solution::total_n_queens(13), 73712);
    }
}

```
