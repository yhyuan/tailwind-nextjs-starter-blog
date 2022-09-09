---
title: Leetcode 0713 subarray product less than k
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0713 subarray product less than k in Rust
---

## Solution in Rust

```rust
/**
 * [713] Subarray Product Less Than K
 *
 * Given an array of integers nums and an integer k, return the number of contiguous subarrays where the product of all the elements in the subarray is strictly less than k.
 *
 * Example 1:
 *
 * Input: nums = [10,5,2,6], k = 100
 * Output: 8
 * Explanation: The 8 subarrays that have product less than 100 are:
 * [10], [5], [2], [6], [10, 5], [5, 2], [2, 6], [5, 2, 6]
 * Note that [10, 5, 2] is not included as the product of 100 is not strictly less than k.
 *
 * Example 2:
 *
 * Input: nums = [1,2,3], k = 0
 * Output: 0
 *
 *
 * Constraints:
 *
 * 	1 <= nums.length <= 3 * 10^4
 * 	1 <= nums[i] <= 1000
 * 	0 <= k <= 10^6
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/subarray-product-less-than-k/
// discuss: https://leetcode.com/problems/subarray-product-less-than-k/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn num_subarray_product_less_than_k(nums: Vec<i32>, k: i32) -> i32 {
        let n = nums.len();
        let mut count = 0;
        for i in 0..n {
            let mut prod = 1i32;
            for j in i..n {
                prod = prod * nums[j];
                if prod >= k {
                    break;
                } else {
                    count += 1;
                }
            }
        }
        count
    }
}
// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_713() {
    }
}

```
