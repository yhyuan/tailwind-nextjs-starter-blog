---
title: Leetcode 0562 longest line of consecutive one in matrix
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0562 longest line of consecutive one in matrix in Rust
---

## Solution in Rust

```rust
/**
 562. Longest Line of Consecutive One in Matrix
Medium

694

97

Add to List

Share
Given an m x n binary matrix mat, return the length of the longest line of consecutive one in the matrix.

The line could be horizontal, vertical, diagonal, or anti-diagonal.



Example 1:


Input: mat = [[0,1,1,0],[0,1,1,0],[0,0,0,1]]
Output: 3
Example 2:


Input: mat = [[1,1,1,1],[0,1,1,0],[0,0,0,1]]
Output: 4


Constraints:

m == mat.length
n == mat[i].length
1 <= m, n <= 104
1 <= m * n <= 104
mat[i][j] is either 0 or 1.
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/longest-line-of-consecutive-one-in-matrix/
// discuss: https://leetcode.com/problems/longest-line-of-consecutive-one-in-matrix/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
impl Solution {
    pub fn longest_line(mat: Vec<Vec<i32>>) -> i32 {
        let m = mat.len();
        let n = mat[0].len();
        let mut res = i32::MIN;
        // Horizontal
        let mut dp: Vec<Vec<i32>> = vec![vec![0; n]; m];
        for i in 0..m {
            for j in 0..n {
                if j == 0 {
                    dp[i][j] = if mat[i][j] == 1 {1} else {0};
                } else {
                    dp[i][j] = if mat[i][j] == 1 {dp[i][j - 1] + 1} else {0};
                }
                res = i32::max(res, dp[i][j]);
            }
        }
        // Vertical
        let mut dp: Vec<Vec<i32>> = vec![vec![0; n]; m];
        for i in 0..m {
            for j in 0..n {
                if i == 0 {
                    dp[i][j] = if mat[i][j] == 1 {1} else {0};
                } else {
                    dp[i][j] = if mat[i][j] == 1 {dp[i - 1][j] + 1} else {0};
                }
                res = i32::max(res, dp[i][j]);
            }
        }

        // Diagonal
        let mut dp: Vec<Vec<i32>> = vec![vec![0; n]; m];
        for i in 0..m {
            for j in 0..n {
                if i == 0 || j == 0 {
                    dp[i][j] = if mat[i][j] == 1 {1} else {0};
                } else {
                    dp[i][j] = if mat[i][j] == 1 {dp[i - 1][j - 1] + 1} else {0};
                }
                res = i32::max(res, dp[i][j]);
            }
        }

        // Anti-Diagonal
        let mut dp: Vec<Vec<i32>> = vec![vec![0; n]; m];
        for i in 0..m {
            for j in (0..n).rev() {
                if i == 0 || j == n - 1 {
                    dp[i][j] = if mat[i][j] == 1 {1} else {0};
                } else {
                    dp[i][j] = if mat[i][j] == 1 {dp[i - 1][j + 1] + 1} else {0};
                }
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
    fn test_562() {
        assert_eq!(Solution::longest_line(vec![vec![0,1,1,0],vec![0,1,1,0],vec![0,0,0,1]]), 3);
        assert_eq!(Solution::longest_line(vec![vec![1,1,1,1],vec![0,1,1,0],vec![0,0,0,1]]), 4);
    }
}

```
