---
title: Leetcode 1254 number of closed islands
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 1254 number of closed islands in Rust
---

## Solution in Rust

```rust
/**
 * [1254] Number of Closed Islands
 *
 * Given a 2D grid consists of 0s (land) and 1s (water).  An island is a maximal 4-directionally connected group of <font face="monospace">0</font>s and a closed island is an island totally (all left, top, right, bottom) surrounded by 1s.
 * Return the number of closed islands.
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2019/10/31/sample_3_1610.png" style="width: 240px; height: 120px;" />
 *
 * Input: grid = [[1,1,1,1,1,1,1,0],[1,0,0,0,0,1,1,0],[1,0,1,0,1,1,1,0],[1,0,0,0,0,1,0,1],[1,1,1,1,1,1,1,0]]
 * Output: 2
 * Explanation:
 * Islands in gray are closed because they are completely surrounded by water (group of 1s).
 * Example 2:
 * <img alt="" src="https://assets.leetcode.com/uploads/2019/10/31/sample_4_1610.png" style="width: 160px; height: 80px;" />
 *
 * Input: grid = [[0,0,1,0,0],[0,1,0,1,0],[0,1,1,1,0]]
 * Output: 1
 *
 * Example 3:
 *
 * Input: grid = [[1,1,1,1,1,1,1],
 *                [1,0,0,0,0,0,1],
 *                [1,0,1,1,1,0,1],
 *                [1,0,1,0,1,0,1],
 *                [1,0,1,1,1,0,1],
 *                [1,0,0,0,0,0,1],
 *                [1,1,1,1,1,1,1]]
 * Output: 2
 *
 *
 * Constraints:
 *
 * 	1 <= grid.length, grid[0].length <= 100
 * 	0 <= grid[i][j] <=1
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/number-of-closed-islands/
// discuss: https://leetcode.com/problems/number-of-closed-islands/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn helper(grid: &Vec<Vec<i32>>, visited: &mut Vec<Vec<bool>>, is_good: &mut bool, row: usize, col: usize) {
        let rows = grid.len();
        let cols = grid[0].len();
        visited[row][col] = true;
        if row == 0 || col == 0 || row == rows - 1 || col == cols - 1 {
            if *is_good {
                *is_good = false;
            }
        }
        let neighbors = vec![(-1, 0), (1, 0), (0, -1), (0, 1)].iter()
            .map(|&(dx, dy)| (dx + row as i32, dy + col as i32))
            .filter(|&(x, y)| x >= 0 && y >= 0 && x < rows as i32 && y < cols as i32 && !visited[x as usize][y as usize] && grid[x as usize][y as usize] == 0)
            .collect::<Vec<_>>();
        //println!("neighbors: {:?}", neighbors);
        for i in 0..neighbors.len() {
            let (x, y) = neighbors[i];
            Self::helper(grid, visited, is_good, x as usize, y as usize);
        }
    }
    pub fn closed_island(grid: Vec<Vec<i32>>) -> i32 {
        let rows = grid.len();
        let cols = grid[0].len();
        let mut visited = vec![vec![false; cols]; rows];
        let mut count = 0;
        for i in 0..rows {
            for j in 0..cols {
                if grid[i][j] == 1 {
                    continue;
                }
                if visited[i][j] {
                    continue;
                }
                let mut is_good = true;
                // println!("i: {}, j: {}", i, j);
                Self::helper(&grid, &mut visited, &mut is_good, i, j);
                if is_good {
                    count += 1;
                }
            }
        }
        count
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_1254() {
        assert_eq!(Solution::closed_island(vec![
            vec![1,1,1,1,1,1,1,0],
            vec![1,0,0,0,0,1,1,0],
            vec![1,0,1,0,1,1,1,0],
            vec![1,0,0,0,0,1,0,1],
            vec![1,1,1,1,1,1,1,0]]), 2);
    }
}

```
