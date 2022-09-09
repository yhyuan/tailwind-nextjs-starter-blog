---
title: Leetcode 0847 shortest path visiting all nodes
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0847 shortest path visiting all nodes in Rust
---

## Solution in Rust

```rust
/**
 * [847] Shortest Path Visiting All Nodes
 *
 * You have an undirected, connected graph of n nodes labeled from 0 to n - 1. You are given an array graph where graph[i] is a list of all the nodes connected with node i by an edge.
 * Return the length of the shortest path that visits every node. You may start and stop at any node, you may revisit nodes multiple times, and you may reuse edges.
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2021/05/12/shortest1-graph.jpg" style="width: 222px; height: 183px;" />
 * Input: graph = [[1,2,3],[0],[0],[0]]
 * Output: 4
 * Explanation: One possible path is [1,0,2,0,3]
 *
 * Example 2:
 * <img alt="" src="https://assets.leetcode.com/uploads/2021/05/12/shortest2-graph.jpg" style="width: 382px; height: 222px;" />
 * Input: graph = [[1],[0,2,4],[1,3,4],[2],[1,2]]
 * Output: 4
 * Explanation: One possible path is [0,1,4,2,3]
 *
 *
 * Constraints:
 *
 * 	n == graph.length
 * 	1 <= n <= 12
 * 	0 <= graph[i].length < n
 * 	graph[i] does not contain i.
 * 	If graph[a] contains b, then graph[b] contains a.
 * 	The input graph is always connected.
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/shortest-path-visiting-all-nodes/
// discuss: https://leetcode.com/problems/shortest-path-visiting-all-nodes/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
use std::collections::VecDeque;
use std::collections::HashSet;
impl Solution {
    pub fn find_neighbors(state: u16, cur: u8, visited: &HashSet<(u16, u8)>, graph: &Vec<Vec<i32>>)-> Vec<(u16, u8)> {
        let mut result: Vec<(u16, u8)> = Vec::new();
        let n = graph.len() as u8;
        let neighbors = &graph[cur as usize];
        for i in 0..neighbors.len() {
            let neighbor = neighbors[i] as u8;
            let next_state = state | 1u16 << neighbor;
            if visited.contains(&(next_state, neighbor)) {
                continue;
            }
            result.push((next_state, neighbor));
        }
        result
    }
    pub fn shortest_path_length(graph: Vec<Vec<i32>>) -> i32 {
        let n = graph.len() as u8;
        let mut q: VecDeque<(u16, u8)> = VecDeque::new();
        let mut visited: HashSet<(u16, u8)> = HashSet::new();
        for i in 0..n {
            let state = 1u16 << i;
            q.push_back((state, i));
            visited.insert((state, i));
        }
        let target = 2_i32.pow(n as u32) as u16 - 1;
        //println!("target: {}", target);
        let mut step = 0;
        while !q.is_empty() {
            let mut next_q: VecDeque<(u16, u8)> = VecDeque::new();
            while !q.is_empty() {
                let (state, cur) = q.pop_front().unwrap();
                //println!("state: {}, cur: {}, step: {}", state, cur, step);
                if state == target {
                    return step;
                }
                let neighbors = Self::find_neighbors(state, cur, &visited, &graph);
                for i in 0..neighbors.len() {
                    visited.insert(neighbors[i]);
                    next_q.push_back(neighbors[i]);
                }
            }
            q = next_q;
            step += 1;
        }
        0
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_847() {
        assert_eq!(Solution::shortest_path_length(vec![vec![1,2,3],vec![0],vec![0],vec![0]]), 4);
        assert_eq!(Solution::shortest_path_length(vec![vec![1],vec![0,2,4],vec![1,3,4],vec![2],vec![1,2]]), 4);
    }
}

```
