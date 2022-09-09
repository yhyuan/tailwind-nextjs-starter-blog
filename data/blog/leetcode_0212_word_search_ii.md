---
title: Leetcode 0212 word search ii
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0212 word search ii in Rust
---

## Solution in Rust

```rust
/**
 * [212] Word Search II
 *
 * Given an m x n board of characters and a list of strings words, return all words on the board.
 * Each word must be constructed from letters of sequentially adjacent cells, where adjacent cells are horizontally or vertically neighboring. The same letter cell may not be used more than once in a word.
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2020/11/07/search1.jpg" style="width: 322px; height: 322px;" />
 * Input: board = [["o","a","a","n"],["e","t","a","e"],["i","h","k","r"],["i","f","l","v"]], words = ["oath","pea","eat","rain"]
 * Output: ["eat","oath"]
 *
 * Example 2:
 * <img alt="" src="https://assets.leetcode.com/uploads/2020/11/07/search2.jpg" style="width: 162px; height: 162px;" />
 * Input: board = [["a","b"],["c","d"]], words = ["abcb"]
 * Output: []
 *
 *
 * Constraints:
 *
 * 	m == board.length
 * 	n == board[i].length
 * 	1 <= m, n <= 12
 * 	board[i][j] is a lowercase English letter.
 * 	1 <= words.length <= 3 * 10^4
 * 	1 <= words[i].length <= 10
 * 	words[i] consists of lowercase English letters.
 * 	All the strings of words are unique.
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/word-search-ii/
// discuss: https://leetcode.com/problems/word-search-ii/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

use std::collections::HashMap;
#[derive(Default)]
pub struct Trie {
    children: HashMap<char, Trie>,
    end: Option<String>,
}

impl Trie {
/*
    fn new() -> Self {
        Self::default()
    }
*/
    fn insert(&mut self, word: String) {
        let mut curr = self;
        for c in word.chars() {
            curr = curr.children.entry(c).or_default();
        }
        curr.end = Some(word);
    }
    /*
    fn search(&self, word: String) -> bool {
        let mut curr = self;
        for c in word.chars() {
            if let Some(child) = curr.children.get(&c) {
                curr = child;
            } else {
                return false;
            }
        }
        curr.end
    }

    fn starts_with(&self, prefix: String) -> bool {
        let mut curr = self;
        for c in prefix.chars() {
            if let Some(child) = curr.children.get(&c) {
                curr = child;
            } else {
                return false;
            }
        }
        true
    }
    */
}

/*
impl Solution {
    pub fn find_word_helper(board: &Vec<Vec<char>>, chars: &Vec<char>, index: usize, position: (usize, usize), marked: &mut Vec<Vec<bool>>) -> bool {
        let m = board.len() as i32;
        let n = board[0].len() as i32;
        if chars.len() == index {
            return true;
        }
        let options: Vec<(usize, usize)> = [(1, 0), (-1, 0), (0, 1), (0, -1)].into_iter()
            .map(|&delta| (delta.0 + position.0 as i32, delta.1 + position.1 as i32))
            .filter(|&coor| coor.0 >= 0 && coor.0 < m && coor.1 >= 0 && coor.1 < n)
            .map(|coor| (coor.0 as usize, coor.1 as usize))
            .filter(|coor| board[coor.0][coor.1] == chars[index] && !marked[coor.0][coor.1])
            .collect();
        for &option in options.iter() {
            marked[option.0][option.1] = true;
            if Solution::find_word_helper(board, chars, index + 1, option, marked) {
                return true;
            }
            marked[option.0][option.1] = false;
        }
        false
    }
    pub fn find_word(board: &Vec<Vec<char>>, chars: &Vec<char>) -> bool {
        let m = board.len();
        let n = board[0].len();
        let ch = chars[0];
        let mut start_positions: Vec<(usize, usize)> = vec![];
        for i in 0..m {
            for j in 0..n {
                if board[i][j] == ch {
                    start_positions.push((i, j));
                }
            }
        }
        if start_positions.len() == 0 {
            return false;
        }
        for &position in start_positions.iter() {
            let mut marked: Vec<Vec<bool>> = vec![vec![false; n]; m];
            marked[position.0][position.1] = true;
            if Solution::find_word_helper(board, chars, 1, position, &mut marked) {
                return true;
            }
        }
        false
    }
    pub fn find_words(board: Vec<Vec<char>>, words: Vec<String>) -> Vec<String> {
        let mut results: Vec<String> = vec![];
        for word in words {
            let chars: Vec<char> = word.chars().collect();
            if Self::find_word(&board, &chars) {
                results.push(word);
            }
        }
        results
    }
}
*/
impl Solution {
    pub fn find_words(board: Vec<Vec<char>>, words: Vec<String>) -> Vec<String> {
        let mut board = board;
        let mut trie = Trie::default();
        for word in words {
            trie.insert(word);
        }
        let m = board.len();
        let n = board[0].len();
        let mut res: Vec<String> = vec![];
        for i in 0..m {
            for j in 0..n {
                //start from [i][j]
                Self::dfs(i, j, &mut board, &mut res, &mut trie, m, n);
            }
        }
        res
    }
    pub fn dfs(i: usize, j: usize, board: &mut Vec<Vec<char>>, all: &mut Vec<String>, trie: &mut Trie, m: usize, n: usize) {
        let c = board[i][j];
        if let Some(trie) = trie.children.get_mut(&c) {
            board[i][j] = ' ';
            if trie.end.is_some() {
                all.push(trie.end.take().unwrap());
            }
            if i + 1 < m {
                Self::dfs(i + 1, j, board, all, trie, m, n);
            }
            if j + 1 < n {
                Self::dfs(i, j + 1, board, all, trie, m, n);
            }
            if i > 0 {
                Self::dfs(i - 1, j, board, all, trie, m, n);
            }
            if j > 0 {
                Self::dfs(i, j - 1, board, all, trie, m, n);
            }
            board[i][j] = c;
        }
    }
}
// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_212() {
        assert_eq!(Solution::find_words(
            vec![vec!['o','a','a','n'],vec!['e','t','a','e'],vec!['i','h','k','r'],vec!['i','f','l','v']],
            vec!["oath".to_string(),"pea".to_string(),"eat".to_string(),"rain".to_string()]),
            vec!["oath".to_string(), "eat".to_string()]);
        let empty: Vec<String> = vec![];
        assert_eq!(Solution::find_words(
            vec![vec!['a','b'],vec!['c','d']],
            vec!["abcd".to_string()]),
            empty);
    }
}

```
