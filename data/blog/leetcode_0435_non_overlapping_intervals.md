---
title: Leetcode 0435 non overlapping intervals
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0435 non overlapping intervals in Rust
---

## Solution in Rust

```rust
/**
 * [435] Non-overlapping Intervals
 *
 * Given an array of intervals intervals where intervals[i] = [starti, endi], return the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping.
 *
 * Example 1:
 *
 * Input: intervals = [[1,2],[2,3],[3,4],[1,3]]
 * Output: 1
 * Explanation: [1,3] can be removed and the rest of the intervals are non-overlapping.
 *
 * Example 2:
 *
 * Input: intervals = [[1,2],[1,2],[1,2]]
 * Output: 2
 * Explanation: You need to remove two [1,2] to make the rest of the intervals non-overlapping.
 *
 * Example 3:
 *
 * Input: intervals = [[1,2],[2,3]]
 * Output: 0
 * Explanation: You don't need to remove any of the intervals since they're already non-overlapping.
 *
 *
 * Constraints:
 *
 * 	1 <= intervals.length <= 10^5
 * 	intervals[i].length == 2
 * 	-5 * 10^4 <= starti < endi <= 5 * 10^4
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/non-overlapping-intervals/
// discuss: https://leetcode.com/problems/non-overlapping-intervals/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
impl Solution {
    pub fn overlapping(pre_interval: &Vec<i32>, post_interval: &Vec<i32>) -> bool {
        pre_interval[1] > post_interval[0]
    }
    pub fn erase_overlap_intervals(intervals: Vec<Vec<i32>>) -> i32 {
        let n = intervals.len();
        if n == 0 {
            return 0i32;
        }
        let mut intervals = intervals;
        intervals.sort_by(|a, b| a[0].partial_cmp(&b[0]).unwrap());
        let mut dp: Vec<i32> = vec![0; n];
        // println!("intervals{:?}", intervals);
        //stores the maximum number of valid intervals that can be included in the final list if the intervals upto the i^{th}i
// th  interval only are considered, including itself.
        dp[0] = 1;
        //let mut res = 1;
        for i in 1..n {
            let mut max = 0;
            for j in (0..=i - 1).rev() {
                if dp[j] <= max {
                    break;
                }
                if !Self::overlapping(&intervals[j], &intervals[i]) {
                    max = dp[j];
                }
            }
            dp[i] = max + 1;
        }
        //println!("dp: {:?}", dp);
        //n as i32 - dp.into_iter().max().unwrap()
        n as i32 - dp[n - 1]
    }
}
// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_435() {
        assert_eq!(Solution::erase_overlap_intervals(vec![vec![1,2],vec![2,3],vec![3,4],vec![1,3]]), 1);
    }
}

```
