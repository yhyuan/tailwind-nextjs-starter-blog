---
title: Leetcode 1319 number of operations to make network connected
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 1319 number of operations to make network connected in Rust
---

## Solution in Rust

```rust
/**
 * [1319] Number of Operations to Make Network Connected
 *
 * There are n computers numbered from 0 to n-1 connected by ethernet cables connections forming a network where connections[i] = [a, b] represents a connection between computers a and b. Any computer can reach any other computer directly or indirectly through the network.
 * Given an initial computer network connections. You can extract certain cables between two directly connected computers, and place them between any pair of disconnected computers to make them directly connected. Return the minimum number of times you need to do this in order to make all the computers connected. If it's not possible, return -1.
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2020/01/02/sample_1_1677.png" style="width: 570px; height: 167px;" />
 *
 * Input: n = 4, connections = [[0,1],[0,2],[1,2]]
 * Output: 1
 * Explanation: Remove cable between computer 1 and 2 and place between computers 1 and 3.
 *
 * Example 2:
 * <img alt="" src="https://assets.leetcode.com/uploads/2020/01/02/sample_2_1677.png" style="width: 660px; height: 167px;" />
 *
 * Input: n = 6, connections = [[0,1],[0,2],[0,3],[1,2],[1,3]]
 * Output: 2
 *
 * Example 3:
 *
 * Input: n = 6, connections = [[0,1],[0,2],[0,3],[1,2]]
 * Output: -1
 * Explanation: There are not enough cables.
 *
 * Example 4:
 *
 * Input: n = 5, connections = [[0,1],[0,2],[3,4],[2,3]]
 * Output: 0
 *
 *
 * Constraints:
 *
 * 	1 <= n <= 10^5
 * 	1 <= connections.length <= min(n*(n-1)/2, 10^5)
 * 	connections[i].length == 2
 * 	0 <= connections[i][0], connections[i][1] < n
 * 	connections[i][0] != connections[i][1]
 * 	There are no repeated connections.
 * 	No two computers are connected by more than one cable.
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/number-of-operations-to-make-network-connected/
// discuss: https://leetcode.com/problems/number-of-operations-to-make-network-connected/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn dfs(graph: &Vec<Vec<usize>>, visited: &mut Vec<bool>, index: usize, connection_count: &mut i32) {
        visited[index] = true;
        for &i in (graph[index]).iter(){
            if visited[i] {
                //*back_edge_count += 1;
                // valid_connections[i.1] = false;
                continue;
            }
            *connection_count += 1;
            Self::dfs(graph, visited, i, connection_count);
        }
    }
    pub fn make_connected(n: i32, connections: Vec<Vec<i32>>) -> i32 {
        let n = n as usize;
        let mut graph: Vec<Vec<usize>> = vec![vec![]; n];
        for i in 0..connections.len() {
            let start = connections[i][0] as usize;
            let end = connections[i][1] as usize;
            graph[start].push(end);
            graph[end].push(start);
        }
        let mut visited = vec![false; n];
        let mut count = 0;
        let mut connection_count = 0;
        for i in 0..n {
            if visited[i] {
                continue;
            }
            count += 1;
            Self::dfs(&graph, &mut visited, i, &mut connection_count);
        }
        let extra = connections.len() as i32 - connection_count; //
        if extra + 1 >= count {
            return count - 1;
        }
        // println!("count: {}, connection_count: {}", count, connection_count);
        -1
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_1319() {
        assert_eq!(Solution::make_connected(4, vec![vec![0,1],vec![0,2],vec![1,2]]), 1);
        assert_eq!(Solution::make_connected(6, vec![vec![0,1],vec![0,2],vec![0,3],vec![1,2],vec![1,3]]), 2);
        assert_eq!(Solution::make_connected(6, vec![vec![0,1],vec![0,2],vec![1,2]]), -1);
    }
}

```
