---
title: Leetcode 0075 sort colors
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0075 sort colors in Rust
---

## Solution in Rust

```rust
/**
 * [75] Sort Colors
 *
 * Given an array nums with n objects colored red, white, or blue, sort them <a href="https://en.wikipedia.org/wiki/In-place_algorithm" target="_blank">in-place</a> so that objects of the same color are adjacent, with the colors in the order red, white, and blue.
 * We will use the integers 0, 1, and 2 to represent the color red, white, and blue, respectively.
 * You must solve this problem without using the library's sort function.
 *
 * Example 1:
 * Input: nums = [2,0,2,1,1,0]
 * Output: [0,0,1,1,2,2]
 * Example 2:
 * Input: nums = [2,0,1]
 * Output: [0,1,2]
 * Example 3:
 * Input: nums = [0]
 * Output: [0]
 * Example 4:
 * Input: nums = [1]
 * Output: [1]
 *
 * Constraints:
 *
 * 	n == nums.length
 * 	1 <= n <= 300
 * 	nums[i] is 0, 1, or 2.
 *
 *
 * Follow up: Could you come up with a one-pass algorithm using only constant extra space?
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/sort-colors/
// discuss: https://leetcode.com/problems/sort-colors/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn sort_colors(nums: &mut Vec<i32>) {
        let mut count_0 = 0usize;
        let mut count_1 = 0usize;
        let mut count_2 = 0usize;
        for &num in nums.iter() {
            if num == 0 {
                count_0 += 1;
            } else if num == 1 {
                count_1 += 1;
            } else if num == 2 {
                count_2 += 1;
            } else {
                panic!("Wrong input!")
            }
        }
        for i in 0..count_0 {
            nums[i] = 0;
        }
        for i in count_0..count_0 + count_1 {
            nums[i] = 1;
        }
        for i in count_0 + count_1..nums.len() {
            nums[i] = 2;
        }
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_75() {
        let mut vec = vec![
            1, 2, 0, 1, 2, 2, 2, 0, 0, 0, 2, 1, 1, 2, 0, 1, 2, 2, 1, 1, 0,
        ];
        Solution::sort_colors(&mut vec);
        assert_eq!(
            vec,
            vec![0, 0, 0, 0, 0, 0, 1, 1, 1, 1, 1, 1, 1, 2, 2, 2, 2, 2, 2, 2, 2]
        );

        let mut vec = vec![];
        Solution::sort_colors(&mut vec);
        let empty: Vec<i32> = vec![];
        assert_eq!(vec, empty);

        let mut vec = vec![2, 2, 2];
        Solution::sort_colors(&mut vec);
        assert_eq!(vec, vec![2, 2, 2]);
    }
}

```
