---
title: Leetcode 0174 dungeon game
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0174 dungeon game in Rust
---

## Solution in Rust

```rust
/**
 * [174] Dungeon Game
 *
 * The demons had captured the princess and imprisoned her in the bottom-right corner of a dungeon. The dungeon consists of m x n rooms laid out in a 2D grid. Our valiant knight was initially positioned in the top-left room and must fight his way through dungeon to rescue the princess.
 * The knight has an initial health point represented by a positive integer. If at any point his health point drops to 0 or below, he dies immediately.
 * Some of the rooms are guarded by demons (represented by negative integers), so the knight loses health upon entering these rooms; other rooms are either empty (represented as 0) or contain magic orbs that increase the knight's health (represented by positive integers).
 * To reach the princess as quickly as possible, the knight decides to move only rightward or downward in each step.
 * Return the knight's minimum initial health so that he can rescue the princess.
 * Note that any room can contain threats or power-ups, even the first room the knight enters and the bottom-right room where the princess is imprisoned.
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2021/03/13/dungeon-grid-1.jpg" style="width: 253px; height: 253px;" />
 * Input: dungeon = [[-2,-3,3],[-5,-10,1],[10,30,-5]]
 * Output: 7
 * Explanation: The initial health of the knight must be at least 7 if he follows the optimal path: RIGHT-> RIGHT -> DOWN -> DOWN.
 *
 * Example 2:
 *
 * Input: dungeon = [[0]]
 * Output: 1
 *
 *
 * Constraints:
 *
 * 	m == dungeon.length
 * 	n == dungeon[i].length
 * 	1 <= m, n <= 200
 * 	-1000 <= dungeon[i][j] <= 1000
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/dungeon-game/
// discuss: https://leetcode.com/problems/dungeon-game/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
impl Solution {
    pub fn calculate_minimum_hp(dungeon: Vec<Vec<i32>>) -> i32 {
        let m = dungeon.len();
        let n = dungeon[0].len();
        let mut dp: Vec<Vec<i32>> = vec![vec![0; n + 1]; m + 1];
        dp[m - 1][n - 1] = if dungeon[m - 1][n - 1] > 0 {1} else {1 - dungeon[m - 1][n - 1]};
        for j in 0..=n {
            dp[m][j] = i32::MAX;
        }
        for i in 0..=m {
            dp[i][n] = i32::MAX;
        }
        for i in (0..m).rev() {
            for j in (0..n).rev() {
                if i == m - 1 && j == n - 1 {
                    continue;
                }
                dp[i][j] = i32::min(dp[i + 1][j], dp[i][j + 1]) - dungeon[i][j];
                if dp[i][j] <= 0 {
                    dp[i][j] = 1;
                }
            }
        }
        dp[0][0]
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_174() {
        /*
        assert_eq!(
            Solution::calculate_minimum_hp(vec![
                vec![-2, -3, 3],
                vec![-5, -10, 1],
                vec![10, 30, -5],
            ]),
            7
        );
        assert_eq!(
            Solution::calculate_minimum_hp(vec![vec![1, -4, 5, -99], vec![2, -2, -2, -1]]),
            3
        );
        assert_eq!(
            Solution::calculate_minimum_hp(vec![vec![1,-3,3],vec![0,-2,0],vec![-3,-3,-3]]), 3
        );
        */
        assert_eq!(Solution::calculate_minimum_hp(vec![vec![0,0,0],vec![1,1,-1]]), 1);
    }
}

```
