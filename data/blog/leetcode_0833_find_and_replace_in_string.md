---
title: Leetcode 0833 find and replace in string
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0833 find and replace in string in Rust
---

## Solution in Rust

```rust
/**
 * [833] Find And Replace in String
 *
 * You are given a 0-indexed string s that you must perform k replacement operations on. The replacement operations are given as three 0-indexed parallel arrays, indices, sources, and targets, all of length k.
 * To complete the i^th replacement operation:
 * <ol>
 * 	Check if the substring sources[i] occurs at index indices[i] in the original string s.
 * 	If it does not occur, do nothing.
 * 	Otherwise if it does occur, replace that substring with targets[i].
 * </ol>
 * For example, if s = "<u>ab</u>cd", indices[i] = 0, sources[i] = "ab", and targets[i] = "eee", then the result of this replacement will be "<u>eee</u>cd".
 * All replacement operations must occur simultaneously, meaning the replacement operations should not affect the indexing of each other. The testcases will be generated such that the replacements will not overlap.
 *
 * 	For example, a testcase with s = "abc", indices = [0, 1], and sources = ["ab","bc"] will not be generated because the "ab" and "bc" replacements overlap.
 *
 * Return the resulting string after performing all replacement operations on s.
 * A substring is a contiguous sequence of characters in a string.
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2021/06/12/833-ex1.png" style="width: 411px; height: 251px;" />
 * Input: s = "abcd", indices = [0, 2], sources = ["a", "cd"], targets = ["eee", "ffff"]
 * Output: "eeebffff"
 * Explanation:
 * "a" occurs at index 0 in s, so we replace it with "eee".
 * "cd" occurs at index 2 in s, so we replace it with "ffff".
 *
 * Example 2:
 * <img alt="" src="https://assets.leetcode.com/uploads/2021/06/12/833-ex2-1.png" style="width: 411px; height: 251px;" />
 * Input: s = "abcd", indices = [0, 2], sources = ["ab","ec"], targets = ["eee","ffff"]
 * Output: "eeecd"
 * Explanation:
 * "ab" occurs at index 0 in s, so we replace it with "eee".
 * "ec" does not occur at index 2 in s, so we do nothing.
 *
 *
 * Constraints:
 *
 * 	1 <= s.length <= 1000
 * 	k == indices.length == sources.length == targets.length
 * 	1 <= k <= 100
 * 	0 <= indexes[i] < s.length
 * 	1 <= sources[i].length, targets[i].length <= 50
 * 	s consists of only lowercase English letters.
 * 	sources[i] and targets[i] consist of only lowercase English letters.
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/find-and-replace-in-string/
// discuss: https://leetcode.com/problems/find-and-replace-in-string/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
use std::collections::HashMap;

impl Solution {
    pub fn find_replace_string(s: String, indices: Vec<i32>, sources: Vec<String>, targets: Vec<String>) -> String {
        // String::new()
        let mut chars: Vec<char> = s.chars().collect();
        let n = indices.len();
        let mut replace_dict: HashMap<(usize, usize), String> = HashMap::new();
        // let mut items: Vec<(usize, bool, Vec<char>, Vec<char>)> = Vec::new();
        let mut changed_intervals: Vec<(usize, usize)> = vec![];
        for i in 0..n {
            let index = indices[i] as usize;
            let source_chars: Vec<char> = sources[i].chars().collect();
            let mut success = true;
            for j in 0..source_chars.len() {
                if index + j == chars.len() {
                    success = false;
                    break;
                }
                if chars[index + j] != source_chars[j] {
                    success = false;
                    break;
                }
            }
            if success {
                replace_dict.insert((index, index + source_chars.len() - 1), targets[i].clone());
                changed_intervals.push((index, index + source_chars.len() - 1));
            }
        }
        if changed_intervals.is_empty() {
            return s;
        }
        //println!("changed_intervals: {:?}", changed_intervals);
        changed_intervals.sort();

        let mut change_index = 0;
        let mut results:Vec<char> = vec![];
        let mut i = 0usize;
        while i < chars.len() {
        // for i in 0..chars.len() {
            if i < changed_intervals[change_index].0 {
                results.push(chars[i]);
            } else if i == changed_intervals[change_index].0 {
                let target_chars: Vec<char> = replace_dict[&changed_intervals[change_index]].chars().collect();
                for j in 0..target_chars.len() {
                    results.push(target_chars[j]);
                }
                i = changed_intervals[change_index].1;
                change_index += 1;
            }
            i += 1;
            if change_index >= changed_intervals.len() {
                break;
            }
        }
        for j in i..chars.len() {
            results.push(chars[j]);
        }
        results.into_iter().collect::<String>()
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_833() {
        assert_eq!(Solution::find_replace_string("abcde".to_string(), vec![2, 2], vec_string!["cdef", "bc"], vec_string!["f", "fe"]), "abcde".to_string());
        //assert_eq!(Solution::find_replace_string("jjievdtjfb".to_string(), vec![4, 6, 1], vec_string!["md", "tjgb", "jf"], vec_string!["foe", "oov", "e"]), "jjievdtjfb".to_string());
        //assert_eq!(Solution::find_replace_string("abcd".to_string(), vec![0, 2], vec_string!["a", "cd"], vec_string!["eee", "ffff"]), "eeebffff".to_string());
    }
}

```
