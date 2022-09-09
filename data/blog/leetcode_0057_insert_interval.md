---
title: Leetcode 0057 insert interval
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0057 insert interval in Rust
---

## Solution in Rust

```rust
/**
 * [57] Insert Interval
 *
 * You are given an array of non-overlapping intervals intervals where intervals[i] = [starti, endi] represent the start and the end of the i^th interval and intervals is sorted in ascending order by starti. You are also given an interval newInterval = [start, end] that represents the start and end of another interval.
 * Insert newInterval into intervals such that intervals is still sorted in ascending order by starti and intervals still does not have any overlapping intervals (merge overlapping intervals if necessary).
 * Return intervals after the insertion.
 *
 * Example 1:
 *
 * Input: intervals = [[1,3],[6,9]], newInterval = [2,5]
 * Output: [[1,5],[6,9]]
 *
 * Example 2:
 *
 * Input: intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]
 * Output: [[1,2],[3,10],[12,16]]
 * Explanation: Because the new interval [4,8] overlaps with [3,5],[6,7],[8,10].
 * Example 3:
 *
 * Input: intervals = [], newInterval = [5,7]
 * Output: [[5,7]]
 *
 * Example 4:
 *
 * Input: intervals = [[1,5]], newInterval = [2,3]
 * Output: [[1,5]]
 *
 * Example 5:
 *
 * Input: intervals = [[1,5]], newInterval = [2,7]
 * Output: [[1,7]]
 *
 *
 * Constraints:
 *
 * 	0 <= intervals.length <= 10^4
 * 	intervals[i].length == 2
 * 	0 <= starti <= endi <= 10^5
 * 	intervals is sorted by starti in ascending order.
 * 	newInterval.length == 2
 * 	0 <= start <= end <= 10^5
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/insert-interval/
// discuss: https://leetcode.com/problems/insert-interval/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn insert(intervals: Vec<Vec<i32>>, new_interval: Vec<i32>) -> Vec<Vec<i32>> {
        if intervals.len() == 0 {
            return vec![new_interval];
        }
        if intervals[0][0] > new_interval[1] { // if the end of new interval is smaller than all interval's start.
            return [&(vec![new_interval])[..], &intervals[..]].concat();
        }
        if intervals[intervals.len() - 1][1] < new_interval[0] { // if the start of new interval is larger than all interval's end.
            return [&intervals[..], &(vec![new_interval])[..]].concat();
        }
        let start = match (0..intervals.len()).into_iter().filter(|&i| intervals[i][1] < new_interval[0]).max() {
            Some(v) => {v + 1}
            None => {0}
        };
        let end = match (0..intervals.len()).into_iter().filter(|&i| intervals[i][0] > new_interval[1]).min() {
            Some(v) => {v - 1}
            None => {intervals.len() - 1}
        };
        let start_val = i32::min(intervals[start][0], new_interval[0]);
        let end_val = i32::max(intervals[end][1], new_interval[1]);
        let middle = vec![vec![start_val, end_val]];

        if start == 0 {
            [&middle, &intervals[end + 1..]].concat()
        } else if end == intervals.len() - 1 {
            [&intervals[..start], &middle].concat()
        } else {
            [&intervals[..start], &middle, &intervals[end + 1..]].concat()
        }
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_57() {

        assert_eq!(
            Solution::insert(
                vec![vec![1, 3], vec![6, 9]],
                vec![2, 5]
            ),
            vec![vec![1, 5], vec![6, 9]]
        );


        assert_eq!(
            Solution::insert(
                vec![
                    vec![1, 2],
                    vec![3, 5],
                    vec![6, 7],
                    vec![8, 10],
                    vec![12, 16]
                ],
                vec![4, 8]
            ),
            vec![
                vec![1, 2],
                vec![3, 10],
                vec![12, 16]
            ]
        );


        assert_eq!(
            Solution::insert(vec![vec![3, 4]], vec![1, 2]),
            vec![vec![1, 2], vec![3, 4]]
        );

        assert_eq!(
            Solution::insert(vec![vec![1, 2]], vec![3, 4]),
            vec![vec![1, 2], vec![3, 4]]
        );
        assert_eq!(
            Solution::insert(vec![vec![1, 2]], vec![2, 3]),
            vec![vec![1, 3]]
        );
        assert_eq!(
            Solution::insert(
                vec![vec![1, 2], vec![3, 4]],
                vec![0, 6]
            ),
            vec![vec![0, 6]]
        );

    }
}

```
