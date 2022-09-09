---
title: Leetcode 0930 binary subarrays with sum
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0930 binary subarrays with sum in Rust
---

## Solution in Rust

```rust
/**
 * [930] Binary Subarrays With Sum
 *
 * Given a binary array nums and an integer goal, return the number of non-empty subarrays with a sum goal.
 *
 * A subarray is a contiguous part of the array.
 *
 *
 * Example 1:
 *
 *
 * Input: nums = [1,0,1,0,1], goal = 2
 * Output: 4
 * Explanation: The 4 subarrays are bolded and underlined below:
 * [<u>1,0,1</u>,0,1]
 * [<u>1,0,1,0</u>,1]
 * [1,<u>0,1,0,1</u>]
 * [1,0,<u>1,0,1</u>]
 *
 *
 * Example 2:
 *
 *
 * Input: nums = [0,0,0,0,0], goal = 0
 * Output: 15
 *
 *
 *
 * Constraints:
 *
 *
 * 	1 <= nums.length <= 3 * 10^4
 * 	nums[i] is either 0 or 1.
 * 	0 <= goal <= nums.length
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/binary-subarrays-with-sum/
// discuss: https://leetcode.com/problems/binary-subarrays-with-sum/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
use std::collections::HashMap;
impl Solution {
    pub fn num_subarrays_with_sum(nums: Vec<i32>, goal: i32) -> i32 {
        let mut memo: HashMap<i32, i32> = HashMap::new();
        let n = nums.len();
        let mut ans = 0i32;
        let mut pre_sum = 0;
        for i in 0..n {
            pre_sum = pre_sum + nums[i];
            if pre_sum == goal {
                ans += 1;
            }
            if memo.contains_key(&pre_sum) {
                let val = *memo.get(&pre_sum).unwrap();
                ans += val;
            }
            let next_val = pre_sum + goal;
            if memo.contains_key(&next_val) {
                *memo.get_mut(&next_val).unwrap() += 1;
            } else {
                memo.insert(next_val, 1);
            }
        }
        ans
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_930() {
        assert_eq!(Solution::num_subarrays_with_sum(vec![1,0,1,0,1], 2), 4);
        assert_eq!(Solution::num_subarrays_with_sum(vec![0,0,0,0,0], 0), 15);
    }
}

```
