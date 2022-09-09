---
title: Leetcode 1631 path with minimum effort
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 1631 path with minimum effort in Rust
---

## Solution in Rust

```rust
/**
 * [1631] Path With Minimum Effort
 *
 * You are a hiker preparing for an upcoming hike. You are given heights, a 2D array of size rows x columns, where heights[row][col] represents the height of cell (row, col). You are situated in the top-left cell, (0, 0), and you hope to travel to the bottom-right cell, (rows-1, columns-1) (i.e., 0-indexed). You can move up, down, left, or right, and you wish to find a route that requires the minimum effort.
 *
 * A route's effort is the maximum absolute difference in heights between two consecutive cells of the route.
 *
 * Return the minimum effort required to travel from the top-left cell to the bottom-right cell.
 *
 *
 * Example 1:
 *
 * <img alt="" src="https://assets.leetcode.com/uploads/2020/10/04/ex1.png" style="width: 300px; height: 300px;" />
 *
 *
 * Input: heights = [[1,2,2],[3,8,2],[5,3,5]]
 * Output: 2
 * Explanation: The route of [1,3,5,3,5] has a maximum absolute difference of 2 in consecutive cells.
 * This is better than the route of [1,2,2,2,5], where the maximum absolute difference is 3.
 *
 *
 * Example 2:
 *
 * <img alt="" src="https://assets.leetcode.com/uploads/2020/10/04/ex2.png" style="width: 300px; height: 300px;" />
 *
 *
 * Input: heights = [[1,2,3],[3,8,4],[5,3,5]]
 * Output: 1
 * Explanation: The route of [1,2,3,4,5] has a maximum absolute difference of 1 in consecutive cells, which is better than route [1,3,5,3,5].
 *
 *
 * Example 3:
 * <img alt="" src="https://assets.leetcode.com/uploads/2020/10/04/ex3.png" style="width: 300px; height: 300px;" />
 *
 * Input: heights = [[1,2,1,1,1],[1,2,1,2,1],[1,2,1,2,1],[1,2,1,2,1],[1,1,1,2,1]]
 * Output: 0
 * Explanation: This route does not require any effort.
 *
 *
 *
 * Constraints:
 *
 *
 * 	rows == heights.length
 * 	columns == heights[i].length
 * 	1 <= rows, columns <= 100
 * 	1 <= heights[i][j] <= 10^6
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/path-with-minimum-effort/
// discuss: https://leetcode.com/problems/path-with-minimum-effort/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

use std::collections::BinaryHeap;
impl Solution {
    pub fn minimum_effort_path(heights: Vec<Vec<i32>>) -> i32 {
        let rows = heights.len();
        let cols = heights[0].len();
        let mut heap: BinaryHeap<(i32, usize, usize)> = BinaryHeap::new();
        heap.push((0, 0, 0));
        let mut visited: Vec<Vec<bool>> = vec![vec![false; cols]; rows];
        let mut result = i32::MAX;
        while !heap.is_empty() {
            let (dif, x, y) = heap.pop().unwrap();
            //println!("dif: {}, x: {}, y: {}", dif, x, y);
            result = i32::min(result, dif);
            visited[x][y] = true;
            if x == rows - 1 && y == cols - 1 {
                break;
            }
            let neighbors = vec![(-1, 0), (1, 0), (0, -1), (0, 1)].iter()
                .map(|&(dx, dy)| (x as i32 + dx, y as i32 + dy))
                .filter(|&(x, y)| x >= 0 && y >= 0 && x < rows as i32 && y < cols as i32 && !visited[x as usize][y as usize])
                .collect::<Vec<_>>();
            for i in 0..neighbors.len() {
                let (n_x, n_y) = neighbors[i];
                let n_x = n_x as usize;
                let n_y = n_y as usize;
                let diff = if heights[x][y] > heights[n_x][n_y] {heights[n_x][n_y] - heights[x][y]} else {heights[x][y] - heights[n_x][n_y]};
                heap.push((diff, n_x, n_y));
            }
        }
        if result == i32::MAX {0} else {-result}
    }
}
// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_1631() {
    }
}

```
