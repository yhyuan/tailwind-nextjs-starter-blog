---
title: Leetcode 0387 first unique character in a string
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0387 first unique character in a string in Rust
---

## Solution in Rust

```rust
/**
 * [387] First Unique Character in a String
 *
 * Given a string s, find the first non-repeating character in it and return its index. If it does not exist, return -1.
 *
 * Example 1:
 * Input: s = "leetcode"
 * Output: 0
 * Example 2:
 * Input: s = "loveleetcode"
 * Output: 2
 * Example 3:
 * Input: s = "aabb"
 * Output: -1
 *
 * Constraints:
 *
 * 	1 <= s.length <= 10^5
 * 	s consists of only lowercase English letters.
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/first-unique-character-in-a-string/
// discuss: https://leetcode.com/problems/first-unique-character-in-a-string/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
/*
use std::collections::HashMap;
use std::collections::HashSet;
impl Solution {
    pub fn first_uniq_char(s: String) -> i32 {
        let chars: Vec<char> = s.chars().collect();
        let mut hashmap: HashMap<char, usize> = HashMap::new();
        for ch in chars.iter() {
            if hashmap.contains_key(ch) {
                hashmap.insert(*ch, hashmap[ch] + 1);
            } else {
                hashmap.insert(*ch, 1);
            }
        }
        let min_freq = hashmap.iter().map(|(_, v)| *v).min().unwrap();
        if min_freq >= 2 {
            return -1;
        }
        let min_freq_set: HashSet<char> = hashmap.iter().filter(|(k, &v)| v == 1).map(|(k, _) | *k).collect();
        for (i, ch) in chars.iter().enumerate() {
            if min_freq_set.contains(ch) {
                return i as i32;
            }
        }
        0
    }
}
*/
use std::collections::HashMap;
impl Solution {
    pub fn build_hashmap(chars: &Vec<char>) -> HashMap<char, usize> {
        let mut m: HashMap<char, usize> = HashMap::new();
        for &x in chars {
            *m.entry(x).or_default() += 1;
        }
        m
    }
    pub fn first_uniq_char(s: String) -> i32 {
        let chars: Vec<char> = s.chars().collect();
        let hashmap = Self::build_hashmap(&chars);
        for (i, ch) in chars.iter().enumerate() {
            if hashmap.get(ch).unwrap() == &1 {
                return i as i32;
            }
        }
        -1
    }
}
// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_387() {
        assert_eq!(Solution::first_uniq_char("leetcode".to_string()), 0);
        assert_eq!(Solution::first_uniq_char("loveleetcode".to_string()), 2);
        assert_eq!(Solution::first_uniq_char("aabb".to_string()), -1);
    }
}

```
