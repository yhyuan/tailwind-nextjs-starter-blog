---
title: Leetcode 0035 search insert position
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0035 search insert position in Rust
---

## Solution in Rust

```rust
/**
 * [35] Search Insert Position
 *
 * Given a sorted array of distinct integers and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.
 * You must write an algorithm with O(log n) runtime complexity.
 *
 * Example 1:
 * Input: nums = [1,3,5,6], target = 5
 * Output: 2
 * Example 2:
 * Input: nums = [1,3,5,6], target = 2
 * Output: 1
 * Example 3:
 * Input: nums = [1,3,5,6], target = 7
 * Output: 4
 * Example 4:
 * Input: nums = [1,3,5,6], target = 0
 * Output: 0
 * Example 5:
 * Input: nums = [1], target = 0
 * Output: 0
 *
 * Constraints:
 *
 * 	1 <= nums.length <= 10^4
 * 	-10^4 <= nums[i] <= 10^4
 * 	nums contains distinct values sorted in ascending order.
 * 	-10^4 <= target <= 10^4
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/search-insert-position/
// discuss: https://leetcode.com/problems/search-insert-position/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    /*
    pub fn helper(nums: &Vec<i32>, target: i32, start: usize, end: usize) -> i32 {
        if (end - start) <= 1 {
            if target > nums[end] {
                return end as i32 + 1;
            } else if target <= nums[start] {
                return start as i32;
            } else {
                return start as i32 + 1;
            }
        }
        let middle = (start + end) / 2;
        if nums[middle] == target {
            middle as i32
        } else if nums[middle] > target {
            Solution::helper(nums, target, start, middle - 1)
        } else {
            Solution::helper(nums, target, middle + 1, end)
        }
    }
    pub fn search_insert(nums: Vec<i32>, target: i32) -> i32 {
        if nums.len() == 0 {
            panic!("Zero array!")
        }
        Solution::helper(&nums, target, 0usize, nums.len() - 1)
    }
    */
    pub fn search_insert(nums: Vec<i32>, target: i32) -> i32 {
        let n = nums.len();
        if n == 0 {
            panic!("Zero array!")
        }
        if target <= nums[0] {
            return 0;
        }
        if target == nums[n - 1] {
            return n as i32 - 1;
        }
        if target > nums[n - 1] {
            return n as i32;
        }
        let mut low = 0;
        let mut high = n - 1;
        while low  + 1 < high {
            let mid = low + (high - low) / 2;
            if nums[mid] > target {
                high = mid;
            } else if nums[mid] < target {
                low = mid;
            } else {
                return mid as i32;
            }
        }
        low as i32 + 1
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_35() {
        assert_eq!(Solution::search_insert(vec![1,3,5,6], 5), 2);
        assert_eq!(Solution::search_insert(vec![1,3,5,6], 2), 1);
        assert_eq!(Solution::search_insert(vec![1,3,5,6], 7), 4);
        assert_eq!(Solution::search_insert(vec![1,3,5,6], 0), 0);
        assert_eq!(Solution::search_insert(vec![1], 0), 0);
    }
}

```
