---
title: Leetcode 0056 merge intervals
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0056 merge intervals in Rust
---

## Solution in Rust

```rust
/**
 * [56] Merge Intervals
 *
 * Given an array of intervals where intervals[i] = [starti, endi], merge all overlapping intervals, and return an array of the non-overlapping intervals that cover all the intervals in the input.
 *
 * Example 1:
 *
 * Input: intervals = [[1,3],[2,6],[8,10],[15,18]]
 * Output: [[1,6],[8,10],[15,18]]
 * Explanation: Since intervals [1,3] and [2,6] overlaps, merge them into [1,6].
 *
 * Example 2:
 *
 * Input: intervals = [[1,4],[4,5]]
 * Output: [[1,5]]
 * Explanation: Intervals [1,4] and [4,5] are considered overlapping.
 *
 *
 * Constraints:
 *
 * 	1 <= intervals.length <= 10^4
 * 	intervals[i].length == 2
 * 	0 <= starti <= endi <= 10^4
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/merge-intervals/
// discuss: https://leetcode.com/problems/merge-intervals/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
#[derive(Debug, PartialEq, Eq)]
pub struct Interval {
    pub start: i32,
    pub end: i32,
}

impl  Interval {
    #[inline]
    pub fn new(start: i32, end: i32) -> Self {
        Interval{start: start, end: end}
    }
    #[inline]
    pub fn clone(interval: &Interval) -> Self {
        Interval{start: interval.start, end: interval.end}
    }
    pub fn to_vect(&self) -> Vec<i32> {
        vec![self.start, self.end]
    }
}
/*
impl Solution {
    pub fn merge(intervals: Vec<Vec<i32>>) -> Vec<Vec<i32>> {
        let mut intervals: Vec<Interval> = intervals.iter()
            .map(|inteval| Interval::new(inteval[0], inteval[1])).collect();
        intervals.sort_by(|a, b| a.start.cmp(&b.start));
        let mut results: Vec<Interval> = vec![];
        let mut pre_interval = Interval::clone(&intervals[0]);
        for i in 1..intervals.len() {
            if pre_interval.end >= intervals[i].start {
                pre_interval = Interval::new(pre_interval.start, std::cmp::max(pre_interval.end, intervals[i].end));
            } else {
                results.push(Interval::clone(&pre_interval));
                pre_interval = Interval::clone(&intervals[i])
            }
        }
        results.push(Interval::clone(&pre_interval));
        results.iter().map(|inter| inter.to_vect()).collect()
    }
}
*/
impl Solution {
    pub fn merge(intervals: Vec<Vec<i32>>) -> Vec<Vec<i32>> {
        let mut intervals = intervals;
        let n = intervals.len();
        intervals.sort_by(|a, b| a[0].partial_cmp(&b[0]).unwrap());
        if intervals.len() == 1 {
            return intervals.clone();
        }
        let mut res: Vec<Vec<i32>> = vec![];
        let mut k = 0;
        loop {
            let start = intervals[k][0];
            let end = intervals[k][1];
            // If it is the last remain one.
            if k == n - 1 {
                res.push(vec![start, end]);
                break;
            }
            // Find the interval index from the second one whose beginning is larger than the end the first one.
            let index = (k + 1..n).into_iter().find(|&i| intervals[i][0] > end);
            if index.is_none() {
                // If we can not find it, it means we can merge all remaining intervals to one and break out.
                let new_end = intervals.iter().map(|v| v[1]).max().unwrap();
                res.push(vec![start, new_end]);
                break;
            } else {
                // otherwise, merge from k to index - 1. If the merged one is not intersecting with the one at index,
                // push the merged one to res and increase k to index. If the merged one is intersect with the interval at index,
                // put the merged one at index - 1 and increase k to index - 1.
                let index = index.unwrap();
                let new_end = (k..index).into_iter().map(|i| intervals[i][1]).max().unwrap();
                if new_end < intervals[index][0] {
                    res.push(vec![start, new_end]);
                    k = index;
                } else {
                    intervals[index - 1] = vec![start, new_end];
                    k = index - 1;
                }
            }
        }
        res
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_56() {
        assert_eq!(
            Solution::merge(vec![
                vec![1,3],vec![2,6],vec![6, 7], vec![8,10],vec![15,18]
            ]),
            vec![
                vec![1, 7],
                vec![8, 10],
                vec![15, 18]
            ]
        );
    }
}

```
