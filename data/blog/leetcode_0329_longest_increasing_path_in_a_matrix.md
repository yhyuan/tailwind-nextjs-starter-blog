---
title: Leetcode 0329 longest increasing path in a matrix
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0329 longest increasing path in a matrix in Rust
---

## Solution in Rust

```rust
/**
 * [329] Longest Increasing Path in a Matrix
 *
 * Given an m x n integers matrix, return the length of the longest increasing path in matrix.
 * From each cell, you can either move in four directions: left, right, up, or down. You may not move diagonally or move outside the boundary (i.e., wrap-around is not allowed).
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2021/01/05/grid1.jpg" style="width: 242px; height: 242px;" />
 * Input: matrix = [[9,9,4],[6,6,8],[2,1,1]]
 * Output: 4
 * Explanation: The longest increasing path is [1, 2, 6, 9].
 *
 * Example 2:
 * <img alt="" src="https://assets.leetcode.com/uploads/2021/01/27/tmp-grid.jpg" style="width: 253px; height: 253px;" />
 * Input: matrix = [[3,4,5],[3,2,6],[2,2,1]]
 * Output: 4
 * Explanation: The longest increasing path is [3, 4, 5, 6]. Moving diagonally is not allowed.
 *
 * Example 3:
 *
 * Input: matrix = [[1]]
 * Output: 1
 *
 *
 * Constraints:
 *
 * 	m == matrix.length
 * 	n == matrix[i].length
 * 	1 <= m, n <= 200
 * 	0 <= matrix[i][j] <= 2^31 - 1
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/longest-increasing-path-in-a-matrix/
// discuss: https://leetcode.com/problems/longest-increasing-path-in-a-matrix/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn helper(matrix: &Vec<Vec<i32>>, dp: &mut Vec<Vec<i32>>, i: usize, j: usize) {
        let m = matrix.len();
        let n = matrix[0].len();
        let neighbors = vec![(-1, 0), (1, 0), (0, -1), (0, 1)].into_iter()
            .map(|(dx, dy)| (i as i32 + dx, j as i32 + dy))
            .filter(|&(x, y)| x >= 0 && y >= 0 && x < m as i32 && y < n as i32)
            .map(|(x, y)| (x as usize, y as usize))
            .filter(|&(x, y)| matrix[x][y] > matrix[i][j])
            .collect::<Vec<_>>();
        let mut res = 1i32;
        for k in 0..neighbors.len() {
            let (x, y) = neighbors[k];
            if dp[x][y] < 0 {
                Self::helper(matrix, dp, x, y);
            }
            res = i32::max(res, dp[x][y] + 1);
        }
        dp[i][j] = res;
    }
    pub fn longest_increasing_path(matrix: Vec<Vec<i32>>) -> i32 {
        let m = matrix.len();
        let n = matrix[0].len();
        let mut dp: Vec<Vec<i32>> = vec![vec![-1; n]; m];
        for i in 0..m {
            for j in 0..n {
                if dp[i][j] < 0 {
                    Self::helper(&matrix, &mut dp, i, j);
                }
            }
        }
        let mut res = i32::MIN;
        for i in 0..m {
            for j in 0..n {
                res = i32::max(res, dp[i][j]);
            }
        }
        res
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_329() {
        assert_eq!(Solution::longest_increasing_path(vec![vec![9,9,4],vec![6,6,8],vec![2,1,1]]), 4);
        assert_eq!(Solution::longest_increasing_path(vec![vec![3,4,5],vec![3,2,6],vec![2,2,1]]), 4);
    }
}

```
