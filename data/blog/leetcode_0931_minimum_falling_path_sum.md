---
title: Leetcode 0931 minimum falling path sum
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0931 minimum falling path sum in Rust
---

## Solution in Rust

```rust
/**
 * [931] Minimum Falling Path Sum
 *
 * Given an n x n array of integers matrix, return the minimum sum of any falling path through matrix.
 * A falling path starts at any element in the first row and chooses the element in the next row that is either directly below or diagonally left/right. Specifically, the next element from position (row, col) will be (row + 1, col - 1), (row + 1, col), or (row + 1, col + 1).
 *
 * Example 1:
 *
 * Input: matrix = [[2,1,3],[6,5,4],[7,8,9]]
 * Output: 13
 * Explanation: There are two falling paths with a minimum sum underlined below:
 * [[2,<u>1</u>,3],      [[2,<u>1</u>,3],
 *  [6,<u>5</u>,4],       [6,5,<u>4</u>],
 *  [<u>7</u>,8,9]]       [7,<u>8</u>,9]]
 *
 * Example 2:
 *
 * Input: matrix = [[-19,57],[-40,-5]]
 * Output: -59
 * Explanation: The falling path with a minimum sum is underlined below:
 * [[<u>-19</u>,57],
 *  [<u>-40</u>,-5]]
 *
 * Example 3:
 *
 * Input: matrix = [[-48]]
 * Output: -48
 *
 *
 * Constraints:
 *
 * 	n == matrix.length
 * 	n == matrix[i].length
 * 	1 <= n <= 100
 * 	-100 <= matrix[i][j] <= 100
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/minimum-falling-path-sum/
// discuss: https://leetcode.com/problems/minimum-falling-path-sum/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn min_falling_path_sum(matrix: Vec<Vec<i32>>) -> i32 {
        let n = matrix.len();
        let mut dp = matrix[0].clone();
        for i in 1..n {
            dp = (0..n).into_iter()
                .map(|j| if j > 0 && j < n - 1 {
                        i32::min(i32::min(dp[j - 1], dp[j]), dp[j + 1]) + matrix[i][j]
                    } else if j == 0 {
                        i32::min(dp[j], dp[j + 1]) + matrix[i][j]
                    } else {
                        i32::min(dp[j], dp[j - 1]) + matrix[i][j]
                    }).collect::<Vec<_>>();
        }
        dp.into_iter().min().unwrap()
    }
}
// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_931() {
    }
}

```
