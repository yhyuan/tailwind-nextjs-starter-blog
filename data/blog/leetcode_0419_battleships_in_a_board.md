---
title: Leetcode 0419 battleships in a board
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0419 battleships in a board in Rust
---

## Solution in Rust

```rust
/**
 * [419] Battleships in a Board
 *
 * Given an m x n matrix board where each cell is a battleship 'X' or empty '.', return the number of the battleships on board.
 * Battleships can only be placed horizontally or vertically on board. In other words, they can only be made of the shape 1 x k (1 row, k columns) or k x 1 (k rows, 1 column), where k can be of any size. At least one horizontal or vertical cell separates between two battleships (i.e., there are no adjacent battleships).
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2021/04/10/battelship-grid.jpg" style="width: 333px; height: 333px;" />
 * Input: board = [["X",".",".","X"],[".",".",".","X"],[".",".",".","X"]]
 * Output: 2
 *
 * Example 2:
 *
 * Input: board = [["."]]
 * Output: 0
 *
 *
 * Constraints:
 *
 * 	m == board.length
 * 	n == board[i].length
 * 	1 <= m, n <= 200
 * 	board[i][j] is either '.' or 'X'.
 *
 *
 * Follow up: Could you do it in one-pass, using only O(1) extra memory and without modifying the values board?
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/battleships-in-a-board/
// discuss: https://leetcode.com/problems/battleships-in-a-board/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn dfs(board: &Vec<Vec<char>>, i: usize, j: usize, visited: &mut Vec<Vec<bool>>) {
        let rows = board.len();
        let cols = board[0].len();
        visited[i][j] = true;
        let diffs = vec![(-1, 0), (1, 0), (0, -1), (0, 1)];
        let neighbors = diffs.iter()
            .map(|&(dx, dy)| (dx + i as i32, dy + j as i32))
            .filter(|&(x, y)| x >= 0 && y >= 0 && x < rows as i32 && y < cols as i32)
            .map(|(x, y)| (x as usize, y as usize))
            .filter(|&(x, y)| !visited[x][y])
            .collect::<Vec<_>>();
        for i in 0..neighbors.len() {
            let (next_i, next_j) = neighbors[i];
            Self::dfs(board, next_i, next_j, visited);
        }
    }
    pub fn count_battleships(board: Vec<Vec<char>>) -> i32 {
        let rows = board.len();
        let cols = board[0].len();
        let mut visited: Vec<Vec<bool>> = vec![vec![false; cols]; rows];
        for i in 0..rows {
            for j in 0..cols {
                if board[i][j] == '.' {
                    visited[i][j] = true;
                }
            }
        }
        let mut ans = 0;
        for i in 0..rows {
            for j in 0..cols {
                if visited[i][j] {
                    continue;
                }
                if board[i][j] == 'X' {
                    Self::dfs(&board, i, j, &mut visited);
                    ans += 1;
                }
            }
        }
        ans
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_419() {
        assert_eq!(Solution::count_battleships(vec![vec!['X','.','.','X'],vec!['.','.','.','X'],vec!['.','.','.','X']]), 2);
    }
}

```
