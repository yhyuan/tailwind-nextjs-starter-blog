---
title: Leetcode 0111 minimum depth of binary tree
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0111 minimum depth of binary tree in Rust
---

## Solution in Rust

```rust
/**
 * [111] Minimum Depth of Binary Tree
 *
 * Given a binary tree, find its minimum depth.
 * The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.
 * Note: A leaf is a node with no children.
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2020/10/12/ex_depth.jpg" style="width: 432px; height: 302px;" />
 * Input: root = [3,9,20,null,null,15,7]
 * Output: 2
 *
 * Example 2:
 *
 * Input: root = [2,null,3,null,4,null,5,null,6]
 * Output: 5
 *
 *
 * Constraints:
 *
 * 	The number of nodes in the tree is in the range [0, 10^5].
 * 	-1000 <= Node.val <= 1000
 *
 */
pub struct Solution {}
use crate::util::tree::{TreeNode, to_tree};

// problem: https://leetcode.com/problems/minimum-depth-of-binary-tree/
// discuss: https://leetcode.com/problems/minimum-depth-of-binary-tree/discuss/?currentPage=1&orderBy=most_votes&query=

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
trait Preorder {
    fn preorder(&self, visit: &mut dyn FnMut(i32, usize), layer: usize);
}
impl Preorder for TreeLink {
    fn preorder(&self, visit: &mut dyn FnMut(i32, usize), layer: usize) {
        if let Some(node) = self {
            let node: Ref<TreeNode> = node.borrow();
            if node.left.is_none() && node.right.is_none() {
                visit(node.val, layer);
                return;
            }
            let left_depth = node.left.preorder(visit, layer + 1);
            let right_depth = node.right.preorder(visit, layer + 1);
        }
    }
}
impl Solution {
    pub fn min_depth(root: Option<Rc<RefCell<TreeNode>>>) -> i32 {
        let mut res: Option<usize> = None;
        root.preorder(&mut |x, layer| {
            if let Some(min_depth) = res {
                res = Some(usize::min(min_depth, layer));
            } else {
                res = Some(layer);
            }
        }, 1);
        if res.is_none() {0} else {res.unwrap() as i32}
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_111() {
        assert_eq!(Solution::min_depth(tree![3, 9, 20, null, null, 15, 7]), 2);
        assert_eq!(Solution::min_depth(tree![2,null,3,null,4,null,5,null,6]), 5);
        assert_eq!(Solution::min_depth(tree![]), 0);
    }
}

```
