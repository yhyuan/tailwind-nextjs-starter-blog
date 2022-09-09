---
title: Leetcode 1786 number of restricted paths from first to last node
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 1786 number of restricted paths from first to last node in Rust
---

## Solution in Rust

```rust
/**
 * [1786] Number of Restricted Paths From First to Last Node
 *
 * There is an undirected weighted connected graph. You are given a positive integer n which denotes that the graph has n nodes labeled from 1 to n, and an array edges where each edges[i] = [ui, vi, weighti] denotes that there is an edge between nodes ui and vi with weight equal to weighti.
 * A path from node start to node end is a sequence of nodes [z0, z1, z2, ..., zk] such that z0 = start and zk = end and there is an edge between zi and zi+1 where 0 <= i <= k-1.
 * The distance of a path is the sum of the weights on the edges of the path. Let distanceToLastNode(x) denote the shortest distance of a path between node n and node x. A restricted path is a path that also satisfies that distanceToLastNode(zi) > distanceToLastNode(zi+1) where 0 <= i <= k-1.
 * Return the number of restricted paths from node 1 to node n. Since that number may be too large, return it modulo 10^9 + 7.
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2021/02/17/restricted_paths_ex1.png" style="width: 351px; height: 341px;" />
 * Input: n = 5, edges = [[1,2,3],[1,3,3],[2,3,1],[1,4,2],[5,2,2],[3,5,1],[5,4,10]]
 * Output: 3
 * Explanation: Each circle contains the node number in black and its distanceToLastNode value in blue. The three restricted paths are:
 * 1) 1 --> 2 --> 5
 * 2) 1 --> 2 --> 3 --> 5
 * 3) 1 --> 3 --> 5
 *
 * Example 2:
 * <img alt="" src="https://assets.leetcode.com/uploads/2021/02/17/restricted_paths_ex22.png" style="width: 356px; height: 401px;" />
 * Input: n = 7, edges = [[1,3,1],[4,1,2],[7,3,4],[2,5,3],[5,6,1],[6,7,2],[7,5,3],[2,6,4]]
 * Output: 1
 * Explanation: Each circle contains the node number in black and its distanceToLastNode value in blue. The only restricted path is 1 --> 3 --> 7.
 *
 *
 * Constraints:
 *
 * 	1 <= n <= 2 * 10^4
 * 	n - 1 <= edges.length <= 4 * 10^4
 * 	edges[i].length == 3
 * 	1 <= ui, vi <= n
 * 	ui != vi
 * 	1 <= weighti <= 10^5
 * 	There is at most one edge between any two nodes.
 * 	There is at least one path between any two nodes.
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/number-of-restricted-paths-from-first-to-last-node/
// discuss: https://leetcode.com/problems/number-of-restricted-paths-from-first-to-last-node/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
use std::collections::{BinaryHeap, HashSet};

impl Solution {
    pub fn dfs(graph: &Vec<HashSet<(usize, usize)>>, distances: &Vec<i32>, memo: &mut Vec<i32>, index: usize) -> i32 {
        let n = distances.len();
        if memo[index] >= 0 {
            return memo[index];
        }
        let mut res = 0i32;
        for &(neighbor, weight) in graph[index].iter() {
            if distances[neighbor] >= distances[index] {
                continue;
            }
            res += if neighbor == n - 1 {
                1
            } else {
                Self::dfs(graph, distances, memo, neighbor)
            };
            res = res % 1000000007;
        }
        memo[index] = res;

        res
    }
    pub fn count_restricted_paths(n: i32, edges: Vec<Vec<i32>>) -> i32 {
        let n = n as usize;
        let mut graph: Vec<HashSet<(usize, usize)>> = vec![HashSet::new(); n];
        for i in 0..edges.len() {
            let from = edges[i][0] as usize - 1;
            let to = edges[i][1] as usize - 1;
            let weight = edges[i][2] as usize;
            graph[from].insert((to, weight));
            graph[to].insert((from, weight));
        }
        let mut distances = vec![i32::MAX; n];
        let mut visited: Vec<bool> = vec![false; n];
        let mut q: BinaryHeap<(i32, usize)> = BinaryHeap::new();
        q.push((0, n - 1));
        while !q.is_empty() {
            let (dist, v) = q.pop().unwrap();
            if visited[v] {
                continue;
            }
            visited[v] = true;
            distances[v] = -dist;
            for &(neighbor, weight) in graph[v].iter() {
                if !visited[neighbor] {
                    q.push((dist - weight as i32, neighbor));
                }
            }
        }
        let mut memo: Vec<i32> = vec![-1; n];
        Self::dfs(&graph, &distances, &mut memo, 0)
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_1786() {
        assert_eq!(Solution::count_restricted_paths(5, vec![vec![1,2,3],vec![1,3,3],vec![2,3,1],vec![1,4,2],vec![5,2,2],vec![3,5,1],vec![5,4,10]]), 3);
        assert_eq!(Solution::count_restricted_paths(7, vec![vec![1,3,1],vec![4,1,2],vec![7,3,4],vec![2,5,3],vec![5,6,1],vec![6,7,2],vec![7,5,3],vec![2,6,4]]), 1);
    }
}

```
