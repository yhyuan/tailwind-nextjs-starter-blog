---
title: Leetcode 0068 text justification
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0068 text justification in Rust
---

## Solution in Rust

```rust
/**
 * [68] Text Justification
 *
 * Given an array of strings words and a width maxWidth, format the text such that each line has exactly maxWidth characters and is fully (left and right) justified.
 * You should pack your words in a greedy approach; that is, pack as many words as you can in each line. Pad extra spaces ' ' when necessary so that each line has exactly maxWidth characters.
 * Extra spaces between words should be distributed as evenly as possible. If the number of spaces on a line does not divide evenly between words, the empty slots on the left will be assigned more spaces than the slots on the right.
 * For the last line of text, it should be left-justified and no extra space is inserted between words.
 * Note:
 *
 * 	A word is defined as a character sequence consisting of non-space characters only.
 * 	Each word's length is guaranteed to be greater than 0 and not exceed maxWidth.
 * 	The input array words contains at least one word.
 *
 *
 * Example 1:
 *
 * Input: words = ["This", "is", "an", "example", "of", "text", "justification."], maxWidth = 16
 * Output:
 * [
 *    "This    is    an",
 *    "example  of text",
 *    "justification.  "
 * ]
 * Example 2:
 *
 * Input: words = ["What","must","be","acknowledgment","shall","be"], maxWidth = 16
 * Output:
 * [
 *   "What   must   be",
 *   "acknowledgment  ",
 *   "shall be        "
 * ]
 * Explanation: Note that the last line is "shall be    " instead of "shall     be", because the last line must be left-justified instead of fully-justified.
 * Note that the second line is also left-justified becase it contains only one word.
 * Example 3:
 *
 * Input: words = ["Science","is","what","we","understand","well","enough","to","explain","to","a","computer.","Art","is","everything","else","we","do"], maxWidth = 20
 * Output:
 * [
 *   "Science  is  what we",
 *   "understand      well",
 *   "enough to explain to",
 *   "a  computer.  Art is",
 *   "everything  else  we",
 *   "do                  "
 * ]
 *
 * Constraints:
 *
 * 	1 <= words.length <= 300
 * 	1 <= words[i].length <= 20
 * 	words[i] consists of only English letters and symbols.
 * 	1 <= maxWidth <= 100
 * 	words[i].length <= maxWidth
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/text-justification/
// discuss: https://leetcode.com/problems/text-justification/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn full_justify_helper(words: &Vec<String>, index: usize, max_width: usize) -> Vec<String> {
        if index >= words.len() {
            return vec![];
        }
        let max_width = max_width as usize;
        let mut min_total_length = 0;
        let mut k = usize::MAX;
        for i in index..words.len() {
            min_total_length += words[i].len() + 1;
            //println!("min_total_length: {}, words[index]: {}, max_width + 1: {}", min_total_length, words[index], max_width + 1);
            if min_total_length > max_width + 1 {
                k = i;
                break;
            }
        }
        if k == usize::MAX {
            let words: Vec<String> = (index..words.len()).into_iter().map(|i| words[i].clone()).collect();
            let mut result = words.join(" ");
            while result.len() < max_width {
                result.push(' ');
            }
            return vec![result];
        }
        let line_words: Vec<String> = (index..k).into_iter().map(|i| words[i].clone()).collect();
        let number_of_letters: usize = line_words.iter().map(|word| word.len()).sum();
        if line_words.len() == 1 {
            let mut result = line_words.join(" ");
            while result.len() < max_width {
                result.push(' ');
            }
            let mut results = Solution::full_justify_helper(words, index + 1, max_width);
            results.insert(0, result);
            return results;
        }
        //println!("line_words: {:?}, number_of_letters: {}, spaces: {}", line_words, number_of_letters, max_width - number_of_letters);

        let space_postions = line_words.len() - 1;
        let spaces = max_width - number_of_letters;
        //println!("spaces / space_postions: {}, spaces % space_postions: {}", spaces / space_postions, spaces % space_postions);
        // spaces / space_postions, spaces % space_postions
        let mut new_line_words: Vec<String> = vec![];
        for i in 0..line_words.len()-1 {
            new_line_words.push(line_words[i].clone());
            let min_number_of_spaces = spaces / space_postions;
            let num_of_spaces = if i < spaces % space_postions {
                min_number_of_spaces + 1
            } else {
                min_number_of_spaces
            };
            //println!("i: {} num_of_spaces: {}", i, num_of_spaces);
            let space_string: String = (0..num_of_spaces).into_iter().map(|_| ' ').collect();
            new_line_words.push(space_string);
        }

        new_line_words.push(line_words[line_words.len() - 1].clone());
        let result = new_line_words.join("");
        let mut results = Solution::full_justify_helper(words, k, max_width);
        results.insert(0, result);
        results
    }
    pub fn full_justify(words: Vec<String>, max_width: i32) -> Vec<String> {
        Solution::full_justify_helper(&words, 0, max_width as usize)
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_68() {
        assert_eq!(
            Solution::full_justify(
                vec_string![
                    "This",
                    "is",
                    "an",
                    "example",
                    "of",
                    "text",
                    "justification."
                ],
                16
            ),
            vec_string!["This    is    an", "example  of text", "justification.  "]
        );
        assert_eq!(
            Solution::full_justify(
                vec_string!["What", "must", "be", "acknowledgment", "shall", "be"],
                16
            ),
            vec_string!["What   must   be", "acknowledgment  ", "shall be        "]
        );
        assert_eq!(
            Solution::full_justify(
                vec_string![
                    "Science",
                    "is",
                    "what",
                    "we",
                    "understand",
                    "well",
                    "enough",
                    "to",
                    "explain",
                    "to",
                    "a",
                    "computer.",
                    "Art",
                    "is",
                    "everything",
                    "else",
                    "we",
                    "do"
                ],
                20
            ),
            vec_string![
                "Science  is  what we",
                "understand      well",
                "enough to explain to",
                "a  computer.  Art is",
                "everything  else  we",
                "do                  ",
            ]
        );
    }
}

```
