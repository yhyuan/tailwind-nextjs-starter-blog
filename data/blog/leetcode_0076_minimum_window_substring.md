---
title: Leetcode 0076 minimum window substring
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0076 minimum window substring in Rust
---

## Solution in Rust

```rust
/**
 * [76] Minimum Window Substring
 *
 * Given two strings s and t of lengths m and n respectively, return the minimum window substring of s such that every character in t (including duplicates) is included in the window. If there is no such substring, return the empty string "".
 * The testcases will be generated such that the answer is unique.
 * A substring is a contiguous sequence of characters within the string.
 *
 * Example 1:
 *
 * Input: s = "ADOBECODEBANC", t = "ABC"
 * Output: "BANC"
 * Explanation: The minimum window substring "BANC" includes 'A', 'B', and 'C' from string t.
 *
 * Example 2:
 *
 * Input: s = "a", t = "a"
 * Output: "a"
 * Explanation: The entire string s is the minimum window.
 *
 * Example 3:
 *
 * Input: s = "a", t = "aa"
 * Output: ""
 * Explanation: Both 'a's from t must be included in the window.
 * Since the largest window of s only has one 'a', return empty string.
 *
 *
 * Constraints:
 *
 * 	m == s.length
 * 	n == t.length
 * 	1 <= m, n <= 10^5
 * 	s and t consist of uppercase and lowercase English letters.
 *
 *
 * Follow up: Could you find an algorithm that runs in O(m + n) time?
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/minimum-window-substring/
// discuss: https://leetcode.com/problems/minimum-window-substring/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
use std::collections::HashMap;
impl Solution {
    pub fn is_contained(t_hashmap: &HashMap<char, usize>, s_hashmap: &HashMap<char, usize>) -> bool {
        for (k, v) in t_hashmap.iter() {
            if !s_hashmap.contains_key(k) {
                return false;
            }
            if s_hashmap[k] < t_hashmap[k] {
                return false;
            }
        }
        true
    }
    pub fn min_window(s: String, t: String) -> String {
        let s_chars: Vec<char> = s.chars().collect();
        let t_chars: Vec<char> = t.chars().collect();
        let mut t_hashmap: HashMap<char, usize> = HashMap::new();
        for &ch in t_chars.iter() {
            if t_hashmap.contains_key(&ch) {
                t_hashmap.insert(ch, t_hashmap[&ch] + 1);
            } else {
                t_hashmap.insert(ch, 1);
            }
        }
        let mut s_hashmap: HashMap<char, usize> = HashMap::new();
        let mut front = usize::MAX;
        for i in 0..s_chars.len() {
            let ch = s_chars[i];
            if !t_hashmap.contains_key(&ch) {
                continue;
            }
            if s_hashmap.contains_key(&ch) {
                s_hashmap.insert(ch, s_hashmap[&ch] + 1);
            } else {
                s_hashmap.insert(ch, 1);
            }
            if Solution::is_contained(&t_hashmap, &s_hashmap) {
                front = i;
                break;
            }
        }
        if front == usize::MAX {
            return "".to_string();
        }
        let mut back = 0usize;
        //println!("back: {}, front: {}", back, front); //0, 7
        let mut result = (0, 0, usize::MAX); //length is front - back + 1
        //println!("len: {}", front - back + 1);
        loop {
            while Solution::is_contained(&t_hashmap, &s_hashmap) {
                let mut ch = s_chars[back];
                back += 1;
                if !t_hashmap.contains_key(&ch) {
                    continue;
                }
                if s_hashmap[&ch] == 1 {
                    s_hashmap.remove_entry(&ch);
                } else {
                    s_hashmap.insert(ch, s_hashmap[&ch] - 1);
                }
            }
            let new_len = front  + 2 - back;
            if new_len < result.2 {
                result = (back - 1, front, new_len);
            }
            if front == s_chars.len() - 1 {
                break;
            }
            //println!("1 back: {}, front: {}", back, front); //, 3, 7 ** 7, 12 ** 11, 14
            //println!("s_hashmap: {:?}", s_hashmap);
            while front < s_chars.len() - 1 && !Solution::is_contained(&t_hashmap, &s_hashmap) {
                front += 1;
                let ch = s_chars[front];
                if !t_hashmap.contains_key(&ch) {
                    continue;
                }
                if s_hashmap.contains_key(&ch) {
                    s_hashmap.insert(ch, s_hashmap[&ch] + 1);
                } else {
                    s_hashmap.insert(ch, 1);
                }
            }
            //println!("2 back: {}, front: {}", back, front); //, 3, 12 ** 8, 14
            //println!("s_hashmap: {:?}", s_hashmap);
            //println!("back to front: {:?}", &s_chars[back..=front]);
        }
        //println!("len: {}", front - back + 1);
        (result.0..=result.1).into_iter().map(|i| s_chars[i]).collect::<String>()
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_76() {
        assert_eq!(Solution::min_window("EEADOBEC".to_string(), "ABC".to_string()), "ADOBEC".to_string());
        assert_eq!(Solution::min_window("EEADOBECODEBANC".to_string(), "ABC".to_string()), "BANC".to_string());
        assert_eq!(Solution::min_window("a".to_string(), "a".to_string()), "a".to_string());
        assert_eq!(Solution::min_window("a".to_string(), "aa".to_string()), "".to_string());
    }
}

```
