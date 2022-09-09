---
title: Leetcode 0414 third maximum number
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0414 third maximum number in Rust
---

## Solution in Rust

```rust
/**
 * [414] Third Maximum Number
 *
 * Given integer array nums, return the third maximum number in this array. If the third maximum does not exist, return the maximum number.
 *
 * Example 1:
 *
 * Input: nums = [3,2,1]
 * Output: 1
 * Explanation: The third maximum is 1.
 *
 * Example 2:
 *
 * Input: nums = [1,2]
 * Output: 2
 * Explanation: The third maximum does not exist, so the maximum (2) is returned instead.
 *
 * Example 3:
 *
 * Input: nums = [2,2,3,1]
 * Output: 1
 * Explanation: Note that the third maximum here means the third maximum distinct number.
 * Both numbers with value 2 are both considered as second maximum.
 *
 *
 * Constraints:
 *
 * 	1 <= nums.length <= 10^4
 * 	-2^31 <= nums[i] <= 2^31 - 1
 *
 *
 * Follow up: Can you find an O(n) solution?
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/third-maximum-number/
// discuss: https://leetcode.com/problems/third-maximum-number/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn third_max(nums: Vec<i32>) -> i32 {
        let mut maxs: Vec<i64> = vec![i64::MIN, i64::MIN, i64::MIN];
        for &num in nums.iter() {
            let num = num as i64;
            if num == maxs[0] || num == maxs[1] || num == maxs[2] {
                continue;
            }
            if num > maxs[0] {
                maxs[2] = maxs[1];
                maxs[1] = maxs[0];
                maxs[0] = num;
            } else if num > maxs[1] {
                maxs[2] = maxs[1];
                maxs[1] = num;
            } else if num > maxs[2] {
                maxs[2] = num;
            }
        }
        if maxs[2] == i64::MIN {maxs[0] as i32} else {maxs[2] as i32}
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_414() {
        assert_eq!(Solution::third_max(vec![3,2,1]), 1);
        assert_eq!(Solution::third_max(vec![1,2]), 2);
        assert_eq!(Solution::third_max(vec![2,2,3,1]), 1);
        assert_eq!(Solution::third_max(vec![1,2,i32::MIN]), i32::MIN);
    }
}

```
