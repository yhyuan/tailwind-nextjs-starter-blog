---
title: Leetcode 1884 egg drop with 2 eggs and n floors
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 1884 egg drop with 2 eggs and n floors in Rust
---

## Solution in Rust

```rust
/**
 * [1884] Egg Drop With 2 Eggs and N Floors
 *
 * You are given two identical eggs and you have access to a building with n floors labeled from 1 to n.
 * You know that there exists a floor f where 0 <= f <= n such that any egg dropped at a floor higher than f will break, and any egg dropped at or below floor f will not break.
 * In each move, you may take an unbroken egg and drop it from any floor x (where 1 <= x <= n). If the egg breaks, you can no longer use it. However, if the egg does not break, you may reuse it in future moves.
 * Return the minimum number of moves that you need to determine with certainty what the value of f is.
 *
 * Example 1:
 *
 * Input: n = 2
 * Output: 2
 * Explanation: We can drop the first egg from floor 1 and the second egg from floor 2.
 * If the first egg breaks, we know that f = 0.
 * If the second egg breaks but the first egg didn't, we know that f = 1.
 * Otherwise, if both eggs survive, we know that f = 2.
 *
 * Example 2:
 *
 * Input: n = 100
 * Output: 14
 * Explanation: One optimal strategy is:
 * - Drop the 1st egg at floor 9. If it breaks, we know f is between 0 and 8. Drop the 2nd egg starting
 *   from floor 1 and going up one at a time to find f within 7 more drops. Total drops is 1 + 7 = 8.
 * - If the 1st egg does not break, drop the 1st egg again at floor 22. If it breaks, we know f is between 9
 *   and 21. Drop the 2nd egg starting from floor 10 and going up one at a time to find f within 12 more
 *   drops. Total drops is 2 + 12 = 14.
 * - If the 1st egg does not break again, follow a similar process dropping the 1st egg from floors 34, 45,
 *   55, 64, 72, 79, 85, 90, 94, 97, 99, and 100.
 * Regardless of the outcome, it takes at most 14 drops to determine f.
 *
 *
 * Constraints:
 *
 * 	1 <= n <= 1000
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/egg-drop-with-2-eggs-and-n-floors/
// discuss: https://leetcode.com/problems/egg-drop-with-2-eggs-and-n-floors/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn two_egg_drop(n: i32) -> i32 {
        //Calculate the root of k^2 + k - 2 * n = 0
        let a = 1;
        let b = 1;
        let c = -2 * n;
        let mut temp = (b * b - 4 * a * c) as f64;
        temp = temp.sqrt();
        let k =(temp - b as f64) / (2f64 * a as f64);
        k.ceil() as i32
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_1884() {
        assert_eq!(Solution::two_egg_drop(2), 2);
        assert_eq!(Solution::two_egg_drop(100), 14);
    }
}

```
