---
title: Leetcode 0684 redundant connection
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0684 redundant connection in Rust
---

## Solution in Rust

```rust
/**
 * [684] Redundant Connection
 *
 * In this problem, a tree is an undirected graph that is connected and has no cycles.
 * You are given a graph that started as a tree with n nodes labeled from 1 to n, with one additional edge added. The added edge has two different vertices chosen from 1 to n, and was not an edge that already existed. The graph is represented as an array edges of length n where edges[i] = [ai, bi] indicates that there is an edge between nodes ai and bi in the graph.
 * Return an edge that can be removed so that the resulting graph is a tree of n nodes. If there are multiple answers, return the answer that occurs last in the input.
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2021/05/02/reduntant1-1-graph.jpg" style="width: 222px; height: 222px;" />
 * Input: edges = [[1,2],[1,3],[2,3]]
 * Output: [2,3]
 *
 * Example 2:
 * <img alt="" src="https://assets.leetcode.com/uploads/2021/05/02/reduntant1-2-graph.jpg" style="width: 382px; height: 222px;" />
 * Input: edges = [[1,2],[2,3],[3,4],[1,4],[1,5]]
 * Output: [1,4]
 *
 *
 * Constraints:
 *
 * 	n == edges.length
 * 	3 <= n <= 1000
 * 	edges[i].length == 2
 * 	1 <= ai < bi <= edges.length
 * 	ai != bi
 * 	There are no repeated edges.
 * 	The given graph is connected.
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/redundant-connection/
// discuss: https://leetcode.com/problems/redundant-connection/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
pub struct DisjointSet {
    parents: Vec<usize>,
    ranks: Vec<usize>
}

impl DisjointSet {
    pub fn new(n: usize) -> DisjointSet {
        let parents = (0..n).into_iter().collect::<Vec<_>>();
        let ranks = vec![1; n];
        DisjointSet {
            parents,
            ranks
        }
    }

    pub fn find(&self, i: usize) -> usize {
        let mut parent = i;
        while parent != self.parents[parent] {
            parent = self.parents[parent];
        }
        parent
    }

    pub fn union(&mut self, i: usize, j: usize) -> bool {
        let i_p = self.find(i);
        let j_p = self.find(j);
        if i_p == j_p {
            return false;
        }
        if self.ranks[i_p] < self.ranks[j_p] {
            self.parents[i_p] = self.parents[j_p];
        } else if self.ranks[i_p] > self.ranks[j_p] {
            self.parents[j_p] = self.parents[i_p];
        } else {
            self.parents[j_p] = self.parents[i_p];
            self.ranks[i_p] += 1;
        }
        true
    }
}
impl Solution {
    pub fn find_redundant_connection(edges: Vec<Vec<i32>>) -> Vec<i32> {
        let n = edges.len();
        let mut disjoint_set = DisjointSet::new(n);
        let mut result: Vec<Vec<i32>> = vec![];
        for i in 0..edges.len() {
            let start = edges[i][0] as usize - 1;
            let end = edges[i][1] as usize - 1;
            let r = disjoint_set.union(start, end);
            if !r {
                result.push(edges[i].clone());
            }
        }
        result.last().unwrap().clone()
        //vec![]
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_684() {
        assert_eq!(Solution::find_redundant_connection(vec![vec![1,2],vec![1,3],vec![2,3]]), vec![2, 3]);
        assert_eq!(Solution::find_redundant_connection(vec![vec![1,2],vec![2,3],vec![3,4],vec![1,4],vec![1,5]]), vec![1, 4]);
    }
}

```
