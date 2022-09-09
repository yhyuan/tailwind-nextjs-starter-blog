---
title: Leetcode 1462 course schedule iv
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 1462 course schedule iv in Rust
---

## Solution in Rust

```rust
/**
 * [1462] Course Schedule IV
 *
 * There are a total of numCourses courses you have to take, labeled from 0 to numCourses - 1. You are given an array prerequisites where prerequisites[i] = [ai, bi] indicates that you must take course ai first if you want to take course bi.
 *
 * 	For example, the pair [0, 1] indicates that you have to take course 0 before you can take course 1.
 *
 * Prerequisites can also be indirect. If course a is a prerequisite of course b, and course b is a prerequisite of course c, then course a is a prerequisite of course c.
 * You are also given an array queries where queries[j] = [uj, vj]. For the j^th query, you should answer whether course uj is a prerequisite of course vj or not.
 * Return a boolean array answer, where answer[j] is the answer to the j^th query.
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2021/05/01/courses4-1-graph.jpg" style="width: 222px; height: 62px;" />
 * Input: numCourses = 2, prerequisites = [[1,0]], queries = [[0,1],[1,0]]
 * Output: [false,true]
 * Explanation: The pair [1, 0] indicates that you have to take course 1 before you can take course 0.
 * Course 0 is not a prerequisite of course 1, but the opposite is true.
 *
 * Example 2:
 *
 * Input: numCourses = 2, prerequisites = [], queries = [[1,0],[0,1]]
 * Output: [false,false]
 * Explanation: There are no prerequisites, and each course is independent.
 *
 * Example 3:
 * <img alt="" src="https://assets.leetcode.com/uploads/2021/05/01/courses4-3-graph.jpg" style="width: 222px; height: 222px;" />
 * Input: numCourses = 3, prerequisites = [[1,2],[1,0],[2,0]], queries = [[1,0],[1,2]]
 * Output: [true,true]
 *
 *
 * Constraints:
 *
 * 	2 <= numCourses <= 100
 * 	0 <= prerequisites.length <= (numCourses * (numCourses - 1) / 2)
 * 	prerequisites[i].length == 2
 * 	0 <= ai, bi <= n - 1
 * 	ai != bi
 * 	All the pairs [ai, bi] are unique.
 * 	The prerequisites graph has no cycles.
 * 	1 <= queries.length <= 10^4
 * 	0 <= ui, vi <= n - 1
 * 	ui != vi
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/course-schedule-iv/
// discuss: https://leetcode.com/problems/course-schedule-iv/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
use std::collections::HashSet;
impl Solution {
    pub fn dfs(graph: &Vec<HashSet<usize>>, visited: &mut Vec<bool>, index: usize) {
        visited[index] = true;
        for &neighbor in graph[index].iter() {
            if !visited[neighbor] {
                Self::dfs(graph, visited, neighbor);
            }
        }
    }
    pub fn check_if_prerequisite(num_courses: i32, prerequisites: Vec<Vec<i32>>, queries: Vec<Vec<i32>>) -> Vec<bool> {
        let n = num_courses as usize;
        let mut graph: Vec<HashSet<usize>> = vec![HashSet::new(); n];
        let mut memo: HashSet<(usize, usize)> = HashSet::new();
        for i in 0..prerequisites.len() {
            let from = prerequisites[i][0] as usize;
            let to = prerequisites[i][1] as usize;
            graph[from].insert(to);
            memo.insert((from, to));
        }
        let mut res: Vec<bool> = vec![];
        for i in 0..queries.len() {
            let from = queries[i][0] as usize;
            let to = queries[i][1] as usize;
            if memo.contains(&(from, to)) {
                res.push(true);
            } else {
                let mut visited: Vec<bool> = vec![false; n];
                Self::dfs(&graph, &mut visited, from);
                for i in 0..n {
                    if visited[i] {
                        memo.insert((from, i));
                    }
                }
                res.push(memo.contains(&(from, to)));
            }
        }
        res
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_1462() {
        assert_eq!(Solution::check_if_prerequisite(2, vec![vec![1,0]], vec![vec![0,1],vec![1,0]]), vec![false, true]);
        assert_eq!(Solution::check_if_prerequisite(2, vec![], vec![vec![0,1],vec![1,0]]), vec![false, false]);
        assert_eq!(Solution::check_if_prerequisite(3, vec![vec![1,2], vec![1,0], vec![2,0]], vec![vec![1,0],vec![1,2]]), vec![true, true]);
        assert_eq!(Solution::check_if_prerequisite(4,
            vec![vec![2,3],vec![2,1],vec![0,3],vec![0,1]],
            vec![vec![0,1],vec![0,3],vec![2,3],vec![3,0],vec![2,0],vec![0,2]]),
            vec![true,true,true,false,false,false]);
    }
}

```
