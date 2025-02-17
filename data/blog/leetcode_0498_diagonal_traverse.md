---
title: Leetcode 0498 diagonal traverse
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0498 diagonal traverse in Rust
---

## Solution in Rust

```rust
/**
 * [498] Diagonal Traverse
 *
 * Given an m x n matrix mat, return an array of all the elements of the array in a diagonal order.
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2021/04/10/diag1-grid.jpg" style="width: 334px; height: 334px;" />
 * Input: mat = [[1,2,3],[4,5,6],[7,8,9]]
 * Output: [1,2,4,7,5,3,6,8,9]
 *
 * Example 2:
 *
 * Input: mat = [[1,2],[3,4]]
 * Output: [1,2,3,4]
 *
 *
 * Constraints:
 *
 * 	m == mat.length
 * 	n == mat[i].length
 * 	1 <= m, n <= 10^4
 * 	1 <= m * n <= 10^4
 * 	-10^5 <= mat[i][j] <= 10^5
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/diagonal-traverse/
// discuss: https://leetcode.com/problems/diagonal-traverse/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn is_valid(coor: (i32, i32), m: usize, n: usize) -> bool {
        let i = coor.0;
        let j = coor.1;
        i >= 0 && j >=0 && i < m as i32 && j < n as i32
    }
    pub fn next(up: bool, coor: (i32, i32), m: usize, n: usize) -> (bool, (i32, i32)) {
        let mut next_coor = if up {
            (coor.0 - 1, coor.1 + 1)
        } else {
            (coor.0 + 1, coor.1 - 1)
        };
        if Self::is_valid(next_coor, m, n) {
            return (up, next_coor);
        }
        let i = next_coor.0;
        let j = next_coor.1;
        if i < 0 || j >= n as i32 {
            next_coor = (i, j + 1);
            //println!("next_coor: {:?}", next_coor);
            while !Self::is_valid(next_coor, m, n) {
                next_coor = (next_coor.0 + 1, next_coor.1 - 1);
            }
            return (false, next_coor)
        }
        next_coor = (i + 1, j);
        while !Self::is_valid(next_coor, m, n) {
            next_coor = (next_coor.0 - 1, next_coor.1 + 1);
        }
        return (true, next_coor);
    }
    pub fn find_diagonal_order(mat: Vec<Vec<i32>>) -> Vec<i32> {
        let m = mat.len();
        let n = mat[0].len();
        let mut up = true;
        let mut coor = (0i32, 0i32);
        let mut results: Vec<i32> = vec![];
        loop {
            //println!("coor: {:?}, up: {}", coor, up);
            results.push(mat[coor.0 as usize][coor.1 as usize]);
            //println!("results: {:?}", results);
            if results.len() == m * n {
                break;
            }
            let next = Self::next(up, coor, m, n);
            up = next.0;
            coor = next.1;
        }
        results
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_498() {
        assert_eq!(Solution::find_diagonal_order(vec![
            vec![1,2,3],vec![4,5,6],vec![7,8,9]]),
        vec![1,2,4,7,5,3,6,8,9]);
    }
}

```
