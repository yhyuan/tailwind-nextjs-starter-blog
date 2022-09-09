---
title: Leetcode 0167 two sum ii input array is sorted
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0167 two sum ii input array is sorted in Rust
---

## Solution in Rust

```rust
/**
 * [167] Two Sum II - Input array is sorted
 *
 * Given an array of integers numbers that is already sorted in non-decreasing order, find two numbers such that they add up to a specific target number.
 * Return the indices of the two numbers (1-indexed) as an integer array answer of size 2, where 1 <= answer[0] < answer[1] <= numbers.length.
 * The tests are generated such that there is exactly one solution. You may not use the same element twice.
 *
 * Example 1:
 *
 * Input: numbers = [2,7,11,15], target = 9
 * Output: [1,2]
 * Explanation: The sum of 2 and 7 is 9. Therefore index1 = 1, index2 = 2.
 *
 * Example 2:
 *
 * Input: numbers = [2,3,4], target = 6
 * Output: [1,3]
 *
 * Example 3:
 *
 * Input: numbers = [-1,0], target = -1
 * Output: [1,2]
 *
 *
 * Constraints:
 *
 * 	2 <= numbers.length <= 3 * 10^4
 * 	-1000 <= numbers[i] <= 1000
 * 	numbers is sorted in non-decreasing order.
 * 	-1000 <= target <= 1000
 * 	The tests are generated such that there is exactly one solution.
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/
// discuss: https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn two_sum(numbers: Vec<i32>, target: i32) -> Vec<i32> {
        let mut begin = 0usize;
        let mut end = numbers.len() - 1;
        while end >= begin {
            let max_val = target - numbers[begin];
            while numbers[end] > max_val {
                end -= 1;
            }
            if numbers[begin] + numbers[end] == target {
                return vec![begin as i32 + 1, end as i32 + 1];
            }
            begin += 1;
        }
        vec![0, 0]
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_167() {
        assert_eq!(Solution::two_sum(vec![2, 7, 11, 15], 9), vec![1, 2]);
        assert_eq!(Solution::two_sum(vec![2, 3, 4], 6), vec![1, 3]);
        assert_eq!(Solution::two_sum(vec![-1, 0], -1), vec![1, 2]);
        assert_eq!(Solution::two_sum(vec![-1, 0, 1, 2, 7, 11, 15], 9), vec![4, 5]);
    }
}

```
