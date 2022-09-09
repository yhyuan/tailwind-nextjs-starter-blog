---
title: Leetcode 2195 append k integers with minimal sum
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 2195 append k integers with minimal sum in Rust
---

## Solution in Rust

```rust
/*
2195. Append K Integers With Minimal Sum
Medium

153

162

Add to List

Share
You are given an integer array nums and an integer k. Append k unique positive integers that do not appear in nums to nums such that the resulting total sum is minimum.

Return the sum of the k integers appended to nums.



Example 1:

Input: nums = [1,4,25,10,25], k = 2
Output: 5
Explanation: The two unique positive integers that do not appear in nums which we append are 2 and 3.
The resulting sum of nums is 1 + 4 + 25 + 10 + 25 + 2 + 3 = 70, which is the minimum.
The sum of the two integers appended is 2 + 3 = 5, so we return 5.
Example 2:

Input: nums = [5,6], k = 6
Output: 25
Explanation: The six unique positive integers that do not appear in nums which we append are 1, 2, 3, 4, 7, and 8.
The resulting sum of nums is 5 + 6 + 1 + 2 + 3 + 4 + 7 + 8 = 36, which is the minimum.
The sum of the six integers appended is 1 + 2 + 3 + 4 + 7 + 8 = 25, so we return 25.


Constraints:

1 <= nums.length <= 105
1 <= nums[i] <= 109
1 <= k <= 108
Accepted
8,258

*/
// use std::collections::HashMap;
pub struct Solution {}

impl Solution {
    pub fn minimal_k_sum(nums: Vec<i32>, k: i32) -> i64 {
        let mut nums = nums;
        nums.sort();
        let mut start = 0i64;
        let mut end = 0i64;
        let mut k = k as i64;
        let n = nums.len();
        let mut result = 0i64;
        for i in 0..n {
            end = nums[i] as i64;
            if start == end {
                continue;
            }
            let size = end - start - 1;
            //println!("start: {}, end: {}, size: {}", start, end, size);
            if k <= size {
                result += k * start + k * (k + 1) / 2;
                k = 0;
                break;
            } else {
                result += size * start + size * (size + 1) / 2;
                k = k - size;
            }
            start = end;
        }
        //println!("k: {}", k);
        let mut v = nums[n - 1] as i64 + 1;
        for _ in 0..k {
            result += v;
            v = v + 1;
        }
        result as i64
    }
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_2195() {
        assert_eq!(Solution::minimal_k_sum(vec![1,4,25,10,25], 2), 5);
        assert_eq!(Solution::minimal_k_sum(vec![5,6], 6), 25);
    }
}


```
