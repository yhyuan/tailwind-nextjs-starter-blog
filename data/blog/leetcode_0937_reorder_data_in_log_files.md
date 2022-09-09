---
title: Leetcode 0937 reorder data in log files
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0937 reorder data in log files in Rust
---

## Solution in Rust

```rust
/**
 * [937] Reorder Data in Log Files
 *
 * You are given an array of logs. Each log is a space-delimited string of words, where the first word is the identifier.
 * There are two types of logs:
 *
 * 	Letter-logs: All words (except the identifier) consist of lowercase English letters.
 * 	Digit-logs: All words (except the identifier) consist of digits.
 *
 * Reorder these logs so that:
 * <ol>
 * 	The letter-logs come before all digit-logs.
 * 	The letter-logs are sorted lexicographically by their contents. If their contents are the same, then sort them lexicographically by their identifiers.
 * 	The digit-logs maintain their relative ordering.
 * </ol>
 * Return the final order of the logs.
 *
 * Example 1:
 *
 * Input: logs = ["dig1 8 1 5 1","let1 art can","dig2 3 6","let2 own kit dig","let3 art zero"]
 * Output: ["let1 art can","let3 art zero","let2 own kit dig","dig1 8 1 5 1","dig2 3 6"]
 * Explanation:
 * The letter-log contents are all different, so their ordering is "art can", "art zero", "own kit dig".
 * The digit-logs have a relative order of "dig1 8 1 5 1", "dig2 3 6".
 *
 * Example 2:
 *
 * Input: logs = ["a1 9 2 3 1","g1 act car","zo4 4 7","ab1 off key dog","a8 act zoo"]
 * Output: ["g1 act car","a8 act zoo","ab1 off key dog","a1 9 2 3 1","zo4 4 7"]
 *
 *
 * Constraints:
 *
 * 	1 <= logs.length <= 100
 * 	3 <= logs[i].length <= 100
 * 	All the tokens of logs[i] are separated by a single space.
 * 	logs[i] is guaranteed to have an identifier and at least one word after the identifier.
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/reorder-data-in-log-files/
// discuss: https://leetcode.com/problems/reorder-data-in-log-files/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
use std::cmp::Ordering;
impl Solution {
    pub fn is_digital_log(log: &String) -> bool {
        let items = log.split_ascii_whitespace().collect::<Vec<&str>>();
        if items.len() <= 1 {
            return false;
        }
        let digits = vec!['0', '1', '2', '3', '4', '5', '6', '7', '8', '9'];
        for i in 1..items.len() {
            let item = items[i];
            for ch in item.chars() {
                if !digits.contains(&ch) {
                    return false;
                }
            }
        }
        true
    }
    pub fn compare_letter_log(log1: &String, log2: &String) -> Option<Ordering> {
        let index1 = log1.chars().position(|c| c == ' ').unwrap();
        let index2 = log2.chars().position(|c| c == ' ').unwrap();
        let substring1 = &log1[index1..];
        let substring2 = &log2[index2..];
        if substring1 == substring2 {
            let id1 = &log1[..index1];
            let id2 = &log2[..index2];
            id1.partial_cmp(id2)
        } else {
            substring1.partial_cmp(substring2)
        }
    }
    pub fn reorder_log_files(logs: Vec<String>) -> Vec<String> {
        let digital_logs: Vec<&String> = logs.iter().filter(|&log| Solution::is_digital_log(log)).collect();
        let mut letter_logs: Vec<&String> = logs.iter().filter(|&log| !Solution::is_digital_log(log)).collect();
        letter_logs.sort_by(|&a, &b| Solution::compare_letter_log(a, b).unwrap());
        for &log in digital_logs.iter() {
            letter_logs.push(log);
        }
        let results: Vec<String> = letter_logs.iter().map(|&log| log.to_string()).collect();
        results
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_937() {
        assert_eq!(Solution::reorder_log_files(vec!["dig1 8 1 5 1".to_string(),"let1 art can".to_string(),"dig2 3 6".to_string(),"let2 own kit dig".to_string(),"let3 art zero".to_string()]), vec!["let1 art can".to_string(),"let3 art zero".to_string(),"let2 own kit dig".to_string(),"dig1 8 1 5 1".to_string(),"dig2 3 6".to_string()])
    }
}

```
