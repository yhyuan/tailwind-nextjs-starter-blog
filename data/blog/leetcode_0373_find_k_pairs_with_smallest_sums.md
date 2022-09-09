---
title: Leetcode 0373 find k pairs with smallest sums
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0373 find k pairs with smallest sums in Rust
---

## Solution in Rust

```rust
/**
 * [373] Find K Pairs with Smallest Sums
 *
 * You are given two integer arrays nums1 and nums2 sorted in ascending order and an integer k.
 * Define a pair (u, v) which consists of one element from the first array and one element from the second array.
 * Return the k pairs (u1, v1), (u2, v2), ..., (uk, vk) with the smallest sums.
 *
 * Example 1:
 *
 * Input: nums1 = [1,7,11], nums2 = [2,4,6], k = 3
 * Output: [[1,2],[1,4],[1,6]]
 * Explanation: The first 3 pairs are returned from the sequence: [1,2],[1,4],[1,6],[7,2],[7,4],[11,2],[7,6],[11,4],[11,6]
 *
 * Example 2:
 *
 * Input: nums1 = [1,1,2], nums2 = [1,2,3], k = 2
 * Output: [[1,1],[1,1]]
 * Explanation: The first 2 pairs are returned from the sequence: [1,1],[1,1],[1,2],[2,1],[1,2],[2,2],[1,3],[1,3],[2,3]
 *
 * Example 3:
 *
 * Input: nums1 = [1,2], nums2 = [3], k = 3
 * Output: [[1,3],[2,3]]
 * Explanation: All possible pairs are returned from the sequence: [1,3],[2,3]
 *
 *
 * Constraints:
 *
 * 	1 <= nums1.length, nums2.length <= 10^5
 * 	-10^9 <= nums1[i], nums2[i] <= 10^9
 * 	nums1 and nums2 both are sorted in ascending order.
 * 	1 <= k <= 1000
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/find-k-pairs-with-smallest-sums/
// discuss: https://leetcode.com/problems/find-k-pairs-with-smallest-sums/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
use std::collections::BinaryHeap;
impl Solution {
    pub fn k_smallest_pairs(nums1: Vec<i32>, nums2: Vec<i32>, k: i32) -> Vec<Vec<i32>> {
        let n1 = nums1.len();
        let n2 = nums2.len();
        let k = k as usize;
        let mut heap: BinaryHeap<(i32, usize, usize)> = BinaryHeap::new();
        //let mut result: Vec<Vec<i32>> = vec![];
        for i in 0..nums1.len() {
            for j in 0..nums2.len() {
                if heap.len() < k {
                    heap.push((nums1[i] + nums2[j], i, j));
                    continue;
                }
                let (sum, index_i, index_j) = heap.peek().unwrap();
                let curr_sum = nums1[i] + nums2[j];
                if sum > &curr_sum {
                    heap.pop();
                    heap.push((curr_sum, i, j));
                } else {
                    break;
                }
            }
        }
        let mut result: Vec<Vec<i32>> = vec![];
        while !heap.is_empty() {
            let (sum, index_i, index_j) = heap.pop().unwrap();
            result.insert(0, vec![nums1[index_i], nums2[index_j]]);
        }
        result
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_373() {
        assert_eq!(Solution::k_smallest_pairs(vec![1,7,11], vec![2,4,6], 3), vec![vec![1,2],vec![1,4],vec![1,6]]);
        assert_eq!(Solution::k_smallest_pairs(vec![1,1,2], vec![1,2,3], 2), vec![vec![1,1],vec![1,1]]);
        assert_eq!(Solution::k_smallest_pairs(vec![1,2], vec![3], 3), vec![vec![1,3],vec![2,3]]);
        assert_eq!(Solution::k_smallest_pairs(vec![1,1,2], vec![1,2,3], 10), vec![vec![1,1],vec![1,1],vec![1,2],vec![1,2],vec![2,1],vec![1,3],vec![1,3],vec![2,2],vec![2,3]]);
    }
}

```
