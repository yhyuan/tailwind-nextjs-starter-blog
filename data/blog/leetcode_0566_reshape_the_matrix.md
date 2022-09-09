---
title: Leetcode 0566 reshape the matrix
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0566 reshape the matrix in Rust
---

## Solution in Rust

```rust
/**
 * [566] Reshape the Matrix
 *
 * In MATLAB, there is a handy function called reshape which can reshape an m x n matrix into a new one with a different size r x c keeping its original data.
 * You are given an m x n matrix mat and two integers r and c representing the number of rows and the number of columns of the wanted reshaped matrix.
 * The reshaped matrix should be filled with all the elements of the original matrix in the same row-traversing order as they were.
 * If the reshape operation with given parameters is possible and legal, output the new reshaped matrix; Otherwise, output the original matrix.
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2021/04/24/reshape1-grid.jpg" style="width: 613px; height: 173px;" />
 * Input: mat = [[1,2],[3,4]], r = 1, c = 4
 * Output: [[1,2,3,4]]
 *
 * Example 2:
 * <img alt="" src="https://assets.leetcode.com/uploads/2021/04/24/reshape2-grid.jpg" style="width: 453px; height: 173px;" />
 * Input: mat = [[1,2],[3,4]], r = 2, c = 4
 * Output: [[1,2],[3,4]]
 *
 *
 * Constraints:
 *
 * 	m == mat.length
 * 	n == mat[i].length
 * 	1 <= m, n <= 100
 * 	-1000 <= mat[i][j] <= 1000
 * 	1 <= r, c <= 300
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/reshape-the-matrix/
// discuss: https://leetcode.com/problems/reshape-the-matrix/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn matrix_reshape(mat: Vec<Vec<i32>>, r: i32, c: i32) -> Vec<Vec<i32>> {
        let r = r as usize;
        let c = c as usize;
        let rows = mat.len();
        let cols = mat[0].len();
        if r*c != rows*cols {
            return mat;
        }
        let mut result: Vec<Vec<i32>> = vec![vec![0; c]; r];
        let mut k = 0usize;
        for i in 0..rows {
            for j in 0..cols {
                let k = i * cols + j;
                result[k / c][ k % c] = mat[i][j];
            }
        }
        result
    }
}
// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_566() {
    }
}

```
