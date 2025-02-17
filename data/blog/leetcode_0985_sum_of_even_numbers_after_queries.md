---
title: Leetcode 0985 sum of even numbers after queries
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0985 sum of even numbers after queries in Rust
---

## Solution in Rust

```rust
/**
 * [985] Sum of Even Numbers After Queries
 *
 * You are given an integer array nums and an array queries where queries[i] = [vali, indexi].
 * For each query i, first, apply nums[indexi] = nums[indexi] + vali, then print the sum of the even values of nums.
 * Return an integer array answer where answer[i] is the answer to the i^th query.
 *
 * Example 1:
 *
 * Input: nums = [1,2,3,4], queries = [[1,0],[-3,1],[-4,0],[2,3]]
 * Output: [8,6,2,4]
 * Explanation: At the beginning, the array is [1,2,3,4].
 * After adding 1 to nums[0], the array is [2,2,3,4], and the sum of even values is 2 + 2 + 4 = 8.
 * After adding -3 to nums[1], the array is [2,-1,3,4], and the sum of even values is 2 + 4 = 6.
 * After adding -4 to nums[0], the array is [-2,-1,3,4], and the sum of even values is -2 + 4 = 2.
 * After adding 2 to nums[3], the array is [-2,-1,3,6], and the sum of even values is -2 + 6 = 4.
 *
 * Example 2:
 *
 * Input: nums = [1], queries = [[4,0]]
 * Output: [0]
 *
 *
 * Constraints:
 *
 * 	1 <= nums.length <= 10^4
 * 	-10^4 <= nums[i] <= 10^4
 * 	1 <= queries.length <= 10^4
 * 	-10^4 <= vali <= 10^4
 * 	0 <= indexi < nums.length
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/sum-of-even-numbers-after-queries/
// discuss: https://leetcode.com/problems/sum-of-even-numbers-after-queries/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn sum_even_after_queries(nums: Vec<i32>, queries: Vec<Vec<i32>>) -> Vec<i32> {
        let mut nums = nums;
        let mut total: i32 = nums.iter().filter(|&&v| v % 2 == 0).sum();
        let mut results: Vec<i32> = Vec::with_capacity(nums.len());
        for query in queries.iter() {
            let val = query[0];
            let index = query[1] as usize;
            let num = nums[index];
            nums[index] = num + val;
            let is_new_even = nums[index] % 2 == 0;
            let is_old_even = num % 2 == 0;
            if is_new_even & is_old_even {
                total += val;
            } else if is_new_even & !is_old_even {
                total += nums[index];
            } else if !is_new_even & is_old_even {
                total -= num;
            }
            results.push(total);
        }
        results
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_985() {
        assert_eq!(Solution::sum_even_after_queries(vec![1,2,3,4], vec![vec![1,0],vec![-3,1],vec![-4,0],vec![2,3]]), vec![8,6,2,4]);
    }
}

```
