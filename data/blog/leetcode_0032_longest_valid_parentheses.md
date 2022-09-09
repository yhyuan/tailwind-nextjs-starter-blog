---
title: Leetcode 0032 longest valid parentheses
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0032 longest valid parentheses in Rust
---

## Solution in Rust

```rust
/**
 * [32] Longest Valid Parentheses
 *
 * Given a string containing just the characters '(' and ')', find the length of the longest valid (well-formed) parentheses substring.
 *
 * Example 1:
 *
 * Input: s = "(()"
 * Output: 2
 * Explanation: The longest valid parentheses substring is "()".
 *
 * Example 2:
 *
 * Input: s = ")()())"
 * Output: 4
 * Explanation: The longest valid parentheses substring is "()()".
 *
 * Example 3:
 *
 * Input: s = ""
 * Output: 0
 *
 *
 * Constraints:
 *
 * 	0 <= s.length <= 3 * 10^4
 * 	s[i] is '(', or ')'.
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/longest-valid-parentheses/
// discuss: https://leetcode.com/problems/longest-valid-parentheses/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn longest_valid_parentheses(s: String) -> i32 {
        if s.len() == 0 {
            return 0;
        }
        let chars: Vec<char> = s.chars().collect();
        let mut dp: Vec<usize> = vec![0; chars.len()];
        for i in 1..chars.len() {
            if chars[i] == ')' {
                if chars[i - 1] == '(' {
                    if i == 1 {
                        dp[i] = 2;
                    } else {
                        dp[i] = dp[i - 2] + 2;
                    }
                } else {
                    if i >= dp[i-1] + 1 && chars[i - dp[i - 1] - 1] == '(' {
                        let v = if i >= dp[i - 1] + 2 {dp[i - dp[i - 1] - 2]} else {0};
                        dp[i] = dp[i - 1] + v + 2;
                    } else {
                        dp[i] = 0;
                    }
                }
            }
        }
        //println!("{:?}", chars);
        //println!("{:?}", dp);
        let value = dp.iter().max().unwrap();
        *value as i32
        //dp[s.len() - 1]
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_32() {
        //
        assert_eq!(Solution::longest_valid_parentheses("(()())".to_string()), 6);

        assert_eq!(Solution::longest_valid_parentheses("())".to_string()), 2);
        assert_eq!(Solution::longest_valid_parentheses("()()(())".to_string()), 8);
        assert_eq!(Solution::longest_valid_parentheses(")()())".to_string()), 4);
        assert_eq!(Solution::longest_valid_parentheses(")(".to_string()), 0);
        assert_eq!(Solution::longest_valid_parentheses("(()".to_string()), 2);
        assert_eq!(
            Solution::longest_valid_parentheses("(((((()()".to_string()),
            4
        );
        assert_eq!(
            Solution::longest_valid_parentheses("((((((((()))".to_string()),
            6
        );
        assert_eq!(Solution::longest_valid_parentheses("()".to_string()), 2);
        assert_eq!(Solution::longest_valid_parentheses("()(()".to_string()), 2);
        assert_eq!(
            Solution::longest_valid_parentheses(")()(((())))(".to_string()),
            10
        );
        assert_eq!(
            Solution::longest_valid_parentheses("(()(((()".to_string()),
            2
        );
        assert_eq!(Solution::longest_valid_parentheses("".to_string()), 0);

    }
}

```
