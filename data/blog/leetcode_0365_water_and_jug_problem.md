---
title: Leetcode 0365 water and jug problem
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0365 water and jug problem in Rust
---

## Solution in Rust

```rust
/**
 * [365] Water and Jug Problem
 *
 * You are given two jugs with capacities jug1Capacity and jug2Capacity liters. There is an infinite amount of water supply available. Determine whether it is possible to measure exactly targetCapacity liters using these two jugs.
 * If targetCapacity liters of water are measurable, you must have targetCapacity liters of water contained within one or both buckets by the end.
 * Operations allowed:
 *
 * 	Fill any of the jugs with water.
 * 	Empty any of the jugs.
 * 	Pour water from one jug into another till the other jug is completely full, or the first jug itself is empty.
 *
 *
 * Example 1:
 *
 * Input: jug1Capacity = 3, jug2Capacity = 5, targetCapacity = 4
 * Output: true
 * Explanation: The famous <a href="https://www.youtube.com/watch?v=BVtQNK_ZUJg&amp;ab_channel=notnek01" target="_blank">Die Hard</a> example
 *
 * Example 2:
 *
 * Input: jug1Capacity = 2, jug2Capacity = 6, targetCapacity = 5
 * Output: false
 *
 * Example 3:
 *
 * Input: jug1Capacity = 1, jug2Capacity = 2, targetCapacity = 3
 * Output: true
 *
 *
 * Constraints:
 *
 * 	1 <= jug1Capacity, jug2Capacity, targetCapacity <= 10^6
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/water-and-jug-problem/
// discuss: https://leetcode.com/problems/water-and-jug-problem/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
use std::collections::HashSet;
impl Solution {
    pub fn dfs(visited: &mut HashSet<(i32, i32)>, jug1_capacity: i32, jug2_capacity: i32, target_capacity: i32, jug1: i32, jug2: i32) -> bool {
        if jug1 == target_capacity || jug2 == target_capacity || jug1 + jug2 == target_capacity {
            return true;
        }
        visited.insert((jug1, jug2));
        let mut next_states: Vec<(i32, i32)> = vec![];
        if jug1 != jug1_capacity {
            next_states.push((jug1_capacity, jug2));
        }
        if jug2 != jug2_capacity {
            next_states.push((jug1, jug2_capacity));
        }
        if jug1 < jug1_capacity {
            if jug1 + jug2 > jug1_capacity {
                next_states.push((jug1_capacity, jug1 + jug2 - jug1_capacity));
            } else  {
                next_states.push((jug1 + jug2, 0));
            }
        }
        if jug2 < jug2_capacity {
            if jug1 + jug2 > jug2_capacity {
                next_states.push((jug1 + jug2 - jug2_capacity, jug2_capacity));
            } else  {
                next_states.push((0, jug1 + jug2));
            }
        }
        if jug1 > 0 {
            next_states.push((0, jug2));
        }
        if jug2 > 0 {
            next_states.push((jug1, 0));
        }
        for i in 0..next_states.len() {
            let (v1, v2) = next_states[i];
            if visited.contains(&(v1, v2)) {
                continue;
            }
            if Self::dfs(visited, jug1_capacity, jug2_capacity, target_capacity, v1, v2) {
                return true;
            }
        }
        false
    }
    pub fn can_measure_water(jug1_capacity: i32, jug2_capacity: i32, target_capacity: i32) -> bool {
        if target_capacity > jug1_capacity + jug2_capacity {
            return false;
        }
        if target_capacity == jug1_capacity + jug2_capacity || target_capacity == jug1_capacity || target_capacity == jug2_capacity {
            return true;
        }
        let mut visited: HashSet<(i32, i32)> = HashSet::new();
        visited.insert((jug1_capacity, jug2_capacity));
        Self::dfs(&mut visited, jug1_capacity, jug2_capacity, target_capacity, 0, 0)
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_365() {
        assert_eq!(Solution::can_measure_water(3, 5, 4), true);
        assert_eq!(Solution::can_measure_water(2, 6, 5), false);
        assert_eq!(Solution::can_measure_water(1, 2, 3), true);
    }
}

```
