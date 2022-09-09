---
title: Leetcode 0986 interval list intersections
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0986 interval list intersections in Rust
---

## Solution in Rust

```rust
/**
 * [986] Interval List Intersections
 *
 * You are given two lists of closed intervals, firstList and secondList, where firstList[i] = [starti, endi] and secondList[j] = [startj, endj]. Each list of intervals is pairwise disjoint and in sorted order.
 * Return the intersection of these two interval lists.
 * A closed interval [a, b] (with a < b) denotes the set of real numbers x with a <= x <= b.
 * The intersection of two closed intervals is a set of real numbers that are either empty or represented as a closed interval. For example, the intersection of [1, 3] and [2, 4] is [2, 3].
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2019/01/30/interval1.png" style="width: 700px; height: 194px;" />
 * Input: firstList = [[0,2],[5,10],[13,23],[24,25]], secondList = [[1,5],[8,12],[15,24],[25,26]]
 * Output: [[1,2],[5,5],[8,10],[15,23],[24,24],[25,25]]
 *
 * Example 2:
 *
 * Input: firstList = [[1,3],[5,9]], secondList = []
 * Output: []
 *
 * Example 3:
 *
 * Input: firstList = [], secondList = [[4,8],[10,12]]
 * Output: []
 *
 * Example 4:
 *
 * Input: firstList = [[1,7]], secondList = [[3,10]]
 * Output: [[3,7]]
 *
 *
 * Constraints:
 *
 * 	0 <= firstList.length, secondList.length <= 1000
 * 	firstList.length + secondList.length >= 1
 * 	0 <= starti < endi <= 10^9
 * 	endi < starti+1
 * 	0 <= startj < endj <= 10^9
 * 	endj < startj+1
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/interval-list-intersections/
// discuss: https://leetcode.com/problems/interval-list-intersections/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn interval_intersection_helper(first_list: &Vec<Vec<i32>>, k1: usize, second_list: &Vec<Vec<i32>>, k2: usize) -> Vec<Vec<i32>> {
        let n1 = first_list.len();
        let n2 = second_list.len();
        if k1 == n1 || k2 == n2 {
            return vec![];
        }
        let first_interval = &first_list[k1];
        let second_interval = &second_list[k2];
        if first_interval[0] <= second_interval[0] {
            if second_interval[0] > first_interval[1]  {
                Self::interval_intersection_helper(first_list, k1 + 1, second_list, k2)
            } else {// second_interval[0] <= first_interval[1] and second_interval[0] >= first_interval[0]
                if second_interval[1] <= first_interval[1] {
                    let mut pre_results = Self::interval_intersection_helper(first_list, k1, second_list, k2 + 1);
                    pre_results.insert(0, vec![second_interval[0], second_interval[1]]);
                    pre_results
                } else {
                    let mut pre_results = Self::interval_intersection_helper(first_list, k1 + 1, second_list, k2);
                    pre_results.insert(0, vec![second_interval[0], first_interval[1]]);
                    pre_results
                }
            }
        } else {
            if first_interval[0] > second_interval[1]  {
                Self::interval_intersection_helper(first_list, k1, second_list, k2 + 1)
            } else {// second_interval[0] <= first_interval[1] and second_interval[0] >= first_interval[0]
                if first_interval[1] <= second_interval[1] {
                    let mut pre_results = Self::interval_intersection_helper(first_list, k1 + 1, second_list, k2);
                    pre_results.insert(0, vec![first_interval[0], first_interval[1]]);
                    pre_results
                } else {
                    let mut pre_results = Self::interval_intersection_helper(first_list, k1, second_list, k2 + 1);
                    pre_results.insert(0, vec![first_interval[0], second_interval[1]]);
                    pre_results
                }
            }
        }
    }
    pub fn interval_intersection(first_list: Vec<Vec<i32>>, second_list: Vec<Vec<i32>>) -> Vec<Vec<i32>> {
        Self::interval_intersection_helper(&first_list, 0, &second_list, 0)
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_986() {
        assert_eq!(Solution::interval_intersection(vec![vec![0,2],vec![5,10],vec![13,23],vec![24,25]],
            vec![vec![1,5],vec![8,12],vec![15,24],vec![25,26]]),
            vec![vec![1,2],vec![5,5],vec![8,10],vec![15,23],vec![24,24],vec![25,25]]);
    }
}

```
