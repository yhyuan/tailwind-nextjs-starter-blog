---
title: Leetcode 0073 set matrix zeroes
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0073 set matrix zeroes in Rust
---

## Solution in Rust

```rust
/**
 * [73] Set Matrix Zeroes
 *
 * Given an m x n integer matrix matrix, if an element is 0, set its entire row and column to 0's, and return the matrix.
 * You must do it <a href="https://en.wikipedia.org/wiki/In-place_algorithm" target="_blank">in place</a>.
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2020/08/17/mat1.jpg" style="width: 450px; height: 169px;" />
 * Input: matrix = [[1,1,1],[1,0,1],[1,1,1]]
 * Output: [[1,0,1],[0,0,0],[1,0,1]]
 *
 * Example 2:
 * <img alt="" src="https://assets.leetcode.com/uploads/2020/08/17/mat2.jpg" style="width: 450px; height: 137px;" />
 * Input: matrix = [[0,1,2,0],[3,4,5,2],[1,3,1,5]]
 * Output: [[0,0,0,0],[0,4,5,0],[0,3,1,0]]
 *
 *
 * Constraints:
 *
 * 	m == matrix.length
 * 	n == matrix[0].length
 * 	1 <= m, n <= 200
 * 	-2^31 <= matrix[i][j] <= 2^31 - 1
 *
 *
 * Follow up:
 *
 * 	A straightforward solution using O(mn) space is probably a bad idea.
 * 	A simple improvement uses O(m + n) space, but still not the best solution.
 * 	Could you devise a constant space solution?
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/set-matrix-zeroes/
// discuss: https://leetcode.com/problems/set-matrix-zeroes/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn set_zeroes(matrix: &mut Vec<Vec<i32>>) {
        let m = matrix.len();
        let n = matrix[0].len();
        let zero_rows: Vec<usize> = (0..m).into_iter()
            .filter(|&i| matrix[i].contains(&0))
            .collect();
        let zero_cols: Vec<usize> = (0..n).into_iter()
            .filter(|&j| (0..m).into_iter().map(|i| matrix[i][j]).collect::<Vec<i32>>().contains(&0))
            .collect();
        for &i in zero_rows.iter() {
            matrix[i] = vec![0; n];
        }
        for &j in zero_cols.iter() {
            for i in 0..m {
                matrix[i][j] = 0;
            }
        }
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_73() {
        let mut matrix = vec![vec![1,1,1],vec![1,0,1],vec![1,1,1]];
        Solution::set_zeroes(&mut matrix);
        assert_eq!(matrix, vec![vec![1,0,1],vec![0,0,0],vec![1,0,1]]);

        let mut matrix = vec![vec![0,1,2,0],vec![3,4,5,2],vec![1,3,1,5]];
        Solution::set_zeroes(&mut matrix);
        assert_eq!(matrix, vec![vec![0,0,0,0],vec![0,4,5,0],vec![0,3,1,0]]);

    }
}

```
