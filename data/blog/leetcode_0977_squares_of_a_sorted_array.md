---
title: Leetcode 0977 squares of a sorted array
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0977 squares of a sorted array in Rust
---

## Solution in Rust

```rust
/**
 * [977] Squares of a Sorted Array
 *
 * Given an integer array nums sorted in non-decreasing order, return an array of the squares of each number sorted in non-decreasing order.
 *
 * Example 1:
 *
 * Input: nums = [-4,-1,0,3,10]
 * Output: [0,1,9,16,100]
 * Explanation: After squaring, the array becomes [16,1,0,9,100].
 * After sorting, it becomes [0,1,9,16,100].
 *
 * Example 2:
 *
 * Input: nums = [-7,-3,2,3,11]
 * Output: [4,9,9,49,121]
 *
 *
 * Constraints:
 *
 * 	<span>1 <= nums.length <= </span>10^4
 * 	-10^4 <= nums[i] <= 10^4
 * 	nums is sorted in non-decreasing order.
 *
 *
 * Follow up: Squaring each element and sorting the new array is very trivial, could you find an O(n) solution using a different approach?
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/squares-of-a-sorted-array/
// discuss: https://leetcode.com/problems/squares-of-a-sorted-array/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn sorted_squares(nums: Vec<i32>) -> Vec<i32> {
        let n = nums.len();
        let index = nums.iter().position(|&num| num >= 0);
        if index.is_none() {
            let mut nums = nums;
            nums.reverse();
            return nums.iter().map(|&num| num * num).collect::<Vec<_>>();
        }
        let index = index.unwrap();
        if index == 0 {
            return nums.iter().map(|&num| num * num).collect::<Vec<_>>();
        }
        let mut back = index as i32 - 1;
        let mut forward = index as i32;
        let mut result: Vec<i32> = vec![0; n];
        let mut k = 0;
        while k < nums.len() {
            if back >= 0 && forward <= n as i32 - 1 {
                let back_sq = nums[back as usize] * nums[back as usize];
                let forward_sq = nums[forward as usize] * nums[forward as usize];
                result[k] = if back_sq < forward_sq {
                    back -=1;
                    back_sq
                } else {
                    forward += 1;
                    forward_sq
                };
            } else if back < 0 && forward <= n as i32 - 1 {
                result[k] = nums[forward as usize] * nums[forward as usize];
                forward += 1;
            } else if back >= 0 && forward >= n as i32 {
                result[k] = nums[back as usize] * nums[back as usize];
                back -= 1;
            } else {
                break;
            }
            k += 1;
        }
        result
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_977() {
        assert_eq!(Solution::sorted_squares(vec![-4,-1,0,3,10]), vec![0,1,9,16,100]);
        assert_eq!(Solution::sorted_squares(vec![-4,-1,0,3,10]), vec![0,1,9,16,100]);
    }
}

```
