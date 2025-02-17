---
title: Leetcode 0505 the maze ii
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0505 the maze ii in Rust
---

## Solution in Rust

```rust
/**
 505. The Maze II
Medium

1017

45

Add to List

Share
There is a ball in a maze with empty spaces (represented as 0) and walls (represented as 1). The ball can go through the empty spaces by rolling up, down, left or right, but it won't stop rolling until hitting a wall. When the ball stops, it could choose the next direction.

Given the m x n maze, the ball's start position and the destination, where start = [startrow, startcol] and destination = [destinationrow, destinationcol], return the shortest distance for the ball to stop at the destination. If the ball cannot stop at destination, return -1.

The distance is the number of empty spaces traveled by the ball from the start position (excluded) to the destination (included).

You may assume that the borders of the maze are all walls (see examples).



Example 1:


Input: maze = [[0,0,1,0,0],[0,0,0,0,0],[0,0,0,1,0],[1,1,0,1,1],[0,0,0,0,0]], start = [0,4], destination = [4,4]
Output: 12
Explanation: One possible way is : left -> down -> left -> down -> right -> down -> right.
The length of the path is 1 + 1 + 3 + 1 + 2 + 2 + 2 = 12.
Example 2:


Input: maze = [[0,0,1,0,0],[0,0,0,0,0],[0,0,0,1,0],[1,1,0,1,1],[0,0,0,0,0]], start = [0,4], destination = [3,2]
Output: -1
Explanation: There is no way for the ball to stop at the destination. Notice that you can pass through the destination but you cannot stop there.
Example 3:

Input: maze = [[0,0,0,0,0],[1,1,0,0,1],[0,0,0,0,0],[0,1,0,0,1],[0,1,0,0,0]], start = [4,3], destination = [0,1]
Output: -1


Constraints:

m == maze.length
n == maze[i].length
1 <= m, n <= 100
maze[i][j] is 0 or 1.
start.length == 2
destination.length == 2
0 <= startrow, destinationrow <= m
0 <= startcol, destinationcol <= n
Both the ball and the destination exist in an empty space, and they will not be in the same position initially.
The maze contains at least 2 empty spaces.
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/fibonacci-number/
// discuss: https://leetcode.com/problems/fibonacci-number/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
use std::collections::BinaryHeap;
impl Solution {
    pub fn find_neighbors(maze: &Vec<Vec<i32>>, coor: (usize, usize)) -> Vec<(usize, usize)> {
        let m = maze.len();
        let n = maze[0].len();
        let (x, y) = coor;
        let mut res: Vec<(usize, usize)> = vec![];
        let diffs = vec![(-1, 0), (1, 0), (0, -1), (0, 1)];
        let x = x as i32;
        let y = y as i32;
        for i in 0..diffs.len() {
            let diff = diffs[i];
            let mut u = x;
            let mut v = y;
            while u >= 0 && v >= 0 && u < m as i32 && v < n as i32 && maze[u as usize][v as usize] == 0 {
                u += diff.0;
                v += diff.1;
            }
            u -= diff.0;
            v -= diff.1;
            res.push((u as usize, v as usize));
        }
        res
    }
    pub fn shortest_distance(maze: Vec<Vec<i32>>, start: Vec<i32>, destination: Vec<i32>) -> i32 {
        let rows = maze.len();
        let cols = maze[0].len();
        let start = (start[0] as usize, start[1] as usize);
        let destination = (destination[0] as usize, destination[1] as usize);
        let mut heap: BinaryHeap<(i32, usize, usize)> = BinaryHeap::new();
        heap.push((0, start.0, start.1));
        let mut visited: Vec<Vec<bool>> = vec![vec![false; cols]; rows];
        for i in 0..rows {
            for j in 0..cols {
                if maze[i][j] == 1 {
                    visited[i][j] = true;
                }
            }
        }
        while !heap.is_empty() {
            let (dist, x, y) = heap.pop().unwrap();
            if visited[x][y] {
                continue;
            }
            visited[x][y] = true;
            if (x, y) == destination {
                return -dist;
            }
            for &neighbor in Self::find_neighbors(&maze, (x, y)).iter() {
                if visited[neighbor.0][neighbor.1] {
                    continue;
                }
                let dx = if neighbor.0 > x {neighbor.0 - x} else {x - neighbor.0};
                let dy = if neighbor.1 > y {neighbor.1 - y} else {y - neighbor.1};
                heap.push((dist - (dx + dy) as i32, neighbor.0, neighbor.1));
            }
        }
        -1
    }
}
// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_505() {
        assert_eq!(Solution::shortest_distance(vec![vec![0,0,1,0,0],vec![0,0,0,0,0],vec![0,0,0,1,0],vec![1,1,0,1,1],vec![0,0,0,0,0]], vec![0,4], vec![4, 4]), 12);
    }
}

```
