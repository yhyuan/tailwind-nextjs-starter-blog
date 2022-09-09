---
title: Leetcode 1334 find the city with the smallest number of neighbors at a threshold distance
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 1334 find the city with the smallest number of neighbors at a threshold distance in Rust
---

## Solution in Rust

```rust
/**
 * [1334] Find the City With the Smallest Number of Neighbors at a Threshold Distance
 *
 * There are n cities numbered from 0 to n-1. Given the array edges where edges[i] = [fromi, toi, weighti] represents a bidirectional and weighted edge between cities fromi and toi, and given the integer distanceThreshold.
 * Return the city with the smallest number of cities that are reachable through some path and whose distance is at most distanceThreshold, If there are multiple such cities, return the city with the greatest number.
 * Notice that the distance of a path connecting cities i and j is equal to the sum of the edges' weights along that path.
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2020/01/16/find_the_city_01.png" style="width: 300px; height: 225px;" />
 * Input: n = 4, edges = [[0,1,3],[1,2,1],[1,3,4],[2,3,1]], distanceThreshold = 4
 * Output: 3
 * Explanation: The figure above describes the graph.
 * The neighboring cities at a distanceThreshold = 4 for each city are:
 * City 0 -> [City 1, City 2]
 * City 1 -> [City 0, City 2, City 3]
 * City 2 -> [City 0, City 1, City 3]
 * City 3 -> [City 1, City 2]
 * Cities 0 and 3 have 2 neighboring cities at a distanceThreshold = 4, but we have to return city 3 since it has the greatest number.
 *
 * Example 2:
 * <img alt="" src="https://assets.leetcode.com/uploads/2020/01/16/find_the_city_02.png" style="width: 300px; height: 225px;" />
 * Input: n = 5, edges = [[0,1,2],[0,4,8],[1,2,3],[1,4,2],[2,3,1],[3,4,1]], distanceThreshold = 2
 * Output: 0
 * Explanation: The figure above describes the graph.
 * The neighboring cities at a distanceThreshold = 2 for each city are:
 * City 0 -> [City 1]
 * City 1 -> [City 0, City 4]
 * City 2 -> [City 3, City 4]
 * City 3 -> [City 2, City 4]
 * City 4 -> [City 1, City 2, City 3]
 * The city 0 has 1 neighboring city at a distanceThreshold = 2.
 *
 *
 * Constraints:
 *
 * 	2 <= n <= 100
 * 	1 <= edges.length <= n * (n - 1) / 2
 * 	edges[i].length == 3
 * 	0 <= fromi < toi < n
 * 	1 <= weighti, distanceThreshold <= 10^4
 * 	All pairs (fromi, toi) are distinct.
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/find-the-city-with-the-smallest-number-of-neighbors-at-a-threshold-distance/
// discuss: https://leetcode.com/problems/find-the-city-with-the-smallest-number-of-neighbors-at-a-threshold-distance/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
// Floyd–Warshall algorithm
impl Solution {
    pub fn find_the_city(n: i32, edges: Vec<Vec<i32>>, distance_threshold: i32) -> i32 {
        let n = n as usize;
        let mut distances: Vec<Vec<i32>> = vec![vec![i32::MAX; n]; n];
        for i in 0..edges.len() {
            let from = edges[i][0] as usize;
            let to = edges[i][1] as usize;
            let weight = edges[i][2];
            distances[from][to] = weight;
            distances[to][from] = weight;
        }
        for i in 0..n {
            distances[i][i] = 0;
        }
        for k in 0..n {
            for i in 0..n {
                for j in 0..n {
                    if distances[i][k] < i32::MAX && distances[k][j] < i32::MAX {
                        if distances[i][j] > distances[i][k] + distances[k][j] {
                            distances[i][j] = distances[i][k] + distances[k][j];
                        }
                    }
                }
            }
        }
        // println!("distances: {:?}", distances);
        let mut min_count = usize::MAX;
        let mut res = usize::MAX;
        for i in 0..n {
            let count = distances[i].iter().filter(|&&dist| dist <= distance_threshold).count();
            if count <= min_count {
                min_count = count;
                res = i;
            }
        }
        res as i32
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_1334() {
        assert_eq!(Solution::find_the_city(4, vec![vec![0,1,3],vec![1,2,1],vec![1,3,4],vec![2,3,1]], 4), 3);
        assert_eq!(Solution::find_the_city(5, vec![vec![0,1,2],vec![0,4,8],vec![1,2,3],vec![1,4,2], vec![2,3,1], vec![3,4,1]], 2), 0);
    }
}

```
