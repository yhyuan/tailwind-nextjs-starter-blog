---
title: Leetcode 0104 maximum depth of binary tree
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0104 maximum depth of binary tree in Rust
---

## Solution in Rust

```rust
/**
 * [104] Maximum Depth of Binary Tree
 *
 * Given the root of a binary tree, return its maximum depth.
 * A binary tree's maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2020/11/26/tmp-tree.jpg" style="width: 400px; height: 277px;" />
 * Input: root = [3,9,20,null,null,15,7]
 * Output: 3
 *
 * Example 2:
 *
 * Input: root = [1,null,2]
 * Output: 2
 *
 * Example 3:
 *
 * Input: root = []
 * Output: 0
 *
 * Example 4:
 *
 * Input: root = [0]
 * Output: 1
 *
 *
 * Constraints:
 *
 * 	The number of nodes in the tree is in the range [0, 10^4].
 * 	-100 <= Node.val <= 100
 *
 */
pub struct Solution {}
use crate::util::tree::{TreeNode, to_tree};

// problem: https://leetcode.com/problems/maximum-depth-of-binary-tree/
// discuss: https://leetcode.com/problems/maximum-depth-of-binary-tree/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

use std::borrow::Borrow;
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
    fn inorder(&self, visit: &mut dyn FnMut(usize), depth: usize);
}
impl Inorder for TreeLink {
    fn inorder(&self, visit: &mut dyn FnMut(usize), depth: usize) {
        if let Some(node) = self {
            //let x = self.unwrap().borrow().left;
            // let x = RefCell::borrow(node);
            let node: Ref<TreeNode> = RefCell::borrow(node); //node.borrow();
            node.left.inorder(visit, depth + 1);
            visit(depth);
            node.right.inorder(visit, depth + 1);
        }
    }
}
impl Solution {
    pub fn max_depth(root: Option<Rc<RefCell<TreeNode>>>) -> i32 {
        let mut res = 0usize;
        root.inorder(&mut |depth| {
            res = usize::max(res, depth);
        }, 1);
        res as i32
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_104() {
        assert_eq!(Solution::max_depth(tree![]), 0);
        assert_eq!(Solution::max_depth(tree![3, 9, 20, null, null, 15, 7]), 3);
    }
}

```
