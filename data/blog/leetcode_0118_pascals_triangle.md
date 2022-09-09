---
title: Leetcode 0118 pascals triangle
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0118 pascals triangle in Rust
---

## Solution in Rust

```rust
/**
 * [118] Pascal's Triangle
 *
 * Given an integer numRows, return the first numRows of Pascal's triangle.
 * In Pascal's triangle, each number is the sum of the two numbers directly above it as shown:
 * <img alt="" src="https://upload.wikimedia.org/wikipedia/commons/0/0d/PascalTriangleAnimated2.gif" style="height:240px; width:260px" />
 *
 * Example 1:
 * Input: numRows = 5
 * Output: [[1],[1,1],[1,2,1],[1,3,3,1],[1,4,6,4,1]]
 * Example 2:
 * Input: numRows = 1
 * Output: [[1]]
 *
 * Constraints:
 *
 * 	1 <= numRows <= 30
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/pascals-triangle/
// discuss: https://leetcode.com/problems/pascals-triangle/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn generate(num_rows: i32) -> Vec<Vec<i32>> {
        let mut result:Vec<Vec<i32>> = vec![];
        result.push(vec![1i32]);
        for i in 1..num_rows as usize {
            let mut new_row = vec![0; result[i - 1].len() + 1];
            let n = new_row.len();
            new_row[0] = 1;
            new_row[n - 1] = 1;
            for j in 0..result[i - 1].len() - 1 {
                new_row[j + 1] = result[i - 1][j] + result[i - 1][j + 1];
            }
            result.push(new_row);
        }
        result
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_118() {
        assert_eq!(Solution::generate(1), vec![vec![1]]);
        assert_eq!(
            Solution::generate(5),
            vec![
                vec![1],
                vec![1, 1],
                vec![1, 2, 1],
                vec![1, 3, 3, 1],
                vec![1, 4, 6, 4, 1]
            ]
        );
    }
}

```
