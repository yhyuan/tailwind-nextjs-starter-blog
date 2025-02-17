---
title: Leetcode 0097 interleaving string
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0097 interleaving string in Rust
---

## Solution in Rust

```rust
/**
 * [97] Interleaving String
 *
 * Given strings s1, s2, and s3, find whether s3 is formed by an interleaving of s1 and s2.
 * An interleaving of two strings s and t is a configuration where they are divided into non-empty substrings such that:
 *
 * 	s = s1 + s2 + ... + sn
 * 	t = t1 + t2 + ... + tm
 * 	|n - m| <= 1
 * 	The interleaving is s1 + t1 + s2 + t2 + s3 + t3 + ... or t1 + s1 + t2 + s2 + t3 + s3 + ...
 *
 * Note: a + b is the concatenation of strings a and b.
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2020/09/02/interleave.jpg" style="width: 561px; height: 203px;" />
 * Input: s1 = "aabcc", s2 = "dbbca", s3 = "aadbbcbcac"
 * Output: true
 *
 * Example 2:
 *
 * Input: s1 = "aabcc", s2 = "dbbca", s3 = "aadbbbaccc"
 * Output: false
 *
 * Example 3:
 *
 * Input: s1 = "", s2 = "", s3 = ""
 * Output: true
 *
 *
 * Constraints:
 *
 * 	0 <= s1.length, s2.length <= 100
 * 	0 <= s3.length <= 200
 * 	s1, s2, and s3 consist of lowercase English letters.
 *
 *
 * Follow up: Could you solve it using only O(s2.length) additional memory space?
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/interleaving-string/
// discuss: https://leetcode.com/problems/interleaving-string/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
use std::collections::HashMap;
impl Solution {
    pub fn find_common_length(s1_chars: &Vec<char>, s1_index: usize, s3_chars: &Vec<char>, s3_index: usize) -> usize {
        let mut index = 0usize;

        while s1_index + index < s1_chars.len() && s3_index + index < s3_chars.len() && s1_chars[s1_index + index] == s3_chars[s3_index + index] {
            index += 1;
        }
        index
    }
    pub fn is_interleave_helper(s1_chars: &Vec<char>, s1_index: usize, s2_chars: &Vec<char>, s2_index: usize, s3_chars: &Vec<char>, s3_index: usize, hashmap: &mut HashMap<(usize, usize), bool>) -> bool {
        if hashmap.contains_key(&(s1_index, s2_index)) {
            return hashmap[&(s1_index, s2_index)];
        }
        if s1_index == s1_chars.len() {
            let s2_common_len = Solution::find_common_length(s2_chars, s2_index, s3_chars, s3_index);
            hashmap.insert((s1_index, s2_index), s2_common_len == s2_chars.len() - s2_index);
            return s2_common_len == s2_chars.len() - s2_index;
        }
        if s2_index == s2_chars.len() {
            let s1_common_len = Solution::find_common_length(s1_chars, s1_index, s3_chars, s3_index);
            hashmap.insert((s1_index, s2_index), s1_common_len == s1_chars.len() - s1_index);
            return s1_common_len == s1_chars.len() - s1_index;
        }
        let s1_common_len = Solution::find_common_length(s1_chars, s1_index, s3_chars, s3_index);
        //println!("s1 chars: {:?}, s1_common_len: {}", &s1_chars[s1_index..], s1_common_len);
        //println!("s2 chars: {:?}, s3 chars: {:?}", &s2_chars[s2_index..], &s3_chars[s3_index..]);
        if s1_common_len == 0 {
            hashmap.insert((s1_index, s2_index), false);
            return false;
        }
        for i in 1..=s1_common_len {
            let s2_common_len = Solution::find_common_length(s2_chars, s2_index, s3_chars, s3_index + i);
            if s2_common_len > 0 {
                for j in 1..=s2_common_len {
                    let result = Solution::is_interleave_helper(s1_chars, s1_index + i, s2_chars, s2_index + j, s3_chars, s3_index + i + j, hashmap);
                    hashmap.insert((s1_index + i, s2_index + j), result);
                    if result {
                        return true;
                    }
                }
            }
        }
        false
    }
    pub fn is_interleave(s1: String, s2: String, s3: String) -> bool {
        if s1.len() == 0 && s2.len() == 0 && s3.len() == 0 {
            return true;
        }
        if s1.len() + s2.len() != s3.len() {
            return false;
        }
        let s1_chars:Vec<char> = s1.chars().collect();
        let s2_chars:Vec<char> = s2.chars().collect();
        let s3_chars:Vec<char> = s3.chars().collect();
        let mut hashmap: HashMap<(usize, usize), bool> = HashMap::new();
        let result_1 = Solution::is_interleave_helper(&s1_chars, 0, &s2_chars, 0, &s3_chars, 0, &mut hashmap);
        hashmap.clear();
        let result_2 = Solution::is_interleave_helper(&s2_chars, 0, &s1_chars, 0, &s3_chars, 0, &mut hashmap);
        //println!("result 1: {}, result 2: {}", result_1, result_2);
        result_1 || result_2
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_97() {
        assert_eq!(
            Solution::is_interleave(
                "aabcc".to_owned(),
                "dbbca".to_owned(),
                "aadbbcbcac".to_owned()
            ),
            true
        );
        assert_eq!(
            Solution::is_interleave(
                "aabcc".to_owned(),
                "dbbca".to_owned(),
                "aadbbbaccc".to_owned()
            ),
            false
        );
        assert_eq!(
            Solution::is_interleave("a".to_owned(), "b".to_owned(), "a".to_owned()),
            false
        );
        assert_eq!(
            Solution::is_interleave(
                "abababababababababababababababababababababababababababababababababababababababababababababababababbb".to_owned(),
                "babababababababababababababababababababababababababababababababababababababababababababababababaaaba".to_owned(),
                "abababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababababbb".to_owned()),
            false
        );
        assert_eq!(
            Solution::is_interleave("ab".to_owned(), "bc".to_owned(), "bcab".to_owned()),
            true
        );
    }
}

```
