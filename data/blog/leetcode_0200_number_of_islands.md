---
title: Leetcode 0200 number of islands
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0200 number of islands in Rust
---

## Solution in Rust

```rust
/**
 * [200] Number of Islands
 *
 * Given an m x n 2D binary grid grid which represents a map of '1's (land) and '0's (water), return the number of islands.
 * An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.
 *
 * Example 1:
 *
 * Input: grid = [
 *   ["1","1","1","1","0"],
 *   ["1","1","0","1","0"],
 *   ["1","1","0","0","0"],
 *   ["0","0","0","0","0"]
 * ]
 * Output: 1
 *
 * Example 2:
 *
 * Input: grid = [
 *   ["1","1","0","0","0"],
 *   ["1","1","0","0","0"],
 *   ["0","0","1","0","0"],
 *   ["0","0","0","1","1"]
 * ]
 * Output: 3
 *
 *
 * Constraints:
 *
 * 	m == grid.length
 * 	n == grid[i].length
 * 	1 <= m, n <= 300
 * 	grid[i][j] is '0' or '1'.
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/number-of-islands/
// discuss: https://leetcode.com/problems/number-of-islands/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
use std::collections::HashMap;
impl Solution {
    pub fn find_next_coor(coordinates: &HashMap<(usize, usize), usize>) -> Option<(usize, usize)> {
        if coordinates.len() == 0 {
            return  None;
        }
        for (&k, v) in coordinates.iter() {
            return Some(k);
        }
        None
    }
    pub fn explore_position(coordinates: &mut HashMap<(usize, usize), usize>, rows: usize, cols: usize, i: usize, j: usize) {
        coordinates.remove_entry(&(i, j));
        let i = i as i32;
        let j = j as i32;
        let rows = rows as i32;
        let cols = cols as i32;
        let options = vec![(i - 1, j), (i + 1, j), (i, j - 1), (i, j + 1)];

        let options: Vec<(usize, usize)> = options.iter()
            .filter(|&coor| coor.0 >= 0 && coor.0 < rows && coor.1 >= 0 && coor.1 < cols)
            .map(|coor|(coor.0 as usize, coor.1 as usize))
            .filter(|coor| coordinates.contains_key(coor))
            .collect();
        for option in options.iter() {
            Solution::explore_position(coordinates, rows as usize, cols as usize, option.0, option.1);
        }
    }
    pub fn num_islands(grid: Vec<Vec<char>>) -> i32 {
        let rows = grid.len();
        let cols = grid[0].len();
        let mut coordinates: HashMap<(usize, usize), usize> = HashMap::new();
        //let mut searched: Vec<Vec<bool>> = vec![vec![false; cols]; rows];
        for i in 0..rows {
            for j in 0..cols {
                if grid[i][j] == '1' {
                    //searched[i][j] = true;
                    coordinates.insert((i, j), 1);
                }
            }
        }
        println!("coordinates: {:?}", coordinates.keys().collect::<Vec<&(usize, usize)>>());
        let mut num_islands = 0;
        while let Some(coor) = Solution::find_next_coor(&coordinates) {
            let (i, j) = coor;
            Solution::explore_position(&mut coordinates, rows, cols, i, j);
            num_islands += 1;
        }
        num_islands
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_200() {

        assert_eq!(
            Solution::num_islands(vec![
                vec!['1', '1', '1', '1', '0',],
                vec!['1', '1', '0', '1', '0',],
                vec!['1', '1', '0', '0', '0',],
                vec!['0', '0', '0', '0', '0',],
            ]),
            1
        );

        assert_eq!(
            Solution::num_islands(vec![
                vec!['1', '1', '0', '1', '0',],
                vec!['1', '1', '0', '1', '0',],
                vec!['1', '1', '0', '0', '0',],
                vec!['0', '0', '0', '1', '1',],
            ]),
            3
        );

    }
}

```
