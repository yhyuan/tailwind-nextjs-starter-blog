---
title: Leetcode 0647 palindromic substrings
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0647 palindromic substrings in Rust
---

## Solution in Rust

```rust
/**
 * [647] Palindromic Substrings
 *
 * Given a string s, return the number of palindromic substrings in it.
 * A string is a palindrome when it reads the same backward as forward.
 * A substring is a contiguous sequence of characters within the string.
 *
 * Example 1:
 *
 * Input: s = "abc"
 * Output: 3
 * Explanation: Three palindromic strings: "a", "b", "c".
 *
 * Example 2:
 *
 * Input: s = "aaa"
 * Output: 6
 * Explanation: Six palindromic strings: "a", "a", "a", "aa", "aa", "aaa".
 *
 *
 * Constraints:
 *
 * 	1 <= s.length <= 1000
 * 	s consists of lowercase English letters.
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/palindromic-substrings/
// discuss: https://leetcode.com/problems/palindromic-substrings/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn count_substrings(s: String) -> i32 {
        let chars = s.chars().collect::<Vec<_>>();
        let n = chars.len();
        let mut dp: Vec<Vec<bool> > = vec![vec![false; n]; n];
        //len = 1
        for i in 0..n {
            dp[i][i] = true;
        }
        // len = 2
        for i in 1..n {
            dp[i - 1][i] = chars[i - 1] == chars[i];
        }
        // len > 3
        for len in 3..=n {
            for i in len - 1..n {
                dp[i + 1 - len][i] = chars[i + 1 - len] == chars[i] && dp[i + 1 - len + 1][i - 1];
            }
        }
        let mut res = 0;
        for i in 0..n {
            for j in i..n {
                if dp[i][j] {
                    res += 1;
                }
            }
        }
        res
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_647() {
        assert_eq!(Solution::count_substrings("abc".to_string()), 3);
        assert_eq!(Solution::count_substrings("aaa".to_string()), 6);
    }
}

```
