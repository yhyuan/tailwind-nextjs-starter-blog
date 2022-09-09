---
title: Leetcode 0293 flip game
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0293 flip game in Rust
---

## Solution in Rust

```rust
/**
 * [293] Flip Game
 *
You are playing a Flip Game with your friend.

You are given a string currentState that contains only '+' and '-'. You and your friend take turns to flip two consecutive "++" into "--". The game ends when a person can no longer make a move, and therefore the other person will be the winner.

Return all possible states of the string currentState after one valid move. You may return the answer in any order. If there is no valid move, return an empty list [].



Example 1:

Input: currentState = "++++"
Output: ["--++","+--+","++--"]
Example 2:

Input: currentState = "+"
Output: []


Constraints:

1 <= currentState.length <= 500
currentState[i] is either '+' or '-'. *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/nim-game/
// discuss: https://leetcode.com/problems/nim-game/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn generate_possible_next_moves(current_state: String) -> Vec<String> {
        let chars: Vec<char> = current_state.chars().collect();
        let n = chars.len();
        let mut results: Vec<String> = vec![];
        for i in 1..n {
            if chars[i - 1] == '+' && chars[i] == '+' {
                let mut result = chars.clone();
                result[i - 1] = '-';
                result[i] = '-';
                let r: String = result.iter().collect();
                results.push(r);
            }
        }
        results
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_293() {
        assert_eq!(Solution::generate_possible_next_moves("++++".to_string()), vec!["--++".to_string(),"+--+".to_string(),"++--".to_string()]);
    }
}

```
