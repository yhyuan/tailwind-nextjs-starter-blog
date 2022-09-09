---
title: Leetcode 1514 path with maximum probability
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 1514 path with maximum probability in Rust
---

## Solution in Rust

```rust
/**
 * [1514] Path with Maximum Probability
 *
 * You are given an undirected weighted graph of n nodes (0-indexed), represented by an edge list where edges[i] = [a, b] is an undirected edge connecting the nodes a and b with a probability of success of traversing that edge succProb[i].
 *
 * Given two nodes start and end, find the path with the maximum probability of success to go from start to end and return its success probability.
 *
 * If there is no path from start to end, return 0. Your answer will be accepted if it differs from the correct answer by at most 1e-5.
 *
 *
 * Example 1:
 *
 * <img alt="" src="https://assets.leetcode.com/uploads/2019/09/20/1558_ex1.png" style="width: 187px; height: 186px;" />
 *
 *
 * Input: n = 3, edges = [[0,1],[1,2],[0,2]], succProb = [0.5,0.5,0.2], start = 0, end = 2
 * Output: 0.25000
 * Explanation: There are two paths from start to end, one having a probability of success = 0.2 and the other has 0.5 * 0.5 = 0.25.
 *
 *
 * Example 2:
 *
 * <img alt="" src="https://assets.leetcode.com/uploads/2019/09/20/1558_ex2.png" style="width: 189px; height: 186px;" />
 *
 *
 * Input: n = 3, edges = [[0,1],[1,2],[0,2]], succProb = [0.5,0.5,0.3], start = 0, end = 2
 * Output: 0.30000
 *
 *
 * Example 3:
 *
 * <img alt="" src="https://assets.leetcode.com/uploads/2019/09/20/1558_ex3.png" style="width: 215px; height: 191px;" />
 *
 *
 * Input: n = 3, edges = [[0,1]], succProb = [0.5], start = 0, end = 2
 * Output: 0.00000
 * Explanation: There is no path between 0 and 2.
 *
 *
 *
 * Constraints:
 *
 *
 * 	2 <= n <= 10^4
 * 	0 <= start, end < n
 * 	start != end
 * 	0 <= a, b < n
 * 	a != b
 * 	0 <= succProb.length == edges.length <= 2*10^4
 * 	0 <= succProb[i] <= 1
 * 	There is at most one edge between every two nodes.
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/path-with-maximum-probability/
// discuss: https://leetcode.com/problems/path-with-maximum-probability/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
use std::collections::{BinaryHeap, HashSet, HashMap};
impl Solution {
    pub fn max_probability(n: i32, edges: Vec<Vec<i32>>, succ_prob: Vec<f64>, start: i32, end: i32) -> f64 {
        let n = n as usize;
        let mut graph: Vec<HashSet<usize>> = vec![HashSet::new(); n];
        let mut prob_hashmap: HashMap<(usize, usize), f64> = HashMap::new();
        for i in 0..edges.len() {
            let start = edges[i][0] as usize;
            let end = edges[i][1] as usize;
            graph[start].insert(end);
            graph[end].insert(start);
            prob_hashmap.insert((start, end), succ_prob[i]);
            prob_hashmap.insert((end, start), succ_prob[i]);
        }
        let start = start as usize;
        let end = end as usize;
        let mut heap: BinaryHeap<(u128, usize)> = BinaryHeap::new();
        heap.push((10000000000, start));
        let mut visited: Vec<bool> = vec![false; n];
        let mut res = 1f64;
        let mut is_found = false;
        while !heap.is_empty() {
            let (prob, u) = heap.pop().unwrap();
            visited[u] = true;
            if u == end {
                res = (prob as f64) / 10000000000f64 ;
                is_found = true;
                break;
            }
            for &v in graph[u].iter() {
                if visited[v] {
                    continue;
                }
                let p = prob_hashmap.get(&(u, v)).unwrap() * prob as f64;
                heap.push((p as u128, v));
            }
        }
        if is_found {res} else {0f64}
    }
}
// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_1514() {
        assert_eq!(Solution::max_probability(5, vec![vec![1,4],vec![2,4],vec![0,4],vec![0,3],vec![0,2],vec![2,3]], vec![0.37,0.17,0.93,0.23,0.39,0.04], 3, 4), 0.21390);
        assert_eq!(Solution::max_probability(3, vec![vec![0,1],vec![1,2],vec![0,2]], vec![0.5,0.5,0.2], 0, 2), 0.2500);
        assert_eq!(Solution::max_probability(3, vec![vec![0,1],vec![1,2],vec![0,2]], vec![0.5,0.5,0.3], 0, 2), 0.3000);
        assert_eq!(Solution::max_probability(3, vec![vec![0,1]], vec![0.5], 0, 2), 0.0000);
    }
}

```
