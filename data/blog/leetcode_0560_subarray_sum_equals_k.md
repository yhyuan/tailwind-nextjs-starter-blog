---
title: Leetcode 0560 subarray sum equals k
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0560 subarray sum equals k in Rust
---

## Solution in Rust

```rust
/**
 * [560] Subarray Sum Equals K
 *
 * Given an array of integers nums and an integer k, return the total number of continuous subarrays whose sum equals to k.
 *
 * Example 1:
 * Input: nums = [1,1,1], k = 2
 * Output: 2
 * Example 2:
 * Input: nums = [1,2,3], k = 3
 * Output: 2
 *
 * Constraints:
 *
 * 	1 <= nums.length <= 2 * 10^4
 * 	-1000 <= nums[i] <= 1000
 * 	-10^7 <= k <= 10^7
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/subarray-sum-equals-k/
// discuss: https://leetcode.com/problems/subarray-sum-equals-k/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
/*
impl Solution {
    pub fn subarray_sum(nums: Vec<i32>, k: i32) -> i32 {
        let n = nums.len();
        let mut totals: Vec<i32> = Vec::with_capacity(n);
        let mut total = 0 ;
        for &num in nums.iter() {
            total += num;
            totals.push(total);
        }
        let mut res = 0i32;
        for i in 0..n {
            if totals[i] == k {
                res += 1;
            }
            for j in i + 1..n {
                let sum = totals[j] - totals[i];
                if sum == k {
                    res += 1;
                }
            }
        }
        res
    }
}
*/
impl Solution {
    pub fn subarray_sum(nums: Vec<i32>, k: i32) -> i32 {
        let n = nums.len();
        let mut res = 0i32;
        for i in 0..n {
            let mut total = 0i32;
            for j in i..n {
                total += nums[j];
                if total == k {
                    res += 1;
                }
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
    fn test_560() {
        assert_eq!(Solution::subarray_sum(vec![1,1,1], 2), 2);
        assert_eq!(Solution::subarray_sum(vec![1,2,3], 3), 2);
    }
}

```
