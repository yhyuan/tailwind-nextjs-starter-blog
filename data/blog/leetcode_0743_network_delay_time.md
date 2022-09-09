---
title: Leetcode 0743 network delay time
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0743 network delay time in Rust
---

## Solution in Rust

```rust
/**
 * [743] Network Delay Time
 *
 * You are given a network of n nodes, labeled from 1 to n. You are also given times, a list of travel times as directed edges times[i] = (ui, vi, wi), where ui is the source node, vi is the target node, and wi is the time it takes for a signal to travel from source to target.
 * We will send a signal from a given node k. Return the time it takes for all the n nodes to receive the signal. If it is impossible for all the n nodes to receive the signal, return -1.
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2019/05/23/931_example_1.png" style="width: 217px; height: 239px;" />
 * Input: times = [[2,1,1],[2,3,1],[3,4,1]], n = 4, k = 2
 * Output: 2
 *
 * Example 2:
 *
 * Input: times = [[1,2,1]], n = 2, k = 1
 * Output: 1
 *
 * Example 3:
 *
 * Input: times = [[1,2,1]], n = 2, k = 2
 * Output: -1
 *
 *
 * Constraints:
 *
 * 	1 <= k <= n <= 100
 * 	1 <= times.length <= 6000
 * 	times[i].length == 3
 * 	1 <= ui, vi <= n
 * 	ui != vi
 * 	0 <= wi <= 100
 * 	All the pairs (ui, vi) are unique. (i.e., no multiple edges.)
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/network-delay-time/
// discuss: https://leetcode.com/problems/network-delay-time/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

use std::collections::{HashSet, HashMap, BinaryHeap};
impl Solution {
    pub fn network_delay_time(times: Vec<Vec<i32>>, n: i32, k: i32) -> i32 {
        let n = n as usize;
        let mut graph: Vec<HashSet<usize>> = vec![HashSet::new(); n];
        let mut times_map: HashMap<(usize, usize), i32> = HashMap::new();
        for i in 0..times.len() {
            let start = times[i][0] as usize - 1;
            let end   = times[i][1] as usize - 1;
            let time =  times[i][2];
            times_map.insert((start, end), time);
            graph[start].insert(end);
        }
        let mut visited: Vec<bool> = vec![false; n];
        //visited[k as usize - 1] = true;
        let mut q: BinaryHeap<(i32, usize)> = BinaryHeap::new();
        q.push((0, k as usize - 1));
        let mut res = i32::MIN;
        while !q.is_empty() {
            let (dist, u) = q.pop().unwrap();
            if visited[u] {
                continue;
            }
            visited[u] = true;
            //println!("dist: {}, u: {}", dist, u);
            res = i32::max(res, -dist);
            for &neighbor in graph[u].iter() {
                if visited[neighbor] {
                    continue;
                }
                //visited[neighbor] = true;
                let edge = *times_map.get(&(u, neighbor)).unwrap();
                q.push((dist - edge, neighbor));
            }
        }
        for i in 0..n {
            if !visited[i] {
                return -1;
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
    fn test_743() {
        assert_eq!(Solution::network_delay_time(vec![vec![1,2,1],vec![2,3,2],vec![1,3,4]], 3, 1), 3);
        assert_eq!(Solution::network_delay_time(vec![vec![1,2,1],vec![2,3,2],vec![1,3,2]], 3, 1), 2);
        assert_eq!(Solution::network_delay_time(vec![vec![2,1,1],vec![2,3,1],vec![3,4,1]], 4, 2), 2);
    }
}

```
