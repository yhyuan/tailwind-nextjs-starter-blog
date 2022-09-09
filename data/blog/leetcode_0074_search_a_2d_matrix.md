---
title: Leetcode 0074 search a 2d matrix
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0074 search a 2d matrix in Rust
---

## Solution in Rust

```rust
/**
 * [74] Search a 2D Matrix
 *
 * Write an efficient algorithm that searches for a value in an m x n matrix. This matrix has the following properties:
 *
 * 	Integers in each row are sorted from left to right.
 * 	The first integer of each row is greater than the last integer of the previous row.
 *
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2020/10/05/mat.jpg" style="width: 322px; height: 242px;" />
 * Input: matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 3
 * Output: true
 *
 * Example 2:
 * <img alt="" src="https://assets.leetcode.com/uploads/2020/10/05/mat2.jpg" style="width: 322px; height: 242px;" />
 * Input: matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 13
 * Output: false
 *
 *
 * Constraints:
 *
 * 	m == matrix.length
 * 	n == matrix[i].length
 * 	1 <= m, n <= 100
 * 	-10^4 <= matrix[i][j], target <= 10^4
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/search-a-2d-matrix/
// discuss: https://leetcode.com/problems/search-a-2d-matrix/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    /*
    pub fn search_matrix_helper(matrix: &Vec<Vec<i32>>, target: i32, start: usize, end: usize) -> bool {
        //let rows = matrix.len();
        let columns = matrix[0].len();
        if start == end {
            let i = start / columns;
            let j = start % columns;
            return matrix[i][j] == target;
        }
        if (start + 1) == end {
            let i1 = start / columns;
            let j1 = start % columns;
            let i2 = end / columns;
            let j2 = end % columns;
            return matrix[i1][j1] == target || matrix[i2][j2] == target ;
        }
        let middle = (start + end) / 2;
        let i = middle / columns;
        let j = middle % columns;

        if matrix[i][j] == target {
            true
        } else if matrix[i][j] < target {
            Solution::search_matrix_helper(matrix, target, middle, end)
        } else {
            Solution::search_matrix_helper(matrix, target, start, middle)
        }
    }

    pub fn search_matrix(matrix: Vec<Vec<i32>>, target: i32) -> bool {
        if matrix.len() == 0 {
            return false;
        }
        Solution::search_matrix_helper(&matrix, target, 0, matrix.len() * matrix[0].len() - 1)
    }
    */
    pub fn search_matrix(matrix: Vec<Vec<i32>>, target: i32) -> bool {
        if matrix.len() == 0 {
            return false;
        }
        let rows = matrix.len();
        let cols = matrix[0].len();
        let mut low = 0usize;
        let mut high = rows * cols - 1;
        if target < matrix[0][0] {
            return false;
        }
        if target > matrix[rows - 1][cols - 1] {
            return false;
        }
        while low + 1 < high {
            let mid = low + (high - low) / 2;
            let row = mid / cols;
            let col = mid % cols;
            if matrix[row][col] > target {
                high = mid;
            } else if matrix[row][col] < target {
                low = mid;
            } else {
                return true;
            }
        }
        if matrix[low / cols][low % cols] == target {
            return true;
        }
        if matrix[high / cols][high % cols] == target {
            return true;
        }
        false
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_74() {
        assert_eq!(
            Solution::search_matrix(
                vec![vec![1, 3, 5, 7], vec![10, 11, 16, 20], vec![23, 30, 34, 50]],
                3
            ),
            true
        );
        assert_eq!(
            Solution::search_matrix(
                vec![vec![1, 3, 5, 7], vec![10, 11, 16, 20], vec![23, 30, 34, 50]],
                13
            ),
            false
        );
    }
}

```
