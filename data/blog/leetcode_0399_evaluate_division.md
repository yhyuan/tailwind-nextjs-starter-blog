---
title: Leetcode 0399 evaluate division
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0399 evaluate division in Rust
---

## Solution in Rust

```rust
/**
 * [399] Evaluate Division
 *
 * You are given an array of variable pairs equations and an array of real numbers values, where equations[i] = [Ai, Bi] and values[i] represent the equation Ai / Bi = values[i]. Each Ai or Bi is a string that represents a single variable.
 * You are also given some queries, where queries[j] = [Cj, Dj] represents the j^th query where you must find the answer for Cj / Dj = ?.
 * Return the answers to all queries. If a single answer cannot be determined, return -1.0.
 * Note: The input is always valid. You may assume that evaluating the queries will not result in division by zero and that there is no contradiction.
 *
 * Example 1:
 *
 * Input: equations = [["a","b"],["b","c"]], values = [2.0,3.0], queries = [["a","c"],["b","a"],["a","e"],["a","a"],["x","x"]]
 * Output: [6.00000,0.50000,-1.00000,1.00000,-1.00000]
 * Explanation:
 * Given: a / b = 2.0, b / c = 3.0
 * queries are: a / c = ?, b / a = ?, a / e = ?, a / a = ?, x / x = ?
 * return: [6.0, 0.5, -1.0, 1.0, -1.0 ]
 *
 * Example 2:
 *
 * Input: equations = [["a","b"],["b","c"],["bc","cd"]], values = [1.5,2.5,5.0], queries = [["a","c"],["c","b"],["bc","cd"],["cd","bc"]]
 * Output: [3.75000,0.40000,5.00000,0.20000]
 *
 * Example 3:
 *
 * Input: equations = [["a","b"]], values = [0.5], queries = [["a","b"],["b","a"],["a","c"],["x","y"]]
 * Output: [0.50000,2.00000,-1.00000,-1.00000]
 *
 *
 * Constraints:
 *
 * 	1 <= equations.length <= 20
 * 	equations[i].length == 2
 * 	1 <= Ai.length, Bi.length <= 5
 * 	values.length == equations.length
 * 	0.0 < values[i] <= 20.0
 * 	1 <= queries.length <= 20
 * 	queries[i].length == 2
 * 	1 <= Cj.length, Dj.length <= 5
 * 	Ai, Bi, Cj, Dj consist of lower case English letters and digits.
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/evaluate-division/
// discuss: https://leetcode.com/problems/evaluate-division/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
use std::collections::HashMap;
pub struct Graph {
    links: Vec<HashMap<usize, f64>>,
    nodes: HashMap<String, usize>,
}
impl Graph {
    pub fn new(equations: Vec<Vec<String>>, values: Vec<f64>) -> Self {
        let mut nodes: HashMap<String, usize> = HashMap::new();
        for equation in equations.iter() {
            let node1 = &equation[0];
            let node2 = &equation[1];
            if !nodes.contains_key(node1) {
                let n = nodes.len();
                nodes.insert(node1.clone(), n);
            }
            if !nodes.contains_key(node2) {
                let n = nodes.len();
                nodes.insert(node2.clone(), n);
            }
        }
        let mut links: Vec<HashMap<usize, f64>> = vec![HashMap::new(); nodes.len()];
        for (i, equation ) in equations.iter().enumerate() {
            let node1 = &equation[0];
            let node2 = &equation[1];
            let index1 = nodes[node1];
            let index2 = nodes[node2];
            let val = values[i];
            links[index1].insert(index2, val);
            links[index2].insert(index1, 1f64/val);
        }
        Self {nodes, links}
    }
    pub fn dfs(&self, start: usize, end: usize, marked: &mut Vec<bool>, result: f64) -> Option<f64> {
        if start == end {
            return Some(result);
        }
        marked[start] = true;
        let neighbors = &self.links[start];
        for (&id, &weight) in neighbors.iter() {
            if !marked[id] {
                let res = self.dfs(id, end, marked, result * weight);
                if res.is_some() {
                    return res;
                }
            }
        }
        None
    }
    pub fn query(&self, query: &Vec<String>) -> f64 {
        let start = &query[0];
        let end = &query[1];
        if !self.nodes.contains_key(start) || !self.nodes.contains_key(end) {
            return -1f64;
        }
        let start_index = self.nodes[start];
        let end_index = self.nodes[end];
        if start_index == end_index {
            return 1f64;
        }
        let mut marked: Vec<bool> = vec![false; self.links.len()];
        let result = self.dfs(start_index, end_index, &mut marked, 1f64);
        if result.is_none() {-1f64} else {result.unwrap()}
    }
}
impl Solution {
    pub fn calc_equation(equations: Vec<Vec<String>>, values: Vec<f64>, queries: Vec<Vec<String>>) -> Vec<f64> {
        let graph: Graph = Graph::new(equations, values);
        queries.iter().map(|query| graph.query(query)).collect::<Vec<f64>>()
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_399() {
        assert_eq!(Solution::calc_equation(vec![
            vec!["a".to_string(),"b".to_string()],
            vec!["b".to_string(),"c".to_string()]],
            vec![2.0,3.0],
            vec![
                vec!["a".to_string(),"c".to_string()],
                vec!["b".to_string(),"a".to_string()],
                vec!["a".to_string(),"e".to_string()],
                vec!["a".to_string(),"a".to_string()],
                vec!["x".to_string(),"x".to_string()]]),
            vec![6.00000,0.50000,-1.00000,1.00000,-1.00000]);

        assert_eq!(Solution::calc_equation(vec![
            vec!["a".to_string(),"b".to_string()],
            vec!["b".to_string(),"c".to_string()],
            vec!["bc".to_string(),"cd".to_string()],
            ],
            vec![1.5,2.5,5.0],
            vec![
                vec!["a".to_string(),"c".to_string()],
                vec!["c".to_string(),"b".to_string()],
                vec!["bc".to_string(),"cd".to_string()],
                vec!["cd".to_string(),"bc".to_string()]]),
            vec![3.75000,0.40000,5.00000,0.200000]);

            assert_eq!(Solution::calc_equation(vec![
                vec!["a".to_string(),"b".to_string()],
                ],
                vec![0.5],
                vec![
                    vec!["a".to_string(),"b".to_string()],
                    vec!["b".to_string(),"a".to_string()],
                    vec!["a".to_string(),"c".to_string()],
                    vec!["x".to_string(),"y".to_string()]]),
                vec![0.5000,2.0000,-1.00000,-1.00000]);

        }
}

```
