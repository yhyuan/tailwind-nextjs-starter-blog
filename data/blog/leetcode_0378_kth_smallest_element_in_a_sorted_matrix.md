---
title: Leetcode 0378 kth smallest element in a sorted matrix
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0378 kth smallest element in a sorted matrix in Rust
---

## Solution in Rust

```rust
/**
 * [378] Kth Smallest Element in a Sorted Matrix
 *
 * Given an n x n matrix where each of the rows and columns are sorted in ascending order, return the k^th smallest element in the matrix.
 * Note that it is the k^th smallest element in the sorted order, not the k^th distinct element.
 *
 * Example 1:
 *
 * Input: matrix = [[1,5,9],[10,11,13],[12,13,15]], k = 8
 * Output: 13
 * Explanation: The elements in the matrix are [1,5,9,10,11,12,13,<u>13</u>,15], and the 8^th smallest number is 13
 *
 * Example 2:
 *
 * Input: matrix = [[-5]], k = 1
 * Output: -5
 *
 *
 * Constraints:
 *
 * 	n == matrix.length
 * 	n == matrix[i].length
 * 	1 <= n <= 300
 * 	-10^9 <= matrix[i][j] <= 10^9
 * 	All the rows and columns of matrix are guaranteed to be sorted in non-decreasing order.
 * 	1 <= k <= n^2
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/
// discuss: https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
use std::collections::BinaryHeap;
impl Solution {
    pub fn kth_smallest(matrix: Vec<Vec<i32>>, k: i32) -> i32 {
        let k = k as usize;
        let mut heap: BinaryHeap<i32> = BinaryHeap::with_capacity(k);
        let n = matrix.len();
        for i in 0..n {
            for j in 0..n {
                if heap.len() < k {
                    heap.push(matrix[i][j]);
                } else {
                    let val = heap.peek().unwrap();
                    if val > &matrix[i][j] {
                        heap.pop();
                        heap.push(matrix[i][j]);
                    }
                }
            }
        }
        let v = heap.peek().unwrap();
        *v
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_378() {
        assert_eq!(Solution::kth_smallest(vec![vec![1,5,9],vec![10,11,13],vec![12,13,15]], 8), 13);
        assert_eq!(Solution::kth_smallest(vec![vec![1,2],vec![1,3]], 2), 1);
    }
}

```
