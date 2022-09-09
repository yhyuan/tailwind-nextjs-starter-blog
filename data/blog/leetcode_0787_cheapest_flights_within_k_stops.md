---
title: Leetcode 0787 cheapest flights within k stops
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0787 cheapest flights within k stops in Rust
---

## Solution in Rust

```rust
/**
 * [787] Cheapest Flights Within K Stops
 *
 * There are n cities connected by some number of flights. You are given an array flights where flights[i] = [fromi, toi, pricei] indicates that there is a flight from city fromi to city toi with cost pricei.
 * You are also given three integers src, dst, and k, return the cheapest price from src to dst with at most k stops. If there is no such route, return -1.
 *
 * Example 1:
 * <img alt="" src="https://s3-lc-upload.s3.amazonaws.com/uploads/2018/02/16/995.png" style="height: 360px; width: 492px;" />
 * Input: n = 3, flights = [[0,1,100],[1,2,100],[0,2,500]], src = 0, dst = 2, k = 1
 * Output: 200
 * Explanation: The graph is shown.
 * The cheapest price from city 0 to city 2 with at most 1 stop costs 200, as marked red in the picture.
 *
 * Example 2:
 * <img alt="" src="https://s3-lc-upload.s3.amazonaws.com/uploads/2018/02/16/995.png" style="height: 360px; width: 492px;" />
 * Input: n = 3, flights = [[0,1,100],[1,2,100],[0,2,500]], src = 0, dst = 2, k = 0
 * Output: 500
 * Explanation: The graph is shown.
 * The cheapest price from city 0 to city 2 with at most 0 stop costs 500, as marked blue in the picture.
 *
 *
 * Constraints:
 *
 * 	1 <= n <= 100
 * 	0 <= flights.length <= (n * (n - 1) / 2)
 * 	flights[i].length == 3
 * 	0 <= fromi, toi < n
 * 	fromi != toi
 * 	1 <= pricei <= 10^4
 * 	There will not be any multiple flights between two cities.
 * 	0 <= src, dst, k < n
 * 	src != dst
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/cheapest-flights-within-k-stops/
// discuss: https://leetcode.com/problems/cheapest-flights-within-k-stops/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
use std::collections::{HashSet, HashMap};
impl Solution {
    pub fn find_cheapest_price(n: i32, flights: Vec<Vec<i32>>, src: i32, dst: i32, k: i32) -> i32 {
        let n = n as usize;
        let mut graph: Vec<HashSet<usize>> = vec![HashSet::new(); n];
        let mut costs: HashMap<(usize, usize), i32> = HashMap::new();
        for i in 0..flights.len() {
            let start = flights[i][0] as usize;
            let end = flights[i][1] as usize;
            let cost = flights[i][2];
            graph[start].insert(end);
            costs.insert((start, end), cost);
        }
        let k_max = k as usize;
        let mut dp: Vec<Vec<i32>> = vec![vec![i32::MAX; n]; k_max + 1];
        let src = src as usize;
        let dst = dst as usize;
        for &i in graph[src].iter() {
            dp[0][i] = costs[&(src, i)];
        }
        for k in 1..=k_max {
            for j in 0..n {
                if dp[k - 1][j] != i32::MAX {
                    for &m in graph[j].iter() {
                        dp[k][m] = i32::min(dp[k][m], dp[k - 1][j] + costs[&(j, m)]);
                    }
                }
            }
        }
        let res = dp.iter().map(|v| v[dst]).min().unwrap();
        if res == i32::MAX {-1} else {res}
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_787() {
    }
}

```
