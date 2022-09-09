---
title: Leetcode 0323 number of connected components in an undirected graph
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0323 number of connected components in an undirected graph in Rust
---

## Solution in Rust

```rust
/**
 * [323] Number of Connected Components in an Undirected Graph
 *
 * You have a graph of n nodes. You are given an integer n and an array edges where edges[i] = [ai, bi] indicates that there is an edge between ai and bi in the graph.

Return the number of connected components in the graph.



Example 1:


Input: n = 5, edges = [[0,1],[1,2],[3,4]]
Output: 2
Example 2:


Input: n = 5, edges = [[0,1],[1,2],[2,3],[3,4]]
Output: 1


Constraints:

1 <= n <= 2000
1 <= edges.length <= 5000
edges[i].length == 2
0 <= ai <= bi < n
ai != bi
There are no repeated edges.
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/coin-change/
// discuss: https://leetcode.com/problems/coin-change/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

use std::collections::HashSet;
impl Solution {
    pub fn build_graph(n: usize, edges: &Vec<Vec<i32>>) -> Vec<HashSet<usize>> {
        let mut result: Vec<HashSet<usize>> = vec![HashSet::new(); n as usize];
        for i in 0..edges.len() {
            let v1 = edges[i][0] as usize;
            let v2 = edges[i][1] as usize;
            result[v1].insert(v2);
            result[v2].insert(v1);
        }
        result
    }

    pub fn dfs(graph: &Vec<HashSet<usize>>, index: usize, visited: &mut Vec<bool>, stack: &mut Vec<usize>) {
        visited[index] = true;
        for &neighbor in graph[index].iter() {
            if visited[neighbor] {
                continue;
            }
            Self::dfs(graph, neighbor, visited, stack)
        }
        stack.push(index);
    }

    pub fn count_components(n: i32, edges: Vec<Vec<i32>>) -> i32 {
        let n = n as usize;
        let graph = Self::build_graph(n, &edges);
        let mut visited = vec![false; n];
        let mut stack: Vec<usize> = vec![];
        let mut count = 0i32;
        for i in 0..n {
            if visited[i] {
                continue;
            }
            count += 1;
            Self::dfs(&graph, i, &mut visited, &mut stack);
        }
        //println!("{:?}", stack);
        //let mut visited = vec![false; n];

        count
    }
}
// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_323() {
        assert_eq!(Solution::count_components(5, vec![vec![0,1],vec![1,2],vec![3,4]]), 2);
    }
}

```
