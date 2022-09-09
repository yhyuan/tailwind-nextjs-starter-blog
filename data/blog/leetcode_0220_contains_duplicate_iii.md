---
title: Leetcode 0220 contains duplicate iii
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0220 contains duplicate iii in Rust
---

## Solution in Rust

```rust
/**
 * [220] Contains Duplicate III
 *
 * Given an integer array nums and two integers k and t, return true if there are two distinct indices i and j in the array such that abs(nums[i] - nums[j]) <= t and abs(i - j) <= k.
 *
 * Example 1:
 * Input: nums = [1,2,3,1], k = 3, t = 0
 * Output: true
 * Example 2:
 * Input: nums = [1,0,1,1], k = 1, t = 2
 * Output: true
 * Example 3:
 * Input: nums = [1,5,9,1,5,9], k = 2, t = 3
 * Output: false
 *
 * Constraints:
 *
 * 	0 <= nums.length <= 2 * 10^4
 * 	-2^31 <= nums[i] <= 2^31 - 1
 * 	0 <= k <= 10^4
 * 	0 <= t <= 2^31 - 1
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/contains-duplicate-iii/
// discuss: https://leetcode.com/problems/contains-duplicate-iii/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn contains_nearby_almost_duplicate(nums: Vec<i32>, k: i32, t: i32) -> bool {
        let n = nums.len();
        for i in 0..n {
            for j in i + 1..n {
                if (nums[i] as i64 - nums[j] as i64).abs() <= t as i64 && (j - i) as i32 <= k {
                    return true;
                }
            }
        }
        false
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_220() {
        //assert_eq!(Solution::contains_nearby_almost_duplicate(vec![1,2,3,1], 3, 0), true);
        //assert_eq!(Solution::contains_nearby_almost_duplicate(vec![1,0,1,1], 1, 2), true);
        //assert_eq!(Solution::contains_nearby_almost_duplicate(vec![1,5,9,1,5,9], 2, 3), false);
        assert_eq!(Solution::contains_nearby_almost_duplicate(vec![-2147483648,2147483647], 1, 1), false);
    }
}

```
