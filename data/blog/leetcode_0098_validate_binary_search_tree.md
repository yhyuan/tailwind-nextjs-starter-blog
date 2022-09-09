---
title: Leetcode 0098 validate binary search tree
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0098 validate binary search tree in Rust
---

## Solution in Rust

```rust
/**
 * [98] Validate Binary Search Tree
 *
 * Given the root of a binary tree, determine if it is a valid binary search tree (BST).
 * A valid BST is defined as follows:
 *
 * 	The left subtree of a node contains only nodes with keys less than the node's key.
 * 	The right subtree of a node contains only nodes with keys greater than the node's key.
 * 	Both the left and right subtrees must also be binary search trees.
 *
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2020/12/01/tree1.jpg" style="width: 302px; height: 182px;" />
 * Input: root = [2,1,3]
 * Output: true
 *
 * Example 2:
 * <img alt="" src="https://assets.leetcode.com/uploads/2020/12/01/tree2.jpg" style="width: 422px; height: 292px;" />
 * Input: root = [5,1,4,null,null,3,6]
 * Output: false
 * Explanation: The root node's value is 5 but its right child's value is 4.
 *
 *
 * Constraints:
 *
 * 	The number of nodes in the tree is in the range [1, 10^4].
 * 	-2^31 <= Node.val <= 2^31 - 1
 *
 */
pub struct Solution {}
use crate::util::tree::{TreeNode, to_tree};

// problem: https://leetcode.com/problems/validate-binary-search-tree/
// discuss: https://leetcode.com/problems/validate-binary-search-tree/discuss/?currentPage=1&orderBy=most_votes&query=

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
use std::cell::{RefCell, Ref};
type TreeLink = Option<Rc<RefCell<TreeNode>>>;
trait Inorder {
    fn inorder(&self, visit: &mut dyn FnMut(i32));
}
impl Inorder for TreeLink {
    fn inorder(&self, visit: &mut dyn FnMut(i32)) {
        if let Some(node) = self {
            let node: Ref<TreeNode> = node.borrow();
            //Self::inorder(&node.left, visit);
            node.left.inorder(visit);
            visit(node.val);
            //Self::inorder(&node.right, visit);
            node.right.inorder(visit);
        }
    }
}

impl Solution {
    pub fn is_valid_bst(root: TreeLink) -> bool {
        let mut prev: Option<i32> = None;
        let mut res = true;
        root.inorder(&mut |x| {
            if let Some(y) = prev {
                if x <= y {
                    res = false;
                }
            }
            prev = Some(x);
        });
        res
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_98() {
        assert_eq!(
            Solution::is_valid_bst(tree![5, 1, 4, null, null, 3, 6]),
            false
        );
        assert_eq!(Solution::is_valid_bst(tree![2, 1, 3]), true);
        assert_eq!(
            Solution::is_valid_bst(tree![10, 5, 15, null, null, 6, 20]),
            false
        );
    }
}

```
