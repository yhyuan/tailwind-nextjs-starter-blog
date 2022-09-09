---
title: Leetcode 0063 unique paths ii
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0063 unique paths ii in Rust
---

## Solution in Rust

```rust
/**
 * [63] Unique Paths II
 *
 * A robot is located at the top-left corner of a m x n grid (marked 'Start' in the diagram below).
 * The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).
 * Now consider if some obstacles are added to the grids. How many unique paths would there be?
 * An obstacle and space is marked as 1 and 0 respectively in the grid.
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2020/11/04/robot1.jpg" style="width: 242px; height: 242px;" />
 * Input: obstacleGrid = [[0,0,0],[0,1,0],[0,0,0]]
 * Output: 2
 * Explanation: There is one obstacle in the middle of the 3x3 grid above.
 * There are two ways to reach the bottom-right corner:
 * 1. Right -> Right -> Down -> Down
 * 2. Down -> Down -> Right -> Right
 *
 * Example 2:
 * <img alt="" src="https://assets.leetcode.com/uploads/2020/11/04/robot2.jpg" style="width: 162px; height: 162px;" />
 * Input: obstacleGrid = [[0,1],[0,0]]
 * Output: 1
 *
 *
 * Constraints:
 *
 * 	m == obstacleGrid.length
 * 	n == obstacleGrid[i].length
 * 	1 <= m, n <= 100
 * 	obstacleGrid[i][j] is 0 or 1.
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/unique-paths-ii/
// discuss: https://leetcode.com/problems/unique-paths-ii/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn unique_paths_with_obstacles(obstacle_grid: Vec<Vec<i32>>) -> i32 {
        let m = obstacle_grid.len();
        let n = obstacle_grid[0].len();
        let mut table: Vec<Vec<i32>> = vec![vec![0; n]; m];
        let mut found = false;
        for i in 0..m {
            if obstacle_grid[i][0] == 1 {
                found = true;
            }
            table[i][0] = if found {0} else {1};
        }
        found = false;
        for i in 0..n {
            if obstacle_grid[0][i] == 1 {
                found = true;
            }
            table[0][i] = if found {0} else {1};
        }
        for i in 1..m {
            for j in 1..n {
                let v1 = if obstacle_grid[i - 1][j] == 1 {0} else {table[i - 1][j]};
                let v2 = if obstacle_grid[i][j - 1] == 1 {0} else {table[i][j - 1]};
                table[i][j] = v1 + v2;
            }
        }
        if obstacle_grid[m - 1][n - 1] == 1 {0} else {table[m - 1][n - 1]}
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_63() {
        assert_eq!(Solution::unique_paths_with_obstacles(vec![vec![0]]), 1);
        assert_eq!(
            Solution::unique_paths_with_obstacles(vec![vec![0, 0], vec![0, 0],]),
            2
        );
        assert_eq!(
            Solution::unique_paths_with_obstacles(vec![vec![0, 1], vec![1, 0],]),
            0
        );
        assert_eq!(
            Solution::unique_paths_with_obstacles(vec![
                vec![0, 0, 0],
                vec![0, 1, 0],
                vec![0, 0, 0],
            ]),
            2
        );
        assert_eq!(
            Solution::unique_paths_with_obstacles(vec![
                vec![0, 0, 0, 0],
                vec![0, 0, 0, 0],
                vec![0, 0, 0, 0],
            ]),
            10
        );
        assert_eq!(
            Solution::unique_paths_with_obstacles(vec![
                vec![0, 0, 0, 0],
                vec![0, 0, 0, 1],
                vec![0, 0, 1, 0],
            ]),
            0
        );
    }
}

```
