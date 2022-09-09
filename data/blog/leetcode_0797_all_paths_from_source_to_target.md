---
title: Leetcode 0797 all paths from source to target
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0797 all paths from source to target in Rust
---

## Solution in Rust

```rust
/**
 * [797] All Paths From Source to Target
 *
 * Given a directed acyclic graph (DAG) of n nodes labeled from 0 to n - 1, find all possible paths from node 0 to node n - 1 and return them in any order.
 * The graph is given as follows: graph[i] is a list of all nodes you can visit from node i (i.e., there is a directed edge from node i to node graph[i][j]).
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2020/09/28/all_1.jpg" style="width: 242px; height: 242px;" />
 * Input: graph = [[1,2],[3],[3],[]]
 * Output: [[0,1,3],[0,2,3]]
 * Explanation: There are two paths: 0 -> 1 -> 3 and 0 -> 2 -> 3.
 *
 * Example 2:
 * <img alt="" src="https://assets.leetcode.com/uploads/2020/09/28/all_2.jpg" style="width: 423px; height: 301px;" />
 * Input: graph = [[4,3,1],[3,2,4],[3],[4],[]]
 * Output: [[0,4],[0,3,4],[0,1,3,4],[0,1,2,3,4],[0,1,4]]
 *
 * Example 3:
 *
 * Input: graph = [[1],[]]
 * Output: [[0,1]]
 *
 * Example 4:
 *
 * Input: graph = [[1,2,3],[2],[3],[]]
 * Output: [[0,1,2,3],[0,2,3],[0,3]]
 *
 * Example 5:
 *
 * Input: graph = [[1,3],[2],[3],[]]
 * Output: [[0,1,2,3],[0,3]]
 *
 *
 * Constraints:
 *
 * 	n == graph.length
 * 	2 <= n <= 15
 * 	0 <= graph[i][j] < n
 * 	graph[i][j] != i (i.e., there will be no self-loops).
 * 	All the elements of graph[i] are unique.
 * 	The input graph is guaranteed to be a DAG.
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/all-paths-from-source-to-target/
// discuss: https://leetcode.com/problems/all-paths-from-source-to-target/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn dfs(u: i32, path: &mut Vec<i32>, paths: &mut Vec<Vec<i32>>, graph: &Vec<Vec<i32>>, n: usize) {
        path.push(u);
        if u == n as i32 - 1 {
            paths.push(path.clone());
        } else {
            for &v in graph[u as usize].iter() {
                Self::dfs(v, path, paths, graph, n);
            }
        }
        path.pop();
    }

    pub fn all_paths_source_target(graph: Vec<Vec<i32>>) -> Vec<Vec<i32>> {
        let n = graph.len();
        let mut res: Vec<Vec<i32>> = vec![];
        let mut path: Vec<i32> = vec![];
        Self::dfs(0, &mut path, &mut res, &graph, n);
        res
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_797() {
        assert_eq!(Solution::all_paths_source_target(vec![vec![1,2],vec![3],vec![3],vec![]]), vec![vec![0,1,3],vec![0,2,3]]);
        assert_eq!(Solution::all_paths_source_target(vec![vec![4,3,1],vec![3,2,4],vec![3],vec![4],vec![]]), vec![vec![0,4],vec![0,3,4],vec![0,1,3,4],vec![0,1,2,3,4],vec![0,1,4]]);
        assert_eq!(Solution::all_paths_source_target(vec![vec![1],vec![]]), vec![vec![0,1]]);
        assert_eq!(Solution::all_paths_source_target(vec![vec![1,2,3],vec![2],vec![3],vec![]]), vec![vec![0,1,2,3],vec![0,2,3],vec![0,3]]);
        assert_eq!(Solution::all_paths_source_target(vec![vec![1,3],vec![2],vec![3],vec![]]), vec![vec![0,1,2,3],vec![0,3]]);
    }
}

```
