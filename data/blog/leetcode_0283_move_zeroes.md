---
title: Leetcode 0283 move zeroes
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0283 move zeroes in Rust
---

## Solution in Rust

```rust
/**
 * [283] Move Zeroes
 *
 * Given an integer array nums, move all 0's to the end of it while maintaining the relative order of the non-zero elements.
 * Note that you must do this in-place without making a copy of the array.
 *
 * Example 1:
 * Input: nums = [0,1,0,3,12]
 * Output: [1,3,12,0,0]
 * Example 2:
 * Input: nums = [0]
 * Output: [0]
 *
 * Constraints:
 *
 * 	1 <= nums.length <= 10^4
 * 	-2^31 <= nums[i] <= 2^31 - 1
 *
 *
 * Follow up: Could you minimize the total number of operations done?
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/move-zeroes/
// discuss: https://leetcode.com/problems/move-zeroes/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn move_zeroes(nums: &mut Vec<i32>) {
        let n = nums.len();
        let index = nums.iter().position(|&x| x == 0);
        if index.is_none() {
            return;
        }
        let index = index.unwrap();
        if index == n - 1 {
            return;
        }
        let mut front = index;
        let mut back = index;
        while front < n - 1 {
            if nums[front + 1] != 0 {
                nums.swap(back, front + 1);
                back += 1;
                front += 1;
            } else {
                front += 1;
            }
        }
        /*
        while nums[front] != 0 {
            front += 1;
            back += 1;
        }

        while front < n && back < n {
            while front < n - 1 && nums[front] == 0 {
                front += 1;
            }
            while back < n -1 && nums[back] != 0 {
                back += 1;
            }
            if front > back {
                nums.swap(front, back);
            }
            front += 1;
            back += 1;
            //break;
        }
        */
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_283() {
        /*
        let mut vec = vec![0, 1, 0, 3, 12];
        Solution::move_zeroes(&mut vec);
        assert_eq!(vec, vec![1, 3, 12, 0, 0]);
        let mut vec = vec![1, 0];
        Solution::move_zeroes(&mut vec);
        assert_eq!(vec, vec![1, 0]);
        */
        let mut vec = vec![45192,0,-659,-52359,-99225,-75991,0,-15155,27382,59818,0,-30645,-17025,81209,887,64648];
        Solution::move_zeroes(&mut vec);
        assert_eq!(vec, vec![45192,-659,-52359,-99225,-75991,-15155,27382,59818,-30645,-17025,81209,887,64648,0,0,0]);


    }
}

```
