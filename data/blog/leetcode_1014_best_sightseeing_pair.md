---
title: Leetcode 1014 best sightseeing pair
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 1014 best sightseeing pair in Rust
---

## Solution in Rust

```rust
/**
 * [1014] Best Sightseeing Pair
 *
 * You are given an integer array values where values[i] represents the value of the i^th sightseeing spot. Two sightseeing spots i and j have a distance j - i between them.
 * The score of a pair (i < j) of sightseeing spots is values[i] + values[j] + i - j: the sum of the values of the sightseeing spots, minus the distance between them.
 * Return the maximum score of a pair of sightseeing spots.
 *
 * Example 1:
 *
 * Input: values = [8,1,5,2,6]
 * Output: 11
 * Explanation: i = 0, j = 2, values[i] + values[j] + i - j = 8 + 5 + 0 - 2 = 11
 *
 * Example 2:
 *
 * Input: values = [1,2]
 * Output: 2
 *
 *
 * Constraints:
 *
 * 	2 <= values.length <= 5 * 10^4
 * 	1 <= values[i] <= 1000
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/best-sightseeing-pair/
// discuss: https://leetcode.com/problems/best-sightseeing-pair/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn max_score_sightseeing_pair(values: Vec<i32>) -> i32 {
        // f(i) = values[j] + j + max(values[i] + i)  i < j.
        // values[i] + values[j] + i - j => values[i] + i and values[j] - j
        let n = values.len();
        let mut dp = values[0] + 0i32;
        let mut max_val = i32::MIN;
        for i in 1..n {
            let val = values[i] - i as i32 + dp;
            max_val = i32::max(max_val, val);
            dp = i32::max(dp, values[i] + i as i32);
        }
        max_val
    }
}
// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_1014() {
        assert_eq!(Solution::max_score_sightseeing_pair(vec![8,1,5,2,6]), 11);
        assert_eq!(Solution::max_score_sightseeing_pair(vec![1,2]), 2);
    }
}

```
