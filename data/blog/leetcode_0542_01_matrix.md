---
title: Leetcode 0542 01 matrix
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0542 01 matrix in Rust
---

## Solution in Rust

```rust
/**
 * [542] 01 Matrix
 *
 * Given an m x n binary matrix mat, return the distance of the nearest 0 for each cell.
 * The distance between two adjacent cells is 1.
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2021/04/24/01-1-grid.jpg" style="width: 253px; height: 253px;" />
 * Input: mat = [[0,0,0],[0,1,0],[0,0,0]]
 * Output: [[0,0,0],[0,1,0],[0,0,0]]
 *
 * Example 2:
 * <img alt="" src="https://assets.leetcode.com/uploads/2021/04/24/01-2-grid.jpg" style="width: 253px; height: 253px;" />
 * Input: mat = [[0,0,0],[0,1,0],[1,1,1]]
 * Output: [[0,0,0],[0,1,0],[1,2,1]]
 *
 *
 * Constraints:
 *
 * 	m == mat.length
 * 	n == mat[i].length
 * 	1 <= m, n <= 10^4
 * 	1 <= m * n <= 10^4
 * 	mat[i][j] is either 0 or 1.
 * 	There is at least one 0 in mat.
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/01-matrix/
// discuss: https://leetcode.com/problems/01-matrix/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
use std::collections::VecDeque;
impl Solution {
    pub fn update_matrix(mat: Vec<Vec<i32>>) -> Vec<Vec<i32>> {
        let rows = mat.len();
        let cols = mat[0].len();
        let mut q: VecDeque<(usize, usize, usize)> = VecDeque::new();
        let mut dp: Vec<Vec<i32>> = vec![vec![i32::MAX; cols]; rows];
        for i in 0..rows {
            for j in 0..cols {
                if mat[i][j] == 0 {
                    dp[i][j] = 0;
                    q.push_back((i, j, 0));
                }
            }
        }
        let mut visited: Vec<Vec<bool>> = vec![vec![false; cols]; rows];
        while !q.is_empty() {
            let (i, j, level) = q.pop_front().unwrap();
            visited[i][j] = true;
            dp[i][j] = i32::min(dp[i][j], level as i32);
            let diffs = vec![(-1, 0), (1, 0), (0, -1), (0, 1)];
            for k in 0..diffs.len() {
                let (dx, dy) = diffs[k];
                let x = dx + i as i32;
                let y = dy + j as i32;
                if x < 0 || x >= rows as i32 || y < 0 || y >= cols as i32 {
                    continue;
                }
                let x = x as usize;
                let y = y as usize;
                if mat[x][y] == 0 || visited[x][y] {
                    continue;
                }
                q.push_back((x, y, level + 1));
            }
        }
        // println!("dp: {:?}", dp);
        dp
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_542() {
        assert_eq!(Solution::update_matrix(vec![vec![0,0,0],vec![0,1,0],vec![0,0,0]]), vec![vec![0,0,0],vec![0,1,0],vec![0,0,0]]);
        assert_eq!(Solution::update_matrix(vec![vec![0,0,0],vec![0,1,0],vec![1,1,1]]), vec![vec![0,0,0],vec![0,1,0],vec![1,2,1]]);
    }
}

```
