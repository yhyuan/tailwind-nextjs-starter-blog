---
title: Leetcode 0018 4sum
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0018 4sum in Rust
---

## Solution in Rust

```rust
/**
 * [18] 4Sum
 *
 * Given an array nums of n integers, return an array of all the unique quadruplets [nums[a], nums[b], nums[c], nums[d]] such that:
 *
 * 	0 <= a, b, c, d < n
 * 	a, b, c, and d are distinct.
 * 	nums[a] + nums[b] + nums[c] + nums[d] == target
 *
 * You may return the answer in any order.
 *
 * Example 1:
 *
 * Input: nums = [1,0,-1,0,-2,2], target = 0
 * Output: [[-2,-1,1,2],[-2,0,0,2],[-1,0,0,1]]
 *
 * Example 2:
 *
 * Input: nums = [2,2,2,2,2], target = 8
 * Output: [[2,2,2,2]]
 *
 *
 * Constraints:
 *
 * 	1 <= nums.length <= 200
 * 	-10^9 <= nums[i] <= 10^9
 * 	-10^9 <= target <= 10^9
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/4sum/
// discuss: https://leetcode.com/problems/4sum/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
use std::collections::HashMap;
impl Solution {
    pub fn four_sum(nums: Vec<i32>, target: i32) -> Vec<Vec<i32>> {
        let zeros = nums.iter().filter(|&&i| i == 0).count();
        let mut nums = nums;
        if zeros >= 5 {
            nums = nums.iter().filter(|&&i| i != 0).map(|&i| i).collect();
            nums.push(0);
            nums.push(0);
            nums.push(0);
            nums.push(0);
        }
        nums.sort();
        //println!("nums: {:?}", nums);
        let mut results: Vec<Vec<i32>> = vec![];
        let mut is_existed: HashMap<(i32, i32, i32, i32), i32> = HashMap::new();
        for i in 0..nums.len() {
            if i > 0 && nums[i] == nums[i - 1] {
                continue;
            }
            for j in i + 1..nums.len() {
                if j > i + 1 && nums[j] == nums[j - 1] {
                    continue;
                }
                let mut hashmap: HashMap<i32, i32> = HashMap::new();
                for k in j + 1..nums.len() {
                    /*
                    if k > j + 1 && nums[k] == nums[k - 1] {
                        continue;
                    }
                    */
                    let diff = target - nums[i] - nums[j] - nums[k];
                    if hashmap.contains_key(&nums[k]) {
                        let v = if diff < nums[k] {(nums[i], nums[j], diff, nums[k])} else {(nums[i], nums[j], nums[k], diff)};
                        if !is_existed.contains_key(&v) {
                            results.push(vec![v.0, v.1, v.2, v.3]);
                            is_existed.insert(v, 0);
                        }
                    } else {
                        hashmap.insert(diff, 0);
                    }
                    // println!("k: {}, results: {:?}", k, results);
                }
                // println!("j: {}, results: {:?}", j, results);
            }
            // println!("i: {}, results: {:?}", i, results);
        }
        results.reverse();
        results
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_18() {
        assert_eq!(
            Solution::four_sum(vec![1, 0, -1, 0, -2, 2], 0),
            vec![vec![-1, 0, 0, 1], vec![-2, 0, 0, 2], vec![-2, -1, 1, 2]]
        );
        assert_eq!(
            Solution::four_sum(vec![2,2,2,2,2], 8),
            vec![vec![2,2,2,2]]
        );
    }
}

```
