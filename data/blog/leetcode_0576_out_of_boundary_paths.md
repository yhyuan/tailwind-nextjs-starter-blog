---
title: Leetcode 0576 out of boundary paths
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0576 out of boundary paths in Rust
---

## Solution in Rust

```rust
/**
 * [576] Out of Boundary Paths
 *
 * There is an m x n grid with a ball. The ball is initially at the position [startRow, startColumn]. You are allowed to move the ball to one of the four adjacent cells in the grid (possibly out of the grid crossing the grid boundary). You can apply at most maxMove moves to the ball.
 * Given the five integers m, n, maxMove, startRow, startColumn, return the number of paths to move the ball out of the grid boundary. Since the answer can be very large, return it modulo 10^9 + 7.
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2021/04/28/out_of_boundary_paths_1.png" style="width: 500px; height: 296px;" />
 * Input: m = 2, n = 2, maxMove = 2, startRow = 0, startColumn = 0
 * Output: 6
 *
 * Example 2:
 * <img alt="" src="https://assets.leetcode.com/uploads/2021/04/28/out_of_boundary_paths_2.png" style="width: 500px; height: 293px;" />
 * Input: m = 1, n = 3, maxMove = 3, startRow = 0, startColumn = 1
 * Output: 12
 *
 *
 * Constraints:
 *
 * 	1 <= m, n <= 50
 * 	0 <= maxMove <= 50
 * 	0 <= startRow < m
 * 	0 <= startColumn < n
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/out-of-boundary-paths/
// discuss: https://leetcode.com/problems/out-of-boundary-paths/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
//https://zxi.mytechroad.com/blog/dynamic-programming/leetcode-576-out-of-boundary-paths/
impl Solution {
    pub fn find_paths(m: i32, n: i32, max_move: i32, start_row: i32, start_column: i32) -> i32 {
        let k_mod = 1000000007;
        let m = m as usize;
        let n = n as usize;
        let max_move = max_move as usize;
        let start_row = start_row as usize;
        let start_column = start_column as usize;
        let mut dp: Vec<Vec<Vec<i32>>> = vec![vec![vec![0; n]; m]; max_move + 1];
        let dirs = vec![-1, 0, 1, 0, -1];
        for k in 1..=max_move {
            for i in 0..m {
                for j in 0..n {
                    for d in 0..4 {
                        let x = i as i32 + dirs[d];
                        let y = j as i32 + dirs[d + 1];
                        if x < 0 || y < 0 || x >= m as i32 || y >= n as i32 {
                            dp[k][i][j] += 1;
                        } else {
                            dp[k][i][j] = (dp[k][i][j] + dp[k - 1][x as usize][y as usize]) % k_mod;
                        }
                    }
                }
            }
        }
        dp[max_move][start_row][start_column]
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_576() {
        assert_eq!(Solution::find_paths(2, 2, 2, 0, 0), 6);
        assert_eq!(Solution::find_paths(1, 3, 3, 0, 1), 12);
        assert_eq!(Solution::find_paths(10, 10, 0, 5, 5), 0);
    }
}

```
