---
title: Leetcode 0047 permutations ii
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0047 permutations ii in Rust
---

## Solution in Rust

```rust
/**
 * [47] Permutations II
 *
 * Given a collection of numbers, nums, that might contain duplicates, return all possible unique permutations in any order.
 *
 * Example 1:
 *
 * Input: nums = [1,1,2]
 * Output:
 * [[1,1,2],
 *  [1,2,1],
 *  [2,1,1]]
 *
 * Example 2:
 *
 * Input: nums = [1,2,3]
 * Output: [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
 *
 *
 * Constraints:
 *
 * 	1 <= nums.length <= 8
 * 	-10 <= nums[i] <= 10
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/permutations-ii/
// discuss: https://leetcode.com/problems/permutations-ii/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn permute_unique_helper(nums: &Vec<i32>) -> Vec<Vec<i32>> {
        if nums.len() == 1 {
            return vec![nums.clone()];
        }
        let mut results: Vec<Vec<i32>> =vec![];
        let mut pre_val = i32::MIN;
        for i in 0..nums.len() {
            let num = nums[i];
            if pre_val == num {
                continue;
            }
            let new_nums: Vec<i32> = (0..nums.len()).into_iter()
                .filter(|&k| k != i)
                .map(|k| nums[k])
                .collect();
            let mut single_results = Solution::permute_unique_helper(&new_nums);
            for result in single_results.iter_mut() {
                result.insert(0, num);
                results.push(result.clone());
            }
            pre_val = num;
        }
        results
    }

    pub fn permute_unique(nums: Vec<i32>) -> Vec<Vec<i32>> {
        let mut nums = nums;
        nums.sort();
        Solution::permute_unique_helper(&nums)
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_47() {
        assert_eq!(
            Solution::permute_unique(vec![1, 1, 2]),
            vec![vec![1, 1, 2], vec![1, 2, 1], vec![2, 1, 1], ]
        );
        assert_eq!(Solution::permute_unique(vec![1, 1, 1]), vec![vec![1, 1, 1],]);
        assert_eq!(
            Solution::permute_unique(vec![1, 1, 1, 2]),
            vec![
                vec![1, 1, 1, 2],
                vec![1, 1, 2, 1],
                vec![1, 2, 1, 1],
                vec![2, 1, 1, 1],
            ]
        );
    }
}

```
