---
title: Leetcode 0094 binary tree inorder traversal
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0094 binary tree inorder traversal in Rust
---

## Solution in Rust

```rust
/**
 * [94] Binary Tree Inorder Traversal
 *
 * Given the root of a binary tree, return the inorder traversal of its nodes' values.
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2020/09/15/inorder_1.jpg" style="width: 202px; height: 324px;" />
 * Input: root = [1,null,2,3]
 * Output: [1,3,2]
 *
 * Example 2:
 *
 * Input: root = []
 * Output: []
 *
 * Example 3:
 *
 * Input: root = [1]
 * Output: [1]
 *
 * Example 4:
 * <img alt="" src="https://assets.leetcode.com/uploads/2020/09/15/inorder_5.jpg" style="width: 202px; height: 202px;" />
 * Input: root = [1,2]
 * Output: [2,1]
 *
 * Example 5:
 * <img alt="" src="https://assets.leetcode.com/uploads/2020/09/15/inorder_4.jpg" style="width: 202px; height: 202px;" />
 * Input: root = [1,null,2]
 * Output: [1,2]
 *
 *
 * Constraints:
 *
 * 	The number of nodes in the tree is in the range [0, 100].
 * 	-100 <= Node.val <= 100
 *
 *
 * Follow up: Recursive solution is trivial, could you do it iteratively?
 */
pub struct Solution {}
use crate::util::tree::{TreeNode, to_tree};

// problem: https://leetcode.com/problems/binary-tree-inorder-traversal/
// discuss: https://leetcode.com/problems/binary-tree-inorder-traversal/discuss/?currentPage=1&orderBy=most_votes&query=

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
    pub fn inorder_traversal(root: Option<Rc<RefCell<TreeNode>>>) -> Vec<i32> {
        let mut res = Vec::new();
        root.inorder(&mut |x| {
            res.push(x);
        });
        res
    }
}
/*
impl Solution {
    fn inorder_traversal_helper<F: FnMut(i32)>(root: Option<&Rc<RefCell<TreeNode>>>, consumer: &mut F) {
        if let Some(node) = root {
            Solution::inorder_traversal_helper(node.borrow().left.as_ref(), consumer);
            //consumer(root.as_ref().unwrap().borrow().val);
            consumer(node.borrow().val);
            Solution::inorder_traversal_helper(node.borrow().right.as_ref(), consumer);
        }
    }
    pub fn inorder_traversal(root: Option<Rc<RefCell<TreeNode>>>) -> Vec<i32> {
        let mut res = Vec::new();
        Solution::inorder_traversal_helper(root.as_ref(), &mut (|v| res.push(v)));
        res
    }
}
*/
// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_94() {
        assert_eq!(
            Solution::inorder_traversal(tree![1, null, 2, 3]),
            vec![1, 3, 2]
        );
        assert_eq!(
            Solution::inorder_traversal(tree![1, 2, 3, 4, 5, 6, 7]),
            vec![4, 2, 5, 1, 6, 3, 7]
        );
    }
}

```
