---
title: Leetcode 0125 valid palindrome
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0125 valid palindrome in Rust
---

## Solution in Rust

```rust
/**
 * [125] Valid Palindrome
 *
 * Given a string s, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.
 *
 * Example 1:
 *
 * Input: s = "A man, a plan, a canal: Panama"
 * Output: true
 * Explanation: "amanaplanacanalpanama" is a palindrome.
 *
 * Example 2:
 *
 * Input: s = "race a car"
 * Output: false
 * Explanation: "raceacar" is not a palindrome.
 *
 *
 * Constraints:
 *
 * 	1 <= s.length <= 2 * 10^5
 * 	s consists only of printable ASCII characters.
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/valid-palindrome/
// discuss: https://leetcode.com/problems/valid-palindrome/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
impl Solution {
    pub fn is_palindrome(s: String) -> bool {
        let mut chars: Vec<char> = vec![];
        for ch in s.to_lowercase().chars() {
            match ch {
                '0'..='9' | 'a'..='z' => {
                    chars.push(ch);
                },
                _ => {

                }
            }
        }
        if chars.len() == 0 {
            return true;
        }
        for i in 0..chars.len()/2 {
            if chars[i] != chars[chars.len() - 1 - i] {
                return false;
            }
        }
        true
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_125() {
        assert_eq!(Solution::is_palindrome("A man, a plan, a canal: Panama".to_string()), true);
        assert_eq!(Solution::is_palindrome("race a car".to_string()), false);
    }
}

```
