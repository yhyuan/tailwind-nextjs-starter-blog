---
title: Leetcode 0162 find peak element
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0162 find peak element in Rust
---

## Solution in Rust

```rust
/**
 * [162] Find Peak Element
 *
 * A peak element is an element that is strictly greater than its neighbors.
 * Given an integer array nums, find a peak element, and return its index. If the array contains multiple peaks, return the index to any of the peaks.
 * You may imagine that nums[-1] = nums[n] = -&infin;.
 * You must write an algorithm that runs in O(log n) time.
 *
 * Example 1:
 *
 * Input: nums = [1,2,3,1]
 * Output: 2
 * Explanation: 3 is a peak element and your function should return the index number 2.
 * Example 2:
 *
 * Input: nums = [1,2,1,3,5,6,4]
 * Output: 5
 * Explanation: Your function can return either index number 1 where the peak element is 2, or index number 5 where the peak element is 6.
 *
 * Constraints:
 *
 * 	1 <= nums.length <= 1000
 * 	-2^31 <= nums[i] <= 2^31 - 1
 * 	nums[i] != nums[i + 1] for all valid i.
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/find-peak-element/
// discuss: https://leetcode.com/problems/find-peak-element/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
/*
If the middle point is on an ascending slope (nums[i] < nums[i + 1]), then the peak should be on the right side. Otherwise, the middle point must be on a descending slope (nums[i] < nums[i - 1]), and the peak should be on the left side.

class Solution(object):
    def findPeakElement(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if len(nums) <= 1:
            return 0

        if nums[0] > nums[1]:
            return 0
        elif nums[-1] > nums[-2]:
            return len(nums) - 1
        else:
            left = 0
            right = len(nums) - 1

            while left < right:
                mid = (left + right) // 2
                if nums[mid] > nums[mid - 1] and nums[mid] > nums[mid + 1]:
                    return mid
                elif nums[mid] < nums[mid - 1]:
                    right = mid
                elif nums[mid] < nums[mid + 1]:
                    left = mid
*/
impl Solution {
    pub fn find_peak_element(nums: Vec<i32>) -> i32 {
        if nums.len() == 1 {
            return 0;
        }
        if nums.len() == 2 {
            return if nums[0] > nums[1] {0} else {1};
        }
        let mut begin = 0usize;
        let mut end = nums.len() - 1;
        while begin + 1 < end {
            if nums[begin] > nums[end] && nums[begin] > nums[begin + 1] {
                return begin as i32;
            }
            let mid = begin + (end - begin) / 2;
            if nums[mid + 1] > nums[mid] {
                begin = mid;
            } else {
                end = mid;
            }
        }
        //println!("begin: {}, end: {}", begin, end);
        end as i32
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_162() {
        assert_eq!(Solution::find_peak_element(vec![1,2,3,1]), 2);
        assert_eq!(Solution::find_peak_element(vec![1,2,1,3,5,6,4]), 5);
        assert_eq!(Solution::find_peak_element(vec![6,5,4,3,2,3,2]), 0);
    }
}

```
