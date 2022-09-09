---
title: Leetcode 1554 strings differ by one character
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 1554 strings differ by one character in Rust
---

## Solution in Rust

```rust
/**
1554. Strings Differ by One Character
Medium

296

60

Add to List

Share
Given a list of strings dict where all the strings are of the same length.

Return true if there are 2 strings that only differ by 1 character in the same index, otherwise return false.



Example 1:

Input: dict = ["abcd","acbd", "aacd"]
Output: true
Explanation: Strings "abcd" and "aacd" differ only by one character in the index 1.
Example 2:

Input: dict = ["ab","cd","yz"]
Output: false
Example 3:

Input: dict = ["abcd","cccc","abyd","abab"]
Output: true


Constraints:

The number of characters in dict <= 105
dict[i].length == dict[j].length
dict[i] should be unique.
dict[i] contains only lowercase English letters.


Follow up: Could you solve this problem in O(n * m) where n is the length of dict and m is the length of each string.
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/strings-differ-by-one-character/
// discuss: https://leetcode.com/problems/strings-differ-by-one-character/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
use std::collections::HashSet;
impl Solution {
    pub fn differ_by_one(dict: Vec<String>) -> bool {
        let mut dict = dict;
        let n = dict.len();
        let m = dict[0].len();
        let mut chars_dict: Vec<Vec<char>> = vec![];
        for i in 0..n {
            chars_dict.push(dict[i].chars().collect::<Vec<_>>());
        }
        for j in 0..m {
            let mut hashset: HashSet<String> = HashSet::new();
            for i in 0..n {
                let ch = chars_dict[i][j];
                chars_dict[i][j] = '_';
                let result = chars_dict[i].iter().collect::<String>();
                if hashset.contains(&result) {
                    return true;
                }
                hashset.insert(result);
                chars_dict[i][j] = ch;
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
    fn test_1554() {
        assert_eq!(Solution::differ_by_one(vec_string!["abcd","acbd", "aacd"]), true);
        assert_eq!(Solution::differ_by_one(vec_string!["ab","cd","yz"]), false);
        assert_eq!(Solution::differ_by_one(vec_string!["abcd","cccc","abyd","abab"]), true);
    }
}

```
