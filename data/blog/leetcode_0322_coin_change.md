---
title: Leetcode 0322 coin change
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0322 coin change in Rust
---

## Solution in Rust

```rust
/**
 * [322] Coin Change
 *
 * You are given an integer array coins representing coins of different denominations and an integer amount representing a total amount of money.
 * Return the fewest number of coins that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return -1.
 * You may assume that you have an infinite number of each kind of coin.
 *
 * Example 1:
 *
 * Input: coins = [1,2,5], amount = 11
 * Output: 3
 * Explanation: 11 = 5 + 5 + 1
 *
 * Example 2:
 *
 * Input: coins = [2], amount = 3
 * Output: -1
 *
 * Example 3:
 *
 * Input: coins = [1], amount = 0
 * Output: 0
 *
 * Example 4:
 *
 * Input: coins = [1], amount = 1
 * Output: 1
 *
 * Example 5:
 *
 * Input: coins = [1], amount = 2
 * Output: 2
 *
 *
 * Constraints:
 *
 * 	1 <= coins.length <= 12
 * 	1 <= coins[i] <= 2^31 - 1
 * 	0 <= amount <= 10^4
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/coin-change/
// discuss: https://leetcode.com/problems/coin-change/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
/*
impl Solution {
    pub fn coin_change(coins: Vec<i32>, amount: i32) -> i32 {
        let mut dp: Vec<i32> = vec![i32::MAX; amount as usize + 1];
        dp[0] = 0;
        for i in 1..=amount {
            let mut r = i32::MAX;
            for j in 0..coins.len() {
                if i >= coins[j] {
                    if dp[(i - coins[j]) as usize] != i32::MAX {
                        r = i32::min(r, dp[(i - coins[j]) as usize] + 1);
                    }
                }
            }
            dp[i as usize] = r;
        }
        if dp[amount as usize] == i32::MAX {-1} else {dp[amount as usize]}
    }
}
*/
use std::collections::HashSet;
impl Solution {
    pub fn coin_change(coins: Vec<i32>, amount: i32) -> i32 {
        if amount == 0 {
            return 0;
        }
        let amount = amount as usize;
        let coins_set: HashSet<i32> = coins.into_iter().collect::<HashSet<_>>();
        let mut dp: Vec<i32> = vec![i32::MAX; amount + 1];
        for i in 0..=amount {
            if coins_set.contains(&(i as i32)) {
                dp[i] = 1;
            } else {
                let mut min_val = i32::MAX;
                for &coin in coins_set.iter() {
                    if coin > i as i32 {
                        continue;
                    }
                    if dp[i - coin as usize] == i32::MAX {
                        continue;
                    }
                    min_val = i32::min(min_val, dp[i - coin as usize] + 1);
                }
                dp[i] = min_val;
            }
        }
        // println!("dp: {:?}", dp);
        if dp[amount] == i32::MAX {-1} else {dp[amount]}
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_322() {
        assert_eq!(Solution::coin_change(vec![1, 2, 5], 11), 3);
        assert_eq!(Solution::coin_change(vec![2], 3), -1);
        assert_eq!(Solution::coin_change(vec![1], 0), 0);
    }
}

```
