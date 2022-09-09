---
title: Leetcode 0438 find all anagrams in a string
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0438 find all anagrams in a string in Rust
---

## Solution in Rust

```rust
/**
 * [438] Find All Anagrams in a String
 *
 * Given two strings s and p, return an array of all the start indices of p's anagrams in s. You may return the answer in any order.
 * An Anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once.
 *
 * Example 1:
 *
 * Input: s = "cbaebabacd", p = "abc"
 * Output: [0,6]
 * Explanation:
 * The substring with start index = 0 is "cba", which is an anagram of "abc".
 * The substring with start index = 6 is "bac", which is an anagram of "abc".
 *
 * Example 2:
 *
 * Input: s = "abab", p = "ab"
 * Output: [0,1,2]
 * Explanation:
 * The substring with start index = 0 is "ab", which is an anagram of "ab".
 * The substring with start index = 1 is "ba", which is an anagram of "ab".
 * The substring with start index = 2 is "ab", which is an anagram of "ab".
 *
 *
 * Constraints:
 *
 * 	1 <= s.length, p.length <= 3 * 10^4
 * 	s and p consist of lowercase English letters.
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/find-all-anagrams-in-a-string/
// discuss: https://leetcode.com/problems/find-all-anagrams-in-a-string/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn is_all_same(counts: &[i32; 26], cur_counts: &[i32; 26]) -> bool {
        for i in 0..26 {
            if counts[i] != cur_counts[i] {
                return false;
            }
        }
        true
    }
    pub fn find_anagrams(s: String, p: String) -> Vec<i32> {
        if p.len() > s.len() {
            return vec![];
        }
        let mut counts = [0; 26];
        for ch in p.chars() {
            let index = (ch as usize - 'a' as usize);
            counts[index] += 1;
        }
        let chars: Vec<char> = s.chars().collect();
        let mut cur_counts = [0; 26];
        for i in 0..p.len() {
            let index = chars[i] as usize - 'a' as usize;
            cur_counts[index] += 1;
        }
        let mut results: Vec<i32> = vec![];
        if Self::is_all_same(&counts, &cur_counts) {
            results.push(0);
        }
        for i in p.len()..s.len() {
            let enter_index = chars[i] as usize - 'a' as usize;
            let remove_index = chars[i - p.len()] as usize - 'a' as usize;
            cur_counts[enter_index] += 1;
            cur_counts[remove_index] -= 1;
            // println!("i: {}, cur_counts: {:?}", i, cur_counts);
            if Self::is_all_same(&counts, &cur_counts) {
                results.push((i - p.len() + 1) as i32);
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
    fn test_438() {
        assert_eq!(Solution::find_anagrams("cbaebabacd".to_string(), "abc".to_string()), vec![0, 6]);
    }
}

```
