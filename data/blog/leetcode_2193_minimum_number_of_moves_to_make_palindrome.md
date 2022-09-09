---
title: Leetcode 2193 minimum number of moves to make palindrome
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 2193 minimum number of moves to make palindrome in Rust
---

## Solution in Rust

```rust
/*
2193. Minimum Number of Moves to Make Palindrome
Hard

36

35

Add to List

Share
You are given a string s consisting only of lowercase English letters.

In one move, you can select any two adjacent characters of s and swap them.

Return the minimum number of moves needed to make s a palindrome.

Note that the input will be generated such that s can always be converted to a palindrome.



Example 1:

Input: s = "aabb"
Output: 2
Explanation:
We can obtain two palindromes from s, "abba" and "baab".
- We can obtain "abba" from s in 2 moves: "aabb" -> "abab" -> "abba".
- We can obtain "baab" from s in 2 moves: "aabb" -> "abab" -> "baab".
Thus, the minimum number of moves needed to make s a palindrome is 2.
Example 2:

Input: s = "letelt"
Output: 2
Explanation:
One of the palindromes we can obtain from s in 2 moves is "lettel".
One of the ways we can obtain it is "letelt" -> "letetl" -> "lettel".
Other palindromes such as "tleelt" can also be obtained in 2 moves.
It can be shown that it is not possible to obtain a palindrome in less than 2 moves.


Constraints:

1 <= s.length <= 2000
s consists only of lowercase English letters.
s can be converted to a palindrome using a finite number of moves.*/
// use std::collections::HashMap;
pub struct Solution {}

impl Solution {
    pub fn helper(chars: &mut Vec<char>, count: &mut i32, start: usize, end: usize) {
        if start >= end {
            return;
        }
        if chars[start] == chars[end] {
            return Self::helper(chars, count, start + 1, end - 1);
        }

        let mut end_index = end;
        while end >= start && chars[end_index] != chars[start] {
            end_index -= 1;
        }
        let from_end_steps = end - end_index;

        let mut start_index = start;
        while end >= start && chars[start_index] != chars[end] {
            start_index += 1;
        }
        let from_start_steps = start_index - start;

        if from_end_steps < from_start_steps {
            let ch = chars[start];
            for i in end_index..end {
                chars[i] = chars[i + 1];
                *count += 1;
            }
            chars[end] = ch;
        } else {
            let ch = chars[end];
            for i in (start + 1..=start_index).rev() {
                chars[i] = chars[i - 1];
                *count += 1;
            }
            chars[start] = ch;

        }
        return Self::helper(chars, count, start + 1, end - 1);
    }
    pub fn min_moves_to_make_palindrome(s: String) -> i32 {
        let n = s.len();
        let mut chars: Vec<char> = s.chars().collect();
        let mut count = 0i32;
        let start = 0usize;
        let end = n - 1;
        Self::helper(&mut chars, &mut count, start, end);
        count
    }
}

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_2193() {
        assert_eq!(Solution::min_moves_to_make_palindrome("aabb".to_string()), 2);
        assert_eq!(Solution::min_moves_to_make_palindrome("letelt".to_string()), 2);
        assert_eq!(Solution::min_moves_to_make_palindrome("skwhhaaunskegmdtutlgtteunmuuludii".to_string()), 163);
    }
}


```
