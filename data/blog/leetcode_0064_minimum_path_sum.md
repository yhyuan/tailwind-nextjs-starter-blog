---
title: Leetcode 0064 minimum path sum
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0064 minimum path sum in Rust
---

## Solution in Rust

```rust
/**
 * [64] Minimum Path Sum
 *
 * Given a m x n grid filled with non-negative numbers, find a path from top left to bottom right, which minimizes the sum of all numbers along its path.
 * Note: You can only move either down or right at any point in time.
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2020/11/05/minpath.jpg" style="width: 242px; height: 242px;" />
 * Input: grid = [[1,3,1],[1,5,1],[4,2,1]]
 * Output: 7
 * Explanation: Because the path 1 &rarr; 3 &rarr; 1 &rarr; 1 &rarr; 1 minimizes the sum.
 *
 * Example 2:
 *
 * Input: grid = [[1,2,3],[4,5,6]]
 * Output: 12
 *
 *
 * Constraints:
 *
 * 	m == grid.length
 * 	n == grid[i].length
 * 	1 <= m, n <= 200
 * 	0 <= grid[i][j] <= 100
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/minimum-path-sum/
// discuss: https://leetcode.com/problems/minimum-path-sum/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn min_path_sum(grid: Vec<Vec<i32>>) -> i32 {
        if grid.len() <= 0 {
            panic!("Wrong input!");
        }
        let rows = grid.len();
        let columns = grid[0].len();
        let mut state: Vec<Vec<i32>> = vec![vec![0i32; columns]; rows];
        let mut total = 0;
        for i in 0..columns {
            total += grid[0][i];
            state[0][i] = total;
        }
        let mut total = 0i32;
        for j in 0..rows {
            total += grid[j][0];
            state[j][0] = total;
        }
        for i in 1..rows {
            for j in 1..columns {
                state[i][j] = i32::min(state[i-1][j], state[i][j-1]) + grid[i][j];
            }
        }
        state[rows - 1][columns - 1]
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_64() {
        assert_eq!(Solution::min_path_sum(vec![vec![2]]), 2);
        assert_eq!(
            Solution::min_path_sum(vec![vec![1, 3, 1], vec![1, 5, 1], vec![4, 2, 1],]),
            7
        );
        assert_eq!(Solution::min_path_sum(vec![vec![1, 3, 1],]), 5);
    }
}

```
