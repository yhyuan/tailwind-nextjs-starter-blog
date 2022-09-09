---
title: Leetcode 0205 isomorphic strings
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0205 isomorphic strings in Rust
---

## Solution in Rust

```rust
/**
 * [205] Isomorphic Strings
 *
 * Given two strings s and t, determine if they are isomorphic.
 * Two strings s and t are isomorphic if the characters in s can be replaced to get t.
 * All occurrences of a character must be replaced with another character while preserving the order of characters. No two characters may map to the same character, but a character may map to itself.
 *
 * Example 1:
 * Input: s = "egg", t = "add"
 * Output: true
 * Example 2:
 * Input: s = "foo", t = "bar"
 * Output: false
 * Example 3:
 * Input: s = "paper", t = "title"
 * Output: true
 *
 * Constraints:
 *
 * 	1 <= s.length <= 5 * 10^4
 * 	t.length == s.length
 * 	s and t consist of any valid ascii character.
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/isomorphic-strings/
// discuss: https://leetcode.com/problems/isomorphic-strings/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
use std::collections::HashMap;
impl Solution {
    pub fn helper (s_chars: &Vec<char>, t_chars: &Vec<char>) -> bool {
        let mut dict: HashMap<char, char> = HashMap::new();
        for (i, &s_char) in s_chars.iter().enumerate() {
            let t_char = t_chars[i];
            if dict.contains_key(&s_char) {
                if dict[&s_char] != t_char {
                    return false;
                }
            } else {
                dict.insert(s_char, t_char);
            }
        }
        true
    }
    pub fn is_isomorphic(s: String, t: String) -> bool {
        if s.len() != t.len() {
            return false;
        }
        if s.len() == 0 {
            return true;
        }
        let s_chars: Vec<char> = s.chars().collect();
        let t_chars: Vec<char> = t.chars().collect();
        Solution::helper(&s_chars, &t_chars) && Solution::helper(&t_chars, &s_chars)
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_205() {
        assert_eq!(
            Solution::is_isomorphic("egg".to_owned(), "app".to_owned()),
            true
        );
        assert_eq!(
            Solution::is_isomorphic("pecil".to_owned(), "this".to_owned()),
            false
        );
        assert_eq!(
            Solution::is_isomorphic("paper".to_owned(), "title".to_owned()),
            true
        );
    }
}

```
