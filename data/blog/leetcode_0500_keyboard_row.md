---
title: Leetcode 0500 keyboard row
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0500 keyboard row in Rust
---

## Solution in Rust

```rust
/**
 * [500] Keyboard Row
 *
 * Given an array of strings words, return the words that can be typed using letters of the alphabet on only one row of American keyboard like the image below.
 * In the American keyboard:
 *
 * 	the first row consists of the characters "qwertyuiop",
 * 	the second row consists of the characters "asdfghjkl", and
 * 	the third row consists of the characters "zxcvbnm".
 * <img alt="" src="https://assets.leetcode.com/uploads/2018/10/12/keyboard.png" style="width: 800px; max-width: 600px; height: 267px;" />
 *
 * Example 1:
 *
 * Input: words = ["Hello","Alaska","Dad","Peace"]
 * Output: ["Alaska","Dad"]
 *
 * Example 2:
 *
 * Input: words = ["omk"]
 * Output: []
 *
 * Example 3:
 *
 * Input: words = ["adsdf","sfd"]
 * Output: ["adsdf","sfd"]
 *
 *
 * Constraints:
 *
 * 	1 <= words.length <= 20
 * 	1 <= words[i].length <= 100
 * 	words[i] consists of English letters (both lowercase and uppercase).
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/keyboard-row/
// discuss: https://leetcode.com/problems/keyboard-row/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
use std::collections::HashMap;
impl Solution {
    pub fn find_words(words: Vec<String>) -> Vec<String> {
        let first_row = "qwertyuiop";
        let second_row = "asdfghjkl";
        let thrid_row = "zxcvbnm";
        let mut hashmap: HashMap<char, usize> = HashMap::new();
        for ch in first_row.chars() {
            hashmap.insert(ch, 1);
        }
        for ch in second_row.chars() {
            hashmap.insert(ch, 2);
        }
        for ch in thrid_row.chars() {
            hashmap.insert(ch, 3);
        }
        let mut results: Vec<String> = vec![];
        for word in words {
            let low_words = word.to_lowercase();
            let chars = low_words.chars();
            let mut row: Option<usize> = None;
            let mut is_same_row = true;
            for ch in chars {
                if row.is_none() {
                    row = Some(hashmap[&ch]);
                } else {
                    let row_hashmap = hashmap[&ch];
                    if row_hashmap != row.unwrap() {
                        is_same_row = false;
                        break;
                    }
                }
            }
            if is_same_row {
                results.push(word);
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
    fn test_500() {
        assert_eq!(Solution::find_words(vec!["Hello".to_string(),"Alaska".to_string(),"Dad".to_string(),"Peace".to_string()]), vec!["Alaska".to_string(),"Dad".to_string()])
    }
}

```
