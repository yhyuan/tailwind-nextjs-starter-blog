---
title: Leetcode 0990 satisfiability of equality equations
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0990 satisfiability of equality equations in Rust
---

## Solution in Rust

```rust
/**
 * [990] Satisfiability of Equality Equations
 *
 * You are given an array of strings equations that represent relationships between variables where each string equations[i] is of length 4 and takes one of two different forms: "xi==yi" or "xi!=yi".Here, xi and yi are lowercase letters (not necessarily different) that represent one-letter variable names.
 * Return true if it is possible to assign integers to variable names so as to satisfy all the given equations, or false otherwise.
 *
 * Example 1:
 *
 * Input: equations = ["a==b","b!=a"]
 * Output: false
 * Explanation: If we assign say, a = 1 and b = 1, then the first equation is satisfied, but not the second.
 * There is no way to assign the variables to satisfy both equations.
 *
 * Example 2:
 *
 * Input: equations = ["b==a","a==b"]
 * Output: true
 * Explanation: We could assign a = 1 and b = 1 to satisfy both equations.
 *
 * Example 3:
 *
 * Input: equations = ["a==b","b==c","a==c"]
 * Output: true
 *
 * Example 4:
 *
 * Input: equations = ["a==b","b!=c","c==a"]
 * Output: false
 *
 * Example 5:
 *
 * Input: equations = ["c==c","b==d","x!=z"]
 * Output: true
 *
 *
 * Constraints:
 *
 * 	1 <= equations.length <= 500
 * 	equations[i].length == 4
 * 	equations[i][0] is a lowercase letter.
 * 	equations[i][1] is either '=' or '!'.
 * 	equations[i][2] is '='.
 * 	equations[i][3] is a lowercase letter.
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/satisfiability-of-equality-equations/
// discuss: https://leetcode.com/problems/satisfiability-of-equality-equations/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
pub struct DisjointSet {
    parents: Vec<usize>,
    ranks: Vec<usize>,
}

impl DisjointSet {
    pub fn new(n: usize) -> Self {
        let ranks = vec![1; n];
        let parents = (0..n).into_iter().collect::<Vec<_>>();
        DisjointSet {
            ranks,
            parents,
        }
    }

    pub fn find(&self, i: usize) -> usize {
        let mut parent = self.parents[i];
        while parent != self.parents[parent] {
            parent = self.parents[parent];
        }
        parent
    }

    pub fn union(&mut self, i: usize, j: usize) -> bool {
        let ip = self.find(i);
        let jp = self.find(j);
        if ip == jp {
            return false;
        }
        if self.ranks[ip] < self.ranks[jp] {
            self.parents[ip] = self.parents[jp];
        } else if self.parents[ip] > self.parents[jp] {
            self.parents[jp] = self.parents[ip];
        } else {
            self.parents[jp] = self.parents[ip];
            self.ranks[ip] += 1;
        }
        true
    }
}
use std::collections::{HashSet, HashMap};

impl Solution {
    pub fn equations_possible(equations: Vec<String>) -> bool {
        let mut hashset: HashSet<char> = HashSet::new();
        for i in 0..equations.len() {
            let char1 = equations[i].chars().nth(0).unwrap();
            let char2 = equations[i].chars().nth(3).unwrap();
            hashset.insert(char1);
            hashset.insert(char2);
        }
        let chars = hashset.into_iter().collect::<Vec<char>>();
        let mut hashmap: HashMap<char, usize> = HashMap::new();
        for i in 0..chars.len() {
            hashmap.insert(chars[i], i);
        }
        let mut disjoint_set = DisjointSet::new(chars.len());
        for i in 0..equations.len() {
            let char1 = equations[i].chars().nth(1).unwrap();
            let char2 = equations[i].chars().nth(2).unwrap();
            if char1 == '=' && char2 == '=' {
                let char0 = equations[i].chars().nth(0).unwrap();
                let char3 = equations[i].chars().nth(3).unwrap();
                let k1 = hashmap.get(&char0).unwrap();
                let k2 = hashmap.get(&char3).unwrap();
                disjoint_set.union(*k1, *k2);
            }
        }
        for i in 0..equations.len() {
            let char1 = equations[i].chars().nth(1).unwrap();
            let char2 = equations[i].chars().nth(2).unwrap();
            if char1 == '!' && char2 == '=' {
                let char0 = equations[i].chars().nth(0).unwrap();
                let char3 = equations[i].chars().nth(3).unwrap();
                let k1 = hashmap.get(&char0).unwrap();
                let k2 = hashmap.get(&char3).unwrap();
                if disjoint_set.find(*k1) == disjoint_set.find(*k2) {
                    return false;
                }
            }
        }
        true
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_990() {
        assert_eq!(Solution::equations_possible(vec_string!["a==b","b!=a"]), false);
        assert_eq!(Solution::equations_possible(vec_string!["b==a","a==b"]), true);
    }
}

```
