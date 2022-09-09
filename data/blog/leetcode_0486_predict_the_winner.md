---
title: Leetcode 0486 predict the winner
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0486 predict the winner in Rust
---

## Solution in Rust

```rust
/**
 * [486] Predict the Winner
 *
 * You are given an integer array nums. Two players are playing a game with this array: player 1 and player 2.
 * Player 1 and player 2 take turns, with player 1 starting first. Both players start the game with a score of 0. At each turn, the player takes one of the numbers from either end of the array (i.e., nums[0] or nums[nums.length - 1]) which reduces the size of the array by 1. The player adds the chosen number to their score. The game ends when there are no more elements in the array.
 * Return true if Player 1 can win the game. If the scores of both players are equal, then player 1 is still the winner, and you should also return true. You may assume that both players are playing optimally.
 *
 * Example 1:
 *
 * Input: nums = [1,5,2]
 * Output: false
 * Explanation: Initially, player 1 can choose between 1 and 2.
 * If he chooses 2 (or 1), then player 2 can choose from 1 (or 2) and 5. If player 2 chooses 5, then player 1 will be left with 1 (or 2).
 * So, final score of player 1 is 1 + 2 = 3, and player 2 is 5.
 * Hence, player 1 will never be the winner and you need to return false.
 *
 * Example 2:
 *
 * Input: nums = [1,5,233,7]
 * Output: true
 * Explanation: Player 1 first chooses 1. Then player 2 has to choose between 5 and 7. No matter which number player 2 choose, player 1 can choose 233.
 * Finally, player 1 has more score (234) than player 2 (12), so you need to return True representing player1 can win.
 *
 *
 * Constraints:
 *
 * 	1 <= nums.length <= 20
 * 	0 <= nums[i] <= 10^7
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/predict-the-winner/
// discuss: https://leetcode.com/problems/predict-the-winner/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

use std::collections::HashMap;
impl Solution {
    pub fn helper(nums: &Vec<i32>, start: usize, end: usize, is_player_one_pick: bool, memo: &mut HashMap<(usize, usize, bool), (i32, i32)>) -> (i32, i32) {
        if memo.contains_key(&(start, end, is_player_one_pick)) {
            return *memo.get(&(start, end, is_player_one_pick)).unwrap();
        }
        if start == end {
            let res = if is_player_one_pick {(nums[start], 0)} else {(0, nums[start])};
            memo.insert((start, end, is_player_one_pick), res);
            return res;
        }
        let mut pick_left = Self::helper(nums, start + 1, end, !is_player_one_pick, memo);
        let mut pick_right = Self::helper(nums, start, end - 1, !is_player_one_pick, memo);
        let res = if is_player_one_pick {
            if pick_left.0 + nums[start] >= pick_right.0 + nums[end] {
                (pick_left.0 + nums[start], pick_left.1)
            } else {
                (pick_right.0 + nums[end], pick_right.1)
            }
        } else {
            if pick_left.1 + nums[start] >= pick_right.1 + nums[end] {
                (pick_left.0, pick_left.1 + nums[start])
            } else {
                (pick_right.0, pick_right.1 + nums[end])
            }
        };
        memo.insert((start, end, is_player_one_pick), res);
        res
    }
    pub fn predict_the_winner(nums: Vec<i32>) -> bool {
        let n = nums.len();
        //key: start index, end index, is player pick
        //value: player 1 score, player 2 score
        let mut memo: HashMap<(usize, usize, bool), (i32, i32)> = HashMap::new();
        let (score1, score2) = Self::helper(&nums, 0, n - 1, true, &mut memo);
        score1 >= score2
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_486() {
        assert_eq!(Solution::predict_the_winner(vec![1,5,2]), false);
        assert_eq!(Solution::predict_the_winner(vec![1,5,233,7]), true);
    }
}

```
