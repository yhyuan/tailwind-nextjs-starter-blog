---
title: Leetcode 0114 flatten binary tree to linked list
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0114 flatten binary tree to linked list in Rust
---

## Solution in Rust

```rust
/**
 * [114] Flatten Binary Tree to Linked List
 *
 * Given the root of a binary tree, flatten the tree into a "linked list":
 *
 * 	The "linked list" should use the same TreeNode class where the right child pointer points to the next node in the list and the left child pointer is always null.
 * 	The "linked list" should be in the same order as a <a href="https://en.wikipedia.org/wiki/Tree_traversal#Pre-order,_NLR" target="_blank">pre-order traversal</a> of the binary tree.
 *
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2021/01/14/flaten.jpg" style="width: 500px; height: 226px;" />
 * Input: root = [1,2,5,3,4,null,6]
 * Output: [1,null,2,null,3,null,4,null,5,null,6]
 *
 * Example 2:
 *
 * Input: root = []
 * Output: []
 *
 * Example 3:
 *
 * Input: root = [0]
 * Output: [0]
 *
 *
 * Constraints:
 *
 * 	The number of nodes in the tree is in the range [0, 2000].
 * 	-100 <= Node.val <= 100
 *
 *
 * Follow up: Can you flatten the tree in-place (with O(1) extra space)?
 */
pub struct Solution {}
use crate::util::tree::{TreeNode, to_tree};

// problem: https://leetcode.com/problems/flatten-binary-tree-to-linked-list/
// discuss: https://leetcode.com/problems/flatten-binary-tree-to-linked-list/discuss/?currentPage=1&orderBy=most_votes&query=

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
use std::cell::{RefCell, Ref, RefMut};
type TreeLink = Option<Rc<RefCell<TreeNode>>>;
trait Preorder {
    fn preorder(&self, visit: &mut dyn FnMut(i32));
}
impl Preorder for TreeLink {
    fn preorder(&self, visit: &mut dyn FnMut(i32)) {
        if let Some(node) = self {
            let node: Ref<TreeNode> = node.borrow();
            visit(node.val);
            let left_depth = node.left.preorder(visit);
            let right_depth = node.right.preorder(visit);
        }
    }
}
impl Solution {
    pub fn flatten(root: &mut Option<Rc<RefCell<TreeNode>>>) {
        let mut nums: Vec<i32> = vec![];
        root.preorder(&mut |x| {
            nums.push(x);
        });
        if nums.len() == 0 {
            return;
        }
        let mut right: Option<Rc<RefCell<TreeNode>>> = None;
        for i in (1..nums.len()).rev() {
            if right.is_some() {
                right = Some(Rc::new(RefCell::new(TreeNode{val: nums[i], left: None, right: right})));
            } else {
                right = Some(Rc::new(RefCell::new(TreeNode::new(nums[i]))));
            }
        }
        if let Some(node) = root {
            let mut node: RefMut<TreeNode> = node.borrow_mut();
            node.left = None;
            node.right = right;
        }
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_114() {
        let mut tree = tree![1, 2, 5, 3, 4, null, 6];
        Solution::flatten(&mut tree);
        assert_eq!(tree, tree![1, null, 2, null, 3, null, 4, null, 5, null, 6]);

        let mut tree = tree![1, 2, null, 3];
        Solution::flatten(&mut tree);
        assert_eq!(tree, tree![1, null, 2, null, 3]);

        let mut tree = tree![1, null, 2, 3];
        Solution::flatten(&mut tree);
        assert_eq!(tree, tree![1, null, 2, null, 3]);

    }
}

```
