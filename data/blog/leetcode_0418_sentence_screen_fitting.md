---
title: Leetcode 0418 sentence screen fitting
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0418 sentence screen fitting in Rust
---

## Solution in Rust

```rust
/**
418. Sentence Screen Fitting
Medium

941

476

Add to List

Share
Given a rows x cols screen and a sentence represented as a list of strings, return the number of times the given sentence can be fitted on the screen.

The order of words in the sentence must remain unchanged, and a word cannot be split into two lines. A single space must separate two consecutive words in a line.



Example 1:

Input: sentence = ["hello","world"], rows = 2, cols = 8
Output: 1
Explanation:
hello---
world---
The character '-' signifies an empty space on the screen.
Example 2:

Input: sentence = ["a", "bcd", "e"], rows = 3, cols = 6
Output: 2
Explanation:
a-bcd-
e-a---
bcd-e-
The character '-' signifies an empty space on the screen.
Example 3:

Input: sentence = ["i","had","apple","pie"], rows = 4, cols = 5
Output: 1
Explanation:
i-had
apple
pie-i
had--
The character '-' signifies an empty space on the screen.


Constraints:

1 <= sentence.length <= 100
1 <= sentence[i].length <= 10
sentence[i] consists of lowercase English letters.
1 <= rows, cols <= 2 * 104
Accepted
85,596
Submissions
241,053
 */
 pub struct Solution {}

 // problem: https://leetcode.com/problems/sentence-screen-fitting/
 // discuss: https://leetcode.com/problems/sentence-screen-fitting/discuss/?currentPage=1&orderBy=most_votes&query=

 // submission codes start here
 impl Solution {
    pub fn words_typing(sentence: Vec<String>, rows: i32, cols: i32) -> i32 {
        let word_lens: Vec<usize> = sentence.iter().map(|s| s.len()).collect::<Vec<_>>();
        if word_lens.iter().filter(|&size| size > &(cols as usize)).count() > 0 { // exist a word which is longer than cols. It is not possible to fit.
            return 0;
        }
        let sentence_len = word_lens.iter().sum::<usize>() + word_lens.len() - 1;
        let rows = rows as usize;
        let cols = cols as usize;
        //special case: the length + one space can fill each row.
        if cols % (sentence_len + 1) == 0 {
            let result = rows  * cols / (sentence_len + 1);
            return result as i32;
        }
        //special case: the length + one space can fill each row.
        if cols % (sentence_len + 1) == sentence_len {
            let result = rows * (cols + 1) / (sentence_len + 1);
            return result as i32;
        }
        let mut ans = 0i32;
        let mut index = 0usize;
        for i in 0..rows {
            // if cols is very long, the following lines allow us to quick jump.
            let col_index = index % cols;
            let n = (cols - col_index) / (sentence_len + 1); // with a space ending.
            ans += n as i32;
            index = index + n * (sentence_len + 1);
            // deal with remaining spaces or the column is short.
            for j in 0..word_lens.len() {
                let remained = cols - index % cols;
                // remained space is long
                if remained > word_lens[j] {
                    index += word_lens[j];
                    index += 1;
                } else if remained == word_lens[j] { // remained space just fill the word
                    index += word_lens[j];
                } else { // remaining space is too short and we have to jump to the enxt row
                    index = (index / cols + 1) * cols + word_lens[j];
                    index += 1;
                }
            }
            if index > rows * cols {
                break;
            }
            ans += 1;
            if index == rows * cols {
                break;
            }
        }
        return ans;
    }
}

 // submission codes end

 #[cfg(test)]
 mod tests {
     use super::*;

     #[test]
     fn test_418() {
        assert_eq!(Solution::words_typing(vec_string!["a", "bc"], 20000, 20000), 80000000);
        assert_eq!(Solution::words_typing(vec_string!["a", "bcd", "e"], 3, 6), 2);
        assert_eq!(Solution::words_typing(vec_string!["hello","world"], 16, 8), 8);
        assert_eq!(Solution::words_typing(vec_string!["a", "b", "c"], 3, 1), 1);
        assert_eq!(Solution::words_typing(vec_string!["a", "bcd", "e"], 3, 6), 2);
        assert_eq!(Solution::words_typing(vec_string!["hello","world"], 2, 8), 1);
        assert_eq!(Solution::words_typing(vec_string!["i","had","apple","pie"], 4, 5), 1);
    }
 }

```
