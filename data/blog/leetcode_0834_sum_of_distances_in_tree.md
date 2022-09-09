---
title: Leetcode 0834 sum of distances in tree
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0834 sum of distances in tree in Rust
---

## Solution in Rust

```rust
/**
 * [834] Sum of Distances in Tree
 *
 * There is an undirected connected tree with n nodes labeled from 0 to n - 1 and n - 1 edges.
 * You are given the integer n and the array edges where edges[i] = [ai, bi] indicates that there is an edge between nodes ai and bi in the tree.
 * Return an array answer of length n where answer[i] is the sum of the distances between the i^th node in the tree and all other nodes.
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2021/07/23/lc-sumdist1.jpg" style="width: 304px; height: 224px;" />
 * Input: n = 6, edges = [[0,1],[0,2],[2,3],[2,4],[2,5]]
 * Output: [8,12,6,10,10,10]
 * Explanation: The tree is shown above.
 * We can see that dist(0,1) + dist(0,2) + dist(0,3) + dist(0,4) + dist(0,5)
 * equals 1 + 1 + 2 + 2 + 2 = 8.
 * Hence, answer[0] = 8, and so on.
 *
 * Example 2:
 * <img alt="" src="https://assets.leetcode.com/uploads/2021/07/23/lc-sumdist2.jpg" style="width: 64px; height: 65px;" />
 * Input: n = 1, edges = []
 * Output: [0]
 *
 * Example 3:
 * <img alt="" src="https://assets.leetcode.com/uploads/2021/07/23/lc-sumdist3.jpg" style="width: 144px; height: 145px;" />
 * Input: n = 2, edges = [[1,0]]
 * Output: [1,1]
 *
 *
 * Constraints:
 *
 * 	1 <= n <= 3 * 10^4
 * 	edges.length == n - 1
 * 	edges[i].length == 2
 * 	0 <= ai, bi < n
 * 	ai != bi
 * 	The given input represents a valid tree.
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/sum-of-distances-in-tree/
// discuss: https://leetcode.com/problems/sum-of-distances-in-tree/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
use std::collections::HashSet;
impl Solution {
    pub fn dfs(graph: &Vec<HashSet<usize>>, count: &mut Vec<i32>, ans: &mut Vec<i32>, index: usize, parent: usize) {
        for &child in graph[index].iter() {
            if child != parent {
                Self::dfs(graph, count, ans, child, index);
                count[index] += count[child];
                ans[index] += ans[child] + count[child];
            }
        }
    }
    pub fn dfs2(graph: &Vec<HashSet<usize>>, count: &Vec<i32>, ans: &mut Vec<i32>, index: usize, parent: usize) {
        let n = graph.len();
        for &child in graph[index].iter() {
            if child != parent {
                ans[child] = ans[index] - count[child] + n as i32 - count[child];
                Self::dfs2(graph, count, ans, child, index);
            }
        }
    }
    pub fn sum_of_distances_in_tree(n: i32, edges: Vec<Vec<i32>>) -> Vec<i32> {
        let n = n as usize;
        let mut graph: Vec<HashSet<usize>> = vec![HashSet::new(); n];
        for i in 0..edges.len() {
            let from = edges[i][0] as usize;
            let to = edges[i][1] as usize;
            graph[from].insert(to);
            graph[to].insert(from);
        }
        let mut count = vec![1i32; n];
        let mut ans: Vec<i32> = vec![0i32; n];
        Self::dfs(&graph, &mut count, &mut ans, 0, usize::MAX);
        Self::dfs2(&graph, &count, &mut ans, 0, usize::MAX);
        ans
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_834() {
        assert_eq!(Solution::sum_of_distances_in_tree(6, vec![vec![0,1],vec![0,2],vec![2,3],vec![2,4],vec![2,5]]), vec![8,12,6,10,10,10]);
        assert_eq!(Solution::sum_of_distances_in_tree(1, vec![]), vec![0]);
        assert_eq!(Solution::sum_of_distances_in_tree(2, vec![vec![1, 0]]), vec![1, 1]);
    }
}

```
