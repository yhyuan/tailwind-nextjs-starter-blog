---
title: Leetcode 2190 most frequent number following key in an array
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 2190 most frequent number following key in an array in Rust
---

## Solution in Rust

```rust
/*
2190. Most Frequent Number Following Key In an Array
Easy

47

25

Add to List

Share
You are given a 0-indexed integer array nums. You are also given an integer key, which is present in nums.

For every unique integer target in nums, count the number of times target immediately follows an occurrence of key in nums. In other words, count the number of indices i such that:

0 <= i <= n - 2,
nums[i] == key and,
nums[i + 1] == target.
Return the target with the maximum count. The test cases will be generated such that the target with maximum count is unique.



Example 1:

Input: nums = [1,100,200,1,100], key = 1
Output: 100
Explanation: For target = 100, there are 2 occurrences at indices 1 and 4 which follow an occurrence of key.
No other integers follow an occurrence of key, so we return 100.
Example 2:

Input: nums = [2,2,2,2,3], key = 2
Output: 2
Explanation: For target = 2, there are 3 occurrences at indices 1, 2, and 3 which follow an occurrence of key.
For target = 3, there is only one occurrence at index 4 which follows an occurrence of key.
target = 2 has the maximum number of occurrences following an occurrence of key, so we return 2.


Constraints:

2 <= nums.length <= 1000
1 <= nums[i] <= 1000
The test cases will be generated such that the answer is unique.

*/
// use std::collections::HashMap;
pub struct Solution {}

use std::collections::HashMap;
impl Solution {
    pub fn most_frequent(nums: Vec<i32>, key: i32) -> i32 {
        let mut hashmap: HashMap<i32, usize> = HashMap::new();
        let n = nums.len();
        for i in 0..n - 1{
            if nums[i] == key  {
                let target = nums[i + 1];
                if hashmap.contains_key(&target) {
                    let v = hashmap.get(&target).unwrap();
                    hashmap.insert(target, v + 1);
                } else {
                    hashmap.insert(target, 1);
                }
            }
        }
        let mut result = -1;
        let mut result_key = -1;
        for (key, value) in hashmap.iter() {
            //println!("{} / {}", key, value);
            if *value as i32 > result {
                result = *value as i32;
                result_key = *key as i32;
            }
        }
        result_key
    }
}
#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_2191() {
        assert_eq!(Solution::most_frequent(vec![1,100,200,1,100], 1), 100);
        assert_eq!(Solution::most_frequent(vec![2,2,2,2,3], 2), 2);
    }
}


```
