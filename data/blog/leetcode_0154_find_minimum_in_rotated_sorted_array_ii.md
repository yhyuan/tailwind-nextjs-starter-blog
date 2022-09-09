---
title: Leetcode 0154 find minimum in rotated sorted array ii
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0154 find minimum in rotated sorted array ii in Rust
---

## Solution in Rust

```rust
/**
 * [154] Find Minimum in Rotated Sorted Array II
 *
 * Suppose an array of length n sorted in ascending order is rotated between 1 and n times. For example, the array nums = [0,1,4,4,5,6,7] might become:
 *
 * 	[4,5,6,7,0,1,4] if it was rotated 4 times.
 * 	[0,1,4,4,5,6,7] if it was rotated 7 times.
 *
 * Notice that rotating an array [a[0], a[1], a[2], ..., a[n-1]] 1 time results in the array [a[n-1], a[0], a[1], a[2], ..., a[n-2]].
 * Given the sorted rotated array nums that may contain duplicates, return the minimum element of this array.
 * You must decrease the overall operation steps as much as possible.
 *
 * Example 1:
 * Input: nums = [1,3,5]
 * Output: 1
 * Example 2:
 * Input: nums = [2,2,2,0,1]
 * Output: 0
 *
 * Constraints:
 *
 * 	n == nums.length
 * 	1 <= n <= 5000
 * 	-5000 <= nums[i] <= 5000
 * 	nums is sorted and rotated between 1 and n times.
 *
 *
 * Follow up: This problem is similar to <a href="https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/description/" target="_blank">Find Minimum in Rotated Sorted Array</a>, but nums may contain duplicates. Would this affect the runtime complexity? How and why?
 *
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/find-minimum-in-rotated-sorted-array-ii/
// discuss: https://leetcode.com/problems/find-minimum-in-rotated-sorted-array-ii/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    /*
    pub fn find_min_helper(nums: &Vec<i32>, begin: usize, end: usize) -> i32 {
        if begin + 1 >= end {
            return nums[end];
        }
        let mut begin = begin;
        let mut end = end;
        if nums[begin] == nums[end] {
            let val = nums[begin];
            let mut i = 0usize;
            while begin + i <= end && nums[begin + i] == val {
                i += 1;
            }
            begin += i - 1;
            let mut i = 0usize;
            while end - i >= begin && nums[end - i] == val {
                i += 1;
            }
            end -= i;
        }
        if begin + 1 >= end {
            return nums[end];
        }
        let middle = begin + (end - begin) / 2;
        if nums[middle] > nums[begin] {
            Solution::find_min_helper(nums, middle, end)
        } else {
            Solution::find_min_helper(nums, begin, middle)
        }
    }
    pub fn find_min(nums: Vec<i32>) -> i32 {
        let n = nums.len();
        if nums[0] < nums[n - 1] {
            return nums[0];
        }
        Solution::find_min_helper(&nums, 0, n - 1)
    }
    */
    pub fn find_min(nums: Vec<i32>) -> i32 {
        let n = nums.len();
        if nums[0] < nums[n - 1] {
            return nums[0];
        }
        let mut low = 0;
        let mut high = n - 1;
        while low + 1 < high {
            while low < high && nums[low] == nums[low + 1] {
                low += 1;
            }
            while high > low && nums[high] == nums[high - 1] {
                high -= 1;
            }
            if low + 1 >= high {
                break;
            }
            //println!("low: {}, high: {}", low, high);
            let mid = low + (high - low) / 2;
            if nums[mid] > nums[low] {
                low = mid;
            } else if nums[mid] < nums[high] {
                high = mid;
            }
            // println!("low: {}, high: {}", low, high);
        }
        i32::min(nums[low], nums[high])
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_154() {
        assert_eq!(Solution::find_min(vec![1,3,5]), 1);
        assert_eq!(Solution::find_min(vec![2,2,2,0,1]), 0);
        assert_eq!(Solution::find_min(vec![2, 0, 1, 2, 2]), 0);
        assert_eq!(Solution::find_min(vec![1, 2, 2, 2, 2, 2]), 1);
        assert_eq!(Solution::find_min(vec![1, 3, 3]), 1);
        assert_eq!(Solution::find_min(vec![3, 1, 3, 3]), 1);
        assert_eq!(Solution::find_min(vec![10,1,10,10,10]), 1);
        assert_eq!(Solution::find_min(vec![1,1,1]), 1);
        assert_eq!(Solution::find_min(vec![2,1,1,2,2,2,2]), 1);
        assert_eq!(Solution::find_min(vec![1,1,0,1,1,1,1,1,1,1,1,1]), 0);
    }
}

```
