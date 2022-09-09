---
title: Leetcode 0290 word pattern
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0290 word pattern in Rust
---

## Solution in Rust

```rust
/**
 * [290] Word Pattern
 *
 * Given a pattern and a string s, find if s follows the same pattern.
 * Here follow means a full match, such that there is a bijection between a letter in pattern and a non-empty word in s.
 *
 * Example 1:
 *
 * Input: pattern = "abba", s = "dog cat cat dog"
 * Output: true
 *
 * Example 2:
 *
 * Input: pattern = "abba", s = "dog cat cat fish"
 * Output: false
 *
 * Example 3:
 *
 * Input: pattern = "aaaa", s = "dog cat cat dog"
 * Output: false
 *
 * Example 4:
 *
 * Input: pattern = "abba", s = "dog dog dog dog"
 * Output: false
 *
 *
 * Constraints:
 *
 * 	1 <= pattern.length <= 300
 * 	pattern contains only lower-case English letters.
 * 	1 <= s.length <= 3000
 * 	s contains only lower-case English letters and spaces ' '.
 * 	s does not contain any leading or trailing spaces.
 * 	All the words in s are separated by a single space.
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/word-pattern/
// discuss: https://leetcode.com/problems/word-pattern/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
use std::collections::HashMap;
impl Solution {
    pub fn word_pattern(pattern: String, s: String) -> bool {
        let chars: Vec<char> = pattern.chars().collect();
        let words: Vec<&str> = s.split_ascii_whitespace().collect();
        let words: Vec<String> = words.iter().map(|s| s.to_string()).collect();
        if chars.len() != words.len() {
            return false;
        }
        let mut char_str_dict: HashMap<char, String> = HashMap::new();
        let mut str_char_dict: HashMap<String, char> = HashMap::new();
        for i in 0..chars.len() {
            let ch = chars[i];
            let word = words[i].clone();
            if char_str_dict.contains_key(&ch) && !str_char_dict.contains_key(&word){
                return false;
            }
            if !char_str_dict.contains_key(&ch) && str_char_dict.contains_key(&word){
                return false;
            }
            if !char_str_dict.contains_key(&ch) && !str_char_dict.contains_key(&word){
                char_str_dict.insert(ch, word.clone());
                str_char_dict.insert(word, ch);
            } else {
                let w = char_str_dict[&ch].clone();
                let ch_dict = str_char_dict[&word];
                if ch_dict != ch || w != word {
                    return false;
                }
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
    fn test_290() {

        assert_eq!(
            Solution::word_pattern("abba".to_owned(), "dog cat cat dog".to_owned()),
            true
        );

        assert_eq!(
            Solution::word_pattern("aaaa".to_owned(), "dog cat cat dog".to_owned()),
            false
        );

        assert_eq!(
            Solution::word_pattern("abba".to_owned(), "dog cat cat fish".to_owned()),
            false
        );

        assert_eq!(
            Solution::word_pattern("abba".to_owned(), "dog dog dog dog".to_owned()),
            false
        );
    }
}

```
