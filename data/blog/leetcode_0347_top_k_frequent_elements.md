---
title: Leetcode 0347 top k frequent elements
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0347 top k frequent elements in Rust
---

## Solution in Rust

```rust
/**
 * [347] Top K Frequent Elements
 *
 * Given an integer array nums and an integer k, return the k most frequent elements. You may return the answer in any order.
 *
 * Example 1:
 * Input: nums = [1,1,1,2,2,3], k = 2
 * Output: [1,2]
 * Example 2:
 * Input: nums = [1], k = 1
 * Output: [1]
 *
 * Constraints:
 *
 * 	1 <= nums.length <= 10^5
 * 	k is in the range [1, the number of unique elements in the array].
 * 	It is guaranteed that the answer is unique.
 *
 *
 * Follow up: Your algorithm's time complexity must be better than O(n log n), where n is the array's size.
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/top-k-frequent-elements/
// discuss: https://leetcode.com/problems/top-k-frequent-elements/discuss/?currentPage=1&orderBy=most_votes&query=
use std::collections::HashMap;
impl Solution {
    pub fn top_k_frequent(nums: Vec<i32>, k: i32) -> Vec<i32> {
        let mut hashmap: HashMap<i32, usize> = HashMap::new();
        for &num in nums.iter() {
            if hashmap.contains_key(&num) {
                hashmap.insert(num, hashmap[&num] + 1);
            } else {
                hashmap.insert(num, 1);
            }
        }
        let mut keys: Vec<i32> = hashmap.iter().map(|(&k, v)| k).collect();
        keys.sort_by(|a, b| hashmap[b].partial_cmp(&hashmap[a]).unwrap());
        let mut result: Vec<i32> = vec![];
        for i in 0..k {
            result.push(keys[i as usize]);
        }
        result
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_347() {
        assert_eq!(Solution::top_k_frequent(vec![1, 1, 1, 2, 2, 3], 2), vec![1, 2]);
    }
}

```
