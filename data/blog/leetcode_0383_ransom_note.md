---
title: Leetcode 0383 ransom note
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0383 ransom note in Rust
---

## Solution in Rust

```rust
/**
 * [383] Ransom Note
 *
 * Given two stings ransomNote and magazine, return true if ransomNote can be constructed from magazine and false otherwise.
 * Each letter in magazine can only be used once in ransomNote.
 *
 * Example 1:
 * Input: ransomNote = "a", magazine = "b"
 * Output: false
 * Example 2:
 * Input: ransomNote = "aa", magazine = "ab"
 * Output: false
 * Example 3:
 * Input: ransomNote = "aa", magazine = "aab"
 * Output: true
 *
 * Constraints:
 *
 * 	1 <= ransomNote.length, magazine.length <= 10^5
 * 	ransomNote and magazine consist of lowercase English letters.
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/ransom-note/
// discuss: https://leetcode.com/problems/ransom-note/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
/*
use std::collections::HashMap;
impl Solution {
    pub fn can_construct(ransom_note: String, magazine: String) -> bool {
        let mut ransom_hashmap: HashMap<char, usize> = HashMap::new();
        let mut magazine_hashmap: HashMap<char, usize> = HashMap::new();
        for ch in ransom_note.chars() {
            if ransom_hashmap.contains_key(&ch) {
                ransom_hashmap.insert(ch, ransom_hashmap[&ch] + 1);
            } else {
                ransom_hashmap.insert(ch, 1);
            }
        }
        for ch in magazine.chars() {
            if magazine_hashmap.contains_key(&ch) {
                magazine_hashmap.insert(ch, magazine_hashmap[&ch] + 1);
            } else {
                magazine_hashmap.insert(ch, 1);
            }
        }
        println!("ransom: {:?}", ransom_hashmap);
        println!("magazine: {:?}", magazine_hashmap);
        for (k, &v) in ransom_hashmap.iter() {
            if !magazine_hashmap.contains_key(k) {
                return false;
            }
            if magazine_hashmap[k] < v {
                return false;
            }
        }
        true
    }
}
*/
use std::collections::HashMap;
impl Solution {
    pub fn build_hashmap(s: &String) -> HashMap<char, usize> {
        let chars: Vec<char> = s.chars().collect();
        let mut m: HashMap<char, usize> = HashMap::new();
        for &x in chars.iter() {
            *m.entry(x).or_default() += 1;
        }
        m
    }
    pub fn can_construct(ransom_note: String, magazine: String) -> bool {
        let note_m = Self::build_hashmap(&ransom_note);
        let magazine_m = Self::build_hashmap(&magazine);
        for (key, val) in note_m.iter() {
            if !magazine_m.contains_key(key) {
                return false;
            }
            let val2 = magazine_m.get(key).unwrap();
            if val > val2 {
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
    fn test_383() {
        assert_eq!(Solution::can_construct("aa".to_string(), "ab".to_string()), false);
    }
}


```
