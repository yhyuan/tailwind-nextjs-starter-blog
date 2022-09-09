---
title: Leetcode 0253 meeting rooms ii
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0253 meeting rooms ii in Rust
---

## Solution in Rust

```rust
/**
253. Meeting Rooms II
Medium

5726

115

Add to List

Share
Given an array of meeting time intervals intervals where intervals[i] = [starti, endi], return the minimum number of conference rooms required.



Example 1:

Input: intervals = [[0,30],[5,10],[15,20]]
Output: 2
Example 2:

Input: intervals = [[7,10],[2,4]]
Output: 1


Constraints:

1 <= intervals.length <= 104
0 <= starti < endi <= 106 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/meeting-rooms-ii/
// discuss: https://leetcode.com/problems/meeting-rooms-ii/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
//use std::collections::HashMap;
impl Solution {
    pub fn min_meeting_rooms(intervals: Vec<Vec<i32>>) -> i32 {
        let mut nodes: Vec<(i32, i32)> = vec![];
        let n = intervals.len();
        for i in 0..n {
            nodes.push((intervals[i][0], 1)); // start
            nodes.push((intervals[i][1], 0)); // end
        }
        nodes.sort();
        let mut current_rooms = 0;
        let mut max_rooms = 0;
        for i in 0..nodes.len() {
            let (x, start_end) = nodes[i];
            if start_end == 1 {
                current_rooms += 1;
            } else {
                current_rooms -= 1;
            }
            max_rooms = i32::max(max_rooms, current_rooms);
        }
        max_rooms
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_253() {
        assert_eq!(Solution::min_meeting_rooms(vec![vec![0,30], vec![5,10], vec![15,20]]), 2);
        assert_eq!(Solution::min_meeting_rooms(vec![vec![7,10], vec![2,4]]), 1);
    }
}

```
