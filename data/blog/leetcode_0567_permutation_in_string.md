---
title: Leetcode 0567 permutation in string
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0567 permutation in string in Rust
---

## Solution in Rust

```rust
/**
 * [567] Permutation in String
 *
 * Given two strings s1 and s2, return true if s2 contains a permutation of s1, or false otherwise.
 * In other words, return true if one of s1's permutations is the substring of s2.
 *
 * Example 1:
 *
 * Input: s1 = "ab", s2 = "eidbaooo"
 * Output: true
 * Explanation: s2 contains one permutation of s1 ("ba").
 *
 * Example 2:
 *
 * Input: s1 = "ab", s2 = "eidboaoo"
 * Output: false
 *
 *
 * Constraints:
 *
 * 	1 <= s1.length, s2.length <= 10^4
 * 	s1 and s2 consist of lowercase English letters.
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/permutation-in-string/
// discuss: https://leetcode.com/problems/permutation-in-string/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
/*
impl Solution {
    pub fn is_all_same(counts: &[i32; 26], cur_counts: &[i32; 26]) -> bool {
        for i in 0..26 {
            if counts[i] != cur_counts[i] {
                return false;
            }
        }
        true
    }
    pub fn check_inclusion(s1: String, s2: String) -> bool {
        if s1.len() > s2.len() {
            return false;
        }
        let mut counts = [0; 26];
        for ch in s1.chars() {
            let index = (ch as usize - 'a' as usize);
            counts[index] += 1;
        }
        let chars: Vec<char> = s2.chars().collect();
        let mut cur_counts = [0; 26];
        for i in 0..s1.len() {
            let index = chars[i] as usize - 'a' as usize;
            cur_counts[index] += 1;
        }
        if Self::is_all_same(&counts, &cur_counts) {
            return true;
        }
        // println!("counts: {:?}", counts);
        for i in s1.len()..s2.len() {
            // enter i, remove i - s1.len()
            let enter_index = chars[i] as usize - 'a' as usize;
            let remove_index = chars[i - s1.len()] as usize - 'a' as usize;
            cur_counts[enter_index] += 1;
            cur_counts[remove_index] -= 1;
            // println!("i: {}, cur_counts: {:?}", i, cur_counts);

            if Self::is_all_same(&counts, &cur_counts) {
                return true;
            }
        }
        false
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
    pub fn contains_hashmap(m1: &HashMap<char, usize>, m2: &HashMap<char, usize>) -> bool {
        for (key, val) in m1 {
            if !m2.contains_key(key) {
                return false;
            }
            let val2 = m2.get(key).unwrap();
            if val2 < val {
                return false;
            }
        }
        true
    }
    pub fn remove_char_hashmap(m: &mut HashMap<char, usize>, ch: char) {
        let count = *m.get(&ch).unwrap();
        if count > 1 {
            m.insert(ch, count - 1);
        } else {
            m.remove_entry(&ch);
        }
    }
    pub fn check_inclusion(s1: String, s2: String) -> bool {
        let chars1 = s1.chars().collect::<Vec<_>>();
        let m1 = Self::build_hashmap(&chars1);
        let chars = s2.chars().collect::<Vec<_>>();
        let n = chars.len();
        let mut start = 0usize;
        let mut m2: HashMap<char, usize> = HashMap::new();
        for i in 0..=n {
            if Self::contains_hashmap(&m1, &m2) {
                while !Self::contains_hashmap(&m2, &m1) {
                    Self::remove_char_hashmap(&mut m2, chars[start]);
                    start += 1;
                }
                if Self::contains_hashmap(&m1, &m2) {
                    return true;
                }
            }
            if i < n {
                *m2.entry(chars[i]).or_default() += 1;
            }
        }
        false
    }
}
// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_567() {
        assert_eq!(Solution::check_inclusion("ab".to_string(), "eidbaooo".to_string()), true);
        assert_eq!(Solution::check_inclusion("ab".to_string(), "eidboaoo".to_string()), false);
        assert_eq!(Solution::check_inclusion("adc".to_string(), "dcda".to_string()), true);
        assert_eq!(Solution::check_inclusion("qff".to_string(), "ifisnoskikfqzrmzlv".to_string()), false);
    }
}

```
