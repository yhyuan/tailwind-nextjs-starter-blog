---
title: Leetcode 1376 time needed to inform all employees
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 1376 time needed to inform all employees in Rust
---

## Solution in Rust

```rust
/**
 * [1376] Time Needed to Inform All Employees
 *
 * A company has n employees with a unique ID for each employee from 0 to n - 1. The head of the company is the one with headID.
 * Each employee has one direct manager given in the manager array where manager[i] is the direct manager of the i-th employee, manager[headID] = -1. Also, it is guaranteed that the subordination relationships have a tree structure.
 * The head of the company wants to inform all the company employees of an urgent piece of news. He will inform his direct subordinates, and they will inform their subordinates, and so on until all employees know about the urgent news.
 * The i-th employee needs informTime[i] minutes to inform all of his direct subordinates (i.e., After informTime[i] minutes, all his direct subordinates can start spreading the news).
 * Return the number of minutes needed to inform all the employees about the urgent news.
 *
 * Example 1:
 *
 * Input: n = 1, headID = 0, manager = [-1], informTime = [0]
 * Output: 0
 * Explanation: The head of the company is the only employee in the company.
 *
 * Example 2:
 * <img alt="" src="https://assets.leetcode.com/uploads/2020/02/27/graph.png" style="width: 404px; height: 174px;" />
 * Input: n = 6, headID = 2, manager = [2,2,-1,2,2,2], informTime = [0,0,1,0,0,0]
 * Output: 1
 * Explanation: The head of the company with id = 2 is the direct manager of all the employees in the company and needs 1 minute to inform them all.
 * The tree structure of the employees in the company is shown.
 *
 * Example 3:
 * <img alt="" src="https://assets.leetcode.com/uploads/2020/02/28/1730_example_3_5.PNG" style="width: 568px; height: 432px;" />
 * Input: n = 7, headID = 6, manager = [1,2,3,4,5,6,-1], informTime = [0,6,5,4,3,2,1]
 * Output: 21
 * Explanation: The head has id = 6. He will inform employee with id = 5 in 1 minute.
 * The employee with id = 5 will inform the employee with id = 4 in 2 minutes.
 * The employee with id = 4 will inform the employee with id = 3 in 3 minutes.
 * The employee with id = 3 will inform the employee with id = 2 in 4 minutes.
 * The employee with id = 2 will inform the employee with id = 1 in 5 minutes.
 * The employee with id = 1 will inform the employee with id = 0 in 6 minutes.
 * Needed time = 1 + 2 + 3 + 4 + 5 + 6 = 21.
 *
 * Example 4:
 *
 * Input: n = 15, headID = 0, manager = [-1,0,0,1,1,2,2,3,3,4,4,5,5,6,6], informTime = [1,1,1,1,1,1,1,0,0,0,0,0,0,0,0]
 * Output: 3
 * Explanation: The first minute the head will inform employees 1 and 2.
 * The second minute they will inform employees 3, 4, 5 and 6.
 * The third minute they will inform the rest of employees.
 *
 * Example 5:
 *
 * Input: n = 4, headID = 2, manager = [3,3,-1,2], informTime = [0,0,162,914]
 * Output: 1076
 *
 *
 * Constraints:
 *
 * 	1 <= n <= 10^5
 * 	0 <= headID < n
 * 	manager.length == n
 * 	0 <= manager[i] < n
 * 	manager[headID] == -1
 * 	informTime.length == n
 * 	0 <= informTime[i] <= 1000
 * 	informTime[i] == 0 if employee i has no subordinates.
 * 	It is guaranteed that all the employees can be informed.
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/time-needed-to-inform-all-employees/
// discuss: https://leetcode.com/problems/time-needed-to-inform-all-employees/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
use std::collections::VecDeque;
impl Solution {
    pub fn num_of_minutes(n: i32, head_id: i32, manager: Vec<i32>, inform_time: Vec<i32>) -> i32 {
        let n = n as usize;
        let mut graph: Vec<Vec<usize>> = vec![vec![]; n];
        for i in 0..n {
            if  manager[i] == -1 {
                continue;
            }
            graph[ manager[i] as usize].push(i);
        }
        let mut q: VecDeque<(usize, usize)> = VecDeque::new();
        q.push_back((head_id as usize, 0));
        let mut result = i32::MIN;
        while !q.is_empty() {
            let (index, time) = q.pop_front().unwrap();
            result = i32::max(time as i32, result);
            let inform_time_index = inform_time[index] as usize;
            for i in 0..graph[index].len() {
                let v = graph[index][i];
                q.push_back((v, time + inform_time_index));
            }
        }
        result
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_1376() {
        assert_eq!(Solution::num_of_minutes(6, 2, vec![2,2,-1,2,2,2], vec![0,0,1,0,0,0]), 1);
    }
}

```
