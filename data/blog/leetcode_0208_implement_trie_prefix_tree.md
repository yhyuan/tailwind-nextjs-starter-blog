---
title: Leetcode 0208 implement trie prefix tree
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0208 implement trie prefix tree in Rust
---

## Solution in Rust

```rust
/**
 * [208] Implement Trie (Prefix Tree)
 *
 * A <a href="https://en.wikipedia.org/wiki/Trie" target="_blank">trie</a> (pronounced as "try") or prefix tree is a tree data structure used to efficiently store and retrieve keys in a dataset of strings. There are various applications of this data structure, such as autocomplete and spellchecker.
 * Implement the Trie class:
 *
 * 	Trie() Initializes the trie object.
 * 	void insert(String word) Inserts the string word into the trie.
 * 	boolean search(String word) Returns true if the string word is in the trie (i.e., was inserted before), and false otherwise.
 * 	boolean startsWith(String prefix) Returns true if there is a previously inserted string word that has the prefix prefix, and false otherwise.
 *
 *
 * Example 1:
 *
 * Input
 * ["Trie", "insert", "search", "search", "startsWith", "insert", "search"]
 * [[], ["apple"], ["apple"], ["app"], ["app"], ["app"], ["app"]]
 * Output
 * [null, null, true, false, true, null, true]
 * Explanation
 * Trie trie = new Trie();
 * trie.insert("apple");
 * trie.search("apple");   // return True
 * trie.search("app");     // return False
 * trie.startsWith("app"); // return True
 * trie.insert("app");
 * trie.search("app");     // return True
 *
 *
 * Constraints:
 *
 * 	1 <= word.length, prefix.length <= 2000
 * 	word and prefix consist only of lowercase English letters.
 * 	At most 3 * 10^4 calls in total will be made to insert, search, and startsWith.
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/implement-trie-prefix-tree/
// discuss: https://leetcode.com/problems/implement-trie-prefix-tree/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
/*
#[derive(Default)]
struct Trie {
    is_end: bool,
    nodes: [Option<Box<Trie>>; 26],
}

impl Trie {

    fn new() -> Self {
        Default::default()
    }

    fn insert(&mut self, word: String) {
        let mut curr = self;
        for i in word.chars().map(|ch| (ch as u8 - 'a' as u8) as usize) {
            curr = curr.nodes[i].get_or_insert_with(|| Box::new(Trie::new())); // Option get_or_insert_with
        }
        curr.is_end = true;
    }

    fn search(&self, word: String) -> bool {
        self.find(word).map_or(false, |t| t.is_end)
    }

    fn starts_with(&self, prefix: String) -> bool {
        self.find(prefix).is_some()
    }

    fn find(&self, word: String) -> Option<&Trie> {
        let mut curr = self;
        for i in word.chars().map(|ch| (ch as u8 - 'a' as u8) as usize) {
            curr = curr.nodes[i].as_ref()?;
        }
        Some(curr)
    }
}
*/
use std::collections::HashMap;
#[derive(PartialEq, Eq, Clone, Debug, Default)]
struct Trie {
    children: HashMap<char, Trie>,
    end: bool,
}

impl Trie {

    fn new() -> Self {
        Self::default()
    }

    fn insert(&mut self, word: String) {
        let mut curr = self;
        for c in word.chars() {
            curr = curr.children.entry(c).or_default();
        }
        curr.end = true;
    }

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
}
/**
 * Your Trie object will be instantiated and called as such:
 * let obj = Trie::new();
 * obj.insert(word);
 * let ret_2: bool = obj.search(word);
 * let ret_3: bool = obj.starts_with(prefix);
 */

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_208() {
        let mut trie = Trie::new();
        trie.insert("apple".to_owned());
        assert_eq!(trie.search("apple".to_owned()), true); // returns true
        assert_eq!(trie.search("app".to_owned()), false);
        assert_eq!(trie.starts_with("app".to_owned()), true); // returns true
        trie.insert("app".to_owned());
        assert_eq!(trie.search("app".to_owned()), true); // returns true
    }
}

```
