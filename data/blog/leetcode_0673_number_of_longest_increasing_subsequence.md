---
title: Leetcode 0673 number of longest increasing subsequence
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0673 number of longest increasing subsequence in Rust
---

## Solution in Rust

```rust
/**
 * [673] Number of Longest Increasing Subsequence
 *
 * Given an integer array nums, return the number of longest increasing subsequences.
 * Notice that the sequence has to be strictly increasing.
 *
 * Example 1:
 *
 * Input: nums = [1,3,5,4,7]
 * Output: 2
 * Explanation: The two longest increasing subsequences are [1, 3, 4, 7] and [1, 3, 5, 7].
 *
 * Example 2:
 *
 * Input: nums = [2,2,2,2,2]
 * Output: 5
 * Explanation: The length of longest continuous increasing subsequence is 1, and there are 5 subsequences' length is 1, so output 5.
 *
 *
 * Constraints:
 *
 * 	1 <= nums.length <= 2000
 * 	-10^6 <= nums[i] <= 10^6
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/number-of-longest-increasing-subsequence/
// discuss: https://leetcode.com/problems/number-of-longest-increasing-subsequence/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn find_number_of_lis(nums: Vec<i32>) -> i32 {
        let n = nums.len();
        let mut dp: Vec<i32> = vec![0; n];
        let mut dp_counts: Vec<i32> = vec![0; n];
        dp[0] = 1;
        dp_counts[0] = 1;
        for i in 1..n {
            dp[i] = 1;
            dp_counts[i] = 1;
            for j in 0..i {
                if nums[j] < nums[i] {
                    if dp[j] + 1 > dp[i] {
                        dp[i] = dp[j] + 1;
                        dp_counts[i] = dp_counts[j];
                    } else if dp[j] + 1 == dp[i] {
                        dp_counts[i] += dp_counts[j];
                    }
                }
            }
        }
        //println!("dp: {:?}", dp);
        //println!("dp_counts: {:?}", dp_counts);
        let max_val = *dp.iter().max().unwrap();
        let mut res = 0;
        for i in 0..n {
            if dp[i] == max_val {
                res += dp_counts[i];
            }
        }
        res
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_673() {
        assert_eq!(Solution::find_number_of_lis(vec![1,3,5,4,7]), 2);
        assert_eq!(Solution::find_number_of_lis(vec![2,2,2,2,2]), 5);
    }
}

```
