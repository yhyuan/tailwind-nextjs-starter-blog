---
title: Leetcode 1466 reorder routes to make all paths lead to the city zero
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 1466 reorder routes to make all paths lead to the city zero in Rust
---

## Solution in Rust

```rust
/**
 * [1466] Reorder Routes to Make All Paths Lead to the City Zero
 *
 * There are n cities numbered from 0 to n - 1 and n - 1 roads such that there is only one way to travel between two different cities (this network form a tree). Last year, The ministry of transport decided to orient the roads in one direction because they are too narrow.
 * Roads are represented by connections where connections[i] = [ai, bi] represents a road from city ai to city bi.
 * This year, there will be a big event in the capital (city 0), and many people want to travel to this city.
 * Your task consists of reorienting some roads such that each city can visit the city 0. Return the minimum number of edges changed.
 * It's guaranteed that each city can reach city 0 after reorder.
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2020/05/13/sample_1_1819.png" style="width: 311px; height: 189px;" />
 * Input: n = 6, connections = [[0,1],[1,3],[2,3],[4,0],[4,5]]
 * Output: 3
 * Explanation: Change the direction of edges show in red such that each node can reach the node 0 (capital).
 *
 * Example 2:
 * <img alt="" src="https://assets.leetcode.com/uploads/2020/05/13/sample_2_1819.png" style="width: 509px; height: 79px;" />
 * Input: n = 5, connections = [[1,0],[1,2],[3,2],[3,4]]
 * Output: 2
 * Explanation: Change the direction of edges show in red such that each node can reach the node 0 (capital).
 *
 * Example 3:
 *
 * Input: n = 3, connections = [[1,0],[2,0]]
 * Output: 0
 *
 *
 * Constraints:
 *
 * 	2 <= n <= 5 * 10^4
 * 	connections.length == n - 1
 * 	connections[i].length == 2
 * 	0 <= ai, bi <= n - 1
 * 	ai != bi
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/reorder-routes-to-make-all-paths-lead-to-the-city-zero/
// discuss: https://leetcode.com/problems/reorder-routes-to-make-all-paths-lead-to-the-city-zero/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
use std::collections::HashSet;
use std::collections::VecDeque;

impl Solution {
    pub fn min_reorder(n: i32, connections: Vec<Vec<i32>>) -> i32 {
        let n = n as usize;
        let mut graph: Vec<Vec<usize>> = vec![vec![]; n];
        let mut reversed_edges: HashSet<(usize, usize)> = HashSet::new();
        for i in 0..connections.len() {
            let start = connections[i][0] as usize;
            let end   = connections[i][1] as usize;
            graph[start].push(end);
            graph[end].push(start);
            reversed_edges.insert((start, end));
        }
        let mut visited: Vec<bool> = vec![false; n];
        let mut q: VecDeque<(usize, usize)> = VecDeque::new();
        q.push_back((0, 0));
        let mut count = 0;
        while !q.is_empty() {
            let (index, steps) = q.pop_front().unwrap();
            visited[index] = true;
            for i in 0..graph[index].len() {
                let neighbor = graph[index][i];
                if visited[neighbor] {
                    continue;
                }
                if reversed_edges.contains(&(index, neighbor)) {
                    count += 1;
                }
                q.push_back((neighbor, steps + 1));
            }
        }
        count
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_1466() {
        assert_eq!(Solution::min_reorder(6, vec![vec![0,1],vec![1,3],vec![2,3],vec![4,0],vec![4,5]]), 3);
    }
}

```
