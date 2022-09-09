---
title: Leetcode 1192 critical connections in a network
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 1192 critical connections in a network in Rust
---

## Solution in Rust

```rust
/**
 * [1192] Critical Connections in a Network
 *
 * There are n servers numbered from 0 to n - 1 connected by undirected server-to-server connections forming a network where connections[i] = [ai, bi] represents a connection between servers ai and bi. Any server can reach other servers directly or indirectly through the network.
 * A critical connection is a connection that, if removed, will make some servers unable to reach some other server.
 * Return all critical connections in the network in any order.
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2019/09/03/1537_ex1_2.png" style="width: 198px; height: 248px;" />
 * Input: n = 4, connections = [[0,1],[1,2],[2,0],[1,3]]
 * Output: [[1,3]]
 * Explanation: [[3,1]] is also accepted.
 *
 * Example 2:
 *
 * Input: n = 2, connections = [[0,1]]
 * Output: [[0,1]]
 *
 *
 * Constraints:
 *
 * 	2 <= n <= 10^5
 * 	n - 1 <= connections.length <= 10^5
 * 	0 <= ai, bi <= n - 1
 * 	ai != bi
 * 	There are no repeated connections.
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/critical-connections-in-a-network/
// discuss: https://leetcode.com/problems/critical-connections-in-a-network/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
use std::collections::HashSet;
impl Solution {
    pub fn dfs(u: usize, graph: &Vec<Vec<usize>>, visited: &mut Vec<bool>, parent: &mut Vec<usize>, disc: &mut Vec<usize>, low: &mut Vec<usize>, res: &mut Vec<Vec<i32>>, time: &mut usize) {
        visited[u] = true;
        disc[u] = *time;
        low[u] = *time;
        *time += 1;
        for &v in graph[u].iter() {
            if !visited[v] {
                parent[v] = u;
                Self::dfs(v, graph, visited, parent, disc, low, res, time);
                low[u] = usize::min(low[u], low[v]);
                if low[v] > disc[u] {
                    res.push(vec![u as i32, v as i32]);
                }
            } else {
                if v != parent[u] {
                    low[u] = usize::min(low[u], disc[v]);
                }
            }
        }
        *time -= 1;
    }
    pub fn critical_connections(n: i32, connections: Vec<Vec<i32>>) -> Vec<Vec<i32>> {
        let n = n as usize;
        let mut graph: Vec<Vec<usize>> = vec![vec![]; n];
        for i in 0..connections.len() {
            let start = connections[i][0] as usize;
            let end   = connections[i][1] as usize;
            graph[start].push(end);
            graph[end].push(start);
        }
        let mut visited: Vec<bool> = vec![false; n];
        let mut parent: Vec<usize> = vec![usize::MAX; n];
        let mut disc: Vec<usize> = vec![usize::MAX; n];
        let mut low: Vec<usize> = vec![usize::MAX; n];
        let mut res: Vec<Vec<i32>> = vec![];
        let mut time = 1usize;
        for i in 0..n {
            if !visited[i] {
                Self::dfs(i, &graph, &mut visited, &mut parent, &mut disc, &mut low, &mut res, &mut time);
            }
        }
        /*
        println!("visited: {:?}", visited);
        println!("parent: {:?}", parent);
        println!("disc: {:?}", disc);
        println!("low: {:?}", low);
        println!("res: {:?}", res);
        */
        res
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_1192() {
        assert_eq!(Solution::critical_connections(4, vec![vec![0,1],vec![1,2],vec![2,0],vec![1,3]]), vec![vec![1,3]]);
        assert_eq!(Solution::critical_connections(2, vec![vec![0,1]]), vec![vec![0,1]]);
    }
}

```
