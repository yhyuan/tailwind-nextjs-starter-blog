---
title: Leetcode 0215 kth largest element in an array
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0215 kth largest element in an array in Rust
---

## Solution in Rust

```rust
/**
 * [215] Kth Largest Element in an Array
 *
 * Given an integer array nums and an integer k, return the k^th largest element in the array.
 * Note that it is the k^th largest element in the sorted order, not the k^th distinct element.
 *
 * Example 1:
 * Input: nums = [3,2,1,5,6,4], k = 2
 * Output: 5
 * Example 2:
 * Input: nums = [3,2,3,1,2,4,5,5,6], k = 4
 * Output: 4
 *
 * Constraints:
 *
 * 	1 <= k <= nums.length <= 10^4
 * 	-10^4 <= nums[i] <= 10^4
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/kth-largest-element-in-an-array/
// discuss: https://leetcode.com/problems/kth-largest-element-in-an-array/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
use std::collections::BinaryHeap;
impl Solution {
    pub fn find_kth_largest(nums: Vec<i32>, k: i32) -> i32 {
        let k = k as usize;
        let n = nums.len();
        //max_heap
        let mut heap: BinaryHeap<i32> = BinaryHeap::with_capacity(k as usize);
        for i in 0..k {
            heap.push(-nums[i]);
        }
        for i in k..n {
            let val = heap.peek().unwrap();
            if *val > -nums[i] {
                heap.pop();
                heap.push(-nums[i]);
            }
        }
        -heap.pop().unwrap()
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_215() {
        assert_eq!(Solution::find_kth_largest(vec![3,2,1,5,6,4], 2), 5);
        assert_eq!(Solution::find_kth_largest(vec![3,2,3,1,2,4,5,5,6], 4), 4);
    }
}

```
