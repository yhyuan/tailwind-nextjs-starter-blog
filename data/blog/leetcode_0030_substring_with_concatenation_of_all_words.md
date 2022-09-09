---
title: Leetcode 0030 substring with concatenation of all words
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0030 substring with concatenation of all words in Rust
---

## Solution in Rust

```rust
/**
 * [30] Substring with Concatenation of All Words
 *
 * You are given a string s and an array of strings words of the same length. Return all starting indices of substring(s) in s that is a concatenation of each word in words exactly once, in any order, and without any intervening characters.
 * You can return the answer in any order.
 *
 * Example 1:
 *
 * Input: s = "barfoothefoobarman", words = ["foo","bar"]
 * Output: [0,9]
 * Explanation: Substrings starting at index 0 and 9 are "barfoo" and "foobar" respectively.
 * The output order does not matter, returning [9,0] is fine too.
 *
 * Example 2:
 *
 * Input: s = "wordgoodgoodgoodbestword", words = ["word","good","best","word"]
 * Output: []
 *
 * Example 3:
 *
 * Input: s = "barfoofoobarthefoobarman", words = ["bar","foo","the"]
 * Output: [6,9,12]
 *
 *
 * Constraints:
 *
 * 	1 <= s.length <= 10^4
 * 	s consists of lower-case English letters.
 * 	1 <= words.length <= 5000
 * 	1 <= words[i].length <= 30
 * 	words[i] consists of lower-case English letters.
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/substring-with-concatenation-of-all-words/
// discuss: https://leetcode.com/problems/substring-with-concatenation-of-all-words/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
use std::collections::HashMap;
impl Solution {
    pub fn create_words_hashmap(words: &Vec<String>) -> HashMap<String, usize> {
        let word_size = words[0].len();
        let mut hashmap: HashMap<String, usize> = HashMap::new();
        for word in words.iter() {
            assert!(word.len() == word_size);
            if hashmap.contains_key(word) {
                hashmap.insert(word.to_string(), hashmap[word] + 1);
            } else {
                hashmap.insert(word.to_string(), 1);
            }
        }
        hashmap
    }
    pub fn create_string_hashmap(s: &String, i: usize, words_len: usize, w_len: usize) -> HashMap<String, usize> {
        let mut hashmap: HashMap<String, usize> = HashMap::new();
        for j in 0..words_len {
            let sub_string = &s[i + j * w_len..i + (j + 1) * w_len];
            //let word = sub_string.to_string();
            if hashmap.contains_key(sub_string) {
                hashmap.insert(sub_string.to_string(), hashmap[sub_string] + 1);
            } else {
                hashmap.insert(sub_string.to_string(), 1);
            }
        }
        hashmap
    }
    pub fn compare_hashmap(words_hashmap: &HashMap<String, usize>, string_hashmap: &HashMap<String, usize>) -> bool {
        if words_hashmap.len() != string_hashmap.len() {
            return false;
        }
        for (word_key, &word_val) in words_hashmap.iter() {
            if !string_hashmap.contains_key(word_key) {
                return false;
            }
            let string_val = string_hashmap[word_key];
            if word_val != string_val {
                return false;
            }
        }
        for (string_key, &string_val) in string_hashmap.iter() {
            if !words_hashmap.contains_key(string_key) {
                return false;
            }
            let word_val = words_hashmap[string_key];
            if word_val != string_val {
                return false;
            }
        }
        true
    }
    pub fn find_substring(s: String, words: Vec<String>) -> Vec<i32> {
        let w_len = words[0].len();
        let words_size = words.len() * words[0].len();
        if s.len() < words_size {
            return vec![];
        }
        let words_hashmap = Solution::create_words_hashmap(&words);
        let mut results: Vec<i32> = vec![];
        for i in 0..=s.len() - words_size {
            let string_hashmap = Solution::create_string_hashmap(&s, i, words.len(), w_len);
            if Solution::compare_hashmap(&words_hashmap, &string_hashmap) {
                results.push(i as i32);
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
    fn test_30() {
        let empty: Vec<i32> = vec![];
        assert_eq!(Solution::find_substring("barfoothefoobarman".to_string(), vec!["foo".to_string(),"bar".to_string()]), vec![0, 9]);
        assert_eq!(Solution::find_substring("wordgoodgoodgoodbestword".to_string(), vec!["word".to_string(),"good".to_string(), "best".to_string(),"word".to_string()]), empty);
        assert_eq!(Solution::find_substring("barfoofoobarthefoobarman".to_string(), vec!["bar".to_string(), "foo".to_string(), "the".to_string(),]), vec![6, 9, 12]);
        assert_eq!(Solution::find_substring("aaaaaaaaaaaaaa".to_string(), vec!["aa".to_string(), "aa".to_string()]), vec![0,1,2,3,4,5,6,7,8,9,10]);
        assert_eq!(Solution::find_substring("a".to_string(), vec!["a".to_string(), "a".to_string()]), empty);
    }
}

```
