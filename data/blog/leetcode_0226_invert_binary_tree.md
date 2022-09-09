---
title: Leetcode 0226 invert binary tree
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0226 invert binary tree in Rust
---

## Solution in Rust

```rust
/**
 * [226] Invert Binary Tree
 *
 * Given the root of a binary tree, invert the tree, and return its root.
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2021/03/14/invert1-tree.jpg" style="width: 500px; height: 165px;" />
 * Input: root = [4,2,7,1,3,6,9]
 * Output: [4,7,2,9,6,3,1]
 *
 * Example 2:
 * <img alt="" src="https://assets.leetcode.com/uploads/2021/03/14/invert2-tree.jpg" style="width: 500px; height: 120px;" />
 * Input: root = [2,1,3]
 * Output: [2,3,1]
 *
 * Example 3:
 *
 * Input: root = []
 * Output: []
 *
 *
 * Constraints:
 *
 * 	The number of nodes in the tree is in the range [0, 100].
 * 	-100 <= Node.val <= 100
 *
 */
pub struct Solution {}
use crate::util::tree::{TreeNode, to_tree};

// problem: https://leetcode.com/problems/invert-binary-tree/
// discuss: https://leetcode.com/problems/invert-binary-tree/discuss/?currentPage=1&orderBy=most_votes&query=

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
type MaybeNode = Option<Rc<RefCell<TreeNode>>>;
impl Solution {
    pub fn invert_tree(root: Option<Rc<RefCell<TreeNode>>>) -> Option<Rc<RefCell<TreeNode>>> {
        fn helper(node: &mut MaybeNode) {
            if let Some(_node) = node {
                let mut _node = _node.borrow_mut();

                match (_node.left.take(), _node.right.take()) {
                    (None, None) => {}
                    (l, r) => {
                        _node.left = r;
                        _node.right = l;
                    }
                }
                helper(&mut _node.left);
                helper(&mut _node.right);
            }
        }
        let mut root = root;
        helper(&mut root);
        root
        //Some(Rc::new(RefCell::new(TreeNode::new(0))))
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_226() {
        assert_eq!(
            Solution::invert_tree(tree![4, 2, 7, 1, 3, 6, 9]),
            tree![4, 7, 2, 9, 6, 3, 1]
        );
    }
}

```
