---
title: Leetcode 1249 minimum remove to make valid parentheses
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 1249 minimum remove to make valid parentheses in Rust
---

## Solution in Rust

```rust
/**
 * [1249] Minimum Remove to Make Valid Parentheses
 *
 * Given a string <font face="monospace">s</font> of '(' , ')' and lowercase English characters.
 *
 * Your task is to remove the minimum number of parentheses ( '(' or ')', in any positions ) so that the resulting parentheses string is valid and return any valid string.
 *
 * Formally, a parentheses string is valid if and only if:
 *
 *
 * 	It is the empty string, contains only lowercase characters, or
 * 	It can be written as AB (A concatenated with B), where A and B are valid strings, or
 * 	It can be written as (A), where A is a valid string.
 *
 *
 *
 * Example 1:
 *
 *
 * Input: s = "lee(t(c)o)de)"
 * Output: "lee(t(c)o)de"
 * Explanation: "lee(t(co)de)" , "lee(t(c)ode)" would also be accepted.
 *
 *
 * Example 2:
 *
 *
 * Input: s = "a)b(c)d"
 * Output: "ab(c)d"
 *
 *
 * Example 3:
 *
 *
 * Input: s = "))(("
 * Output: ""
 * Explanation: An empty string is also valid.
 *
 *
 * Example 4:
 *
 *
 * Input: s = "(a(b(c)d)"
 * Output: "a(b(c)d)"
 *
 *
 *
 * Constraints:
 *
 *
 * 	1 <= s.length <= 10^5
 * 	s[i] is one of  '(' , ')' and lowercase English letters.
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/minimum-remove-to-make-valid-parentheses/
// discuss: https://leetcode.com/problems/minimum-remove-to-make-valid-parentheses/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
use std::collections::HashSet;
impl Solution {
    pub fn min_remove_to_make_valid(s: String) -> String {
        let mut removed_index_set: HashSet<usize> = HashSet::new();
        let mut stack: Vec<usize> = vec![];
        for (i, char) in s.chars().enumerate() {
            if char == '('  || char == ')' {
                if char == '(' {
                    stack.push(i);
                } else if stack.len() == 0 {
                    removed_index_set.insert(i);
                } else {
                    stack.pop();
                }
            }
        }
        for i in 0..stack.len() {
            removed_index_set.insert(stack[i]);
        }
        let mut res: Vec<char> = vec![];
        for (i, char) in s.chars().enumerate() {
            if removed_index_set.contains(&i) {
                continue;
            }
            res.push(char);
        }
        res.iter().collect::<String>()
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_1249() {
        assert_eq!(Solution::min_remove_to_make_valid("lee(t(c)o)de)".to_string()), "lee(t(c)o)de".to_string());
        assert_eq!(Solution::min_remove_to_make_valid("a)b(c)d".to_string()), "ab(c)d".to_string());
        assert_eq!(Solution::min_remove_to_make_valid("))((".to_string()), "".to_string());
        assert_eq!(Solution::min_remove_to_make_valid("(a(b(c)d)".to_string()), "a(b(c)d)".to_string());
        assert_eq!(Solution::min_remove_to_make_valid(")((c)d()(l".to_string()), "(c)d()l".to_string());
    }
}


```
