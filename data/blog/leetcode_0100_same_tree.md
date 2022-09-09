---
title: Leetcode 0100 same tree
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0100 same tree in Rust
---

## Solution in Rust

```rust
/**
 * [100] Same Tree
 *
 * Given the roots of two binary trees p and q, write a function to check if they are the same or not.
 * Two binary trees are considered the same if they are structurally identical, and the nodes have the same value.
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2020/12/20/ex1.jpg" style="width: 622px; height: 182px;" />
 * Input: p = [1,2,3], q = [1,2,3]
 * Output: true
 *
 * Example 2:
 * <img alt="" src="https://assets.leetcode.com/uploads/2020/12/20/ex2.jpg" style="width: 382px; height: 182px;" />
 * Input: p = [1,2], q = [1,null,2]
 * Output: false
 *
 * Example 3:
 * <img alt="" src="https://assets.leetcode.com/uploads/2020/12/20/ex3.jpg" style="width: 622px; height: 182px;" />
 * Input: p = [1,2,1], q = [1,1,2]
 * Output: false
 *
 *
 * Constraints:
 *
 * 	The number of nodes in both trees is in the range [0, 100].
 * 	-10^4 <= Node.val <= 10^4
 *
 */
pub struct Solution {}
use crate::util::tree::{TreeNode, to_tree};

// problem: https://leetcode.com/problems/same-tree/
// discuss: https://leetcode.com/problems/same-tree/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

// Definition for a binary tree node.
// #[derive(Debug, PartialEq, Eq)]
// pub struct TreeNode {
//   pub val: i32,
//   pub left: Option<Rc<RefCell<TreeNode>>>,
//   pub right: Option<Rc<RefCell<TreeNode>>>,
// }
//
// impl TreeNode {
//   #[inline]
//   pub fn new(val: i32) -> Self {
//     TreeNode {
//       val,
//       left: None,
//       right: None
//     }
//   }
// }
use std::rc::Rc;
use std::cell::RefCell;
impl Solution {
    /*
    pub fn is_same_tree(p: Option<Rc<RefCell<TreeNode>>>, q: Option<Rc<RefCell<TreeNode>>>) -> bool {
        let mut stack: Vec<(Option<Rc<RefCell<TreeNode>>>, Option<Rc<RefCell<TreeNode>>>)> = vec![];
        stack.push((p, q));
        while !stack.is_empty() {
            let pair:(Option<Rc<RefCell<TreeNode>>>, Option<Rc<RefCell<TreeNode>>>) = stack.pop().unwrap();
            match pair {
                (Some(p), Some(q)) if p == q => {
                    let p = p.borrow();
                    let q = q.borrow();
                    stack.push((p.left.clone(), q.left.clone()));
                    stack.push((p.right.clone(), q.right.clone()));
                }
                (None, None) => {}
                _ => {
                    return false;
                }
            }
        }
        true
    }
    */
    pub fn is_same_tree(p: Option<Rc<RefCell<TreeNode>>>, q: Option<Rc<RefCell<TreeNode>>>) -> bool {
        fn helper(p: &Option<Rc<RefCell<TreeNode>>>, q: &Option<Rc<RefCell<TreeNode>>>) -> bool {
            match (p, q) {
                (Some(p), Some(q)) if p == q => {
                    let p = p.borrow();
                    let q = q.borrow();

                    helper(&p.left, &q.left) && helper(&p.right, &q.right)
                },
                (None, None) => true,
                _ => false
            }
        }

        helper(&p, &q)
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_100() {
        assert_eq!(
            Solution::is_same_tree(tree![1, 2, 3, 4, null, 5], tree![1, 2, 3, 4, null, 5]),
            true
        )
    }
}

```
