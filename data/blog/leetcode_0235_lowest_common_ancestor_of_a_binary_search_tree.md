---
title: Leetcode 0235 lowest common ancestor of a binary search tree
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0235 lowest common ancestor of a binary search tree in Rust
---

## Solution in Rust

```rust
/**
 * [235] Lowest Common Ancestor of a Binary Search Tree
 *
 * Given a binary search tree (BST), find the lowest common ancestor (LCA) of two given nodes in the BST.
 * According to the <a href="https://en.wikipedia.org/wiki/Lowest_common_ancestor" target="_blank">definition of LCA on Wikipedia</a>: &ldquo;The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants (where we allow a node to be a descendant of itself).&rdquo;
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2018/12/14/binarysearchtree_improved.png" style="width: 200px; height: 190px;" />
 * Input: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
 * Output: 6
 * Explanation: The LCA of nodes 2 and 8 is 6.
 *
 * Example 2:
 * <img alt="" src="https://assets.leetcode.com/uploads/2018/12/14/binarysearchtree_improved.png" style="width: 200px; height: 190px;" />
 * Input: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4
 * Output: 2
 * Explanation: The LCA of nodes 2 and 4 is 2, since a node can be a descendant of itself according to the LCA definition.
 *
 * Example 3:
 *
 * Input: root = [2,1], p = 2, q = 1
 * Output: 2
 *
 *
 * Constraints:
 *
 * 	The number of nodes in the tree is in the range [2, 10^5].
 * 	-10^9 <= Node.val <= 10^9
 * 	All Node.val are unique.
 * 	p != q
 * 	p and q will exist in the BST.
 *
 */
pub struct Solution {}
use crate::util::tree::{TreeNode, to_tree};

// problem: https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/
// discuss: https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/discuss/?currentPage=1&orderBy=most_votes&query=

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
/*
use std::rc::Rc;
use std::cell::{RefCell, Ref};
type TreeLink = Option<Rc<RefCell<TreeNode>>>;
trait Postorder {
    fn postorder(&self, visit: &mut dyn FnMut(i32, &Vec<i32>), path: &mut Vec<i32>);
}
impl Postorder for TreeLink {
    fn postorder(&self, visit: &mut dyn FnMut(i32, &Vec<i32>), path: &mut Vec<i32>) {
        if let Some(node) = self {
            let node: Ref<TreeNode> = node.borrow();
            path.push(node.val);
            visit(node.val, path);
            node.left.postorder(visit, path);
            node.right.postorder(visit, path);
            path.pop();
        }
    }
}
impl Solution {
    pub fn lowest_common_ancestor(root: Option<Rc<RefCell<TreeNode>>>, p: Option<Rc<RefCell<TreeNode>>>, q: Option<Rc<RefCell<TreeNode>>>) -> Option<Rc<RefCell<TreeNode>>> {
        let mut path: Vec<i32> = vec![];
        let mut p_path: Vec<i32> = vec![];
        let mut q_path: Vec<i32> = vec![];
        let p_val = p.unwrap().borrow().val;
        let q_val = q.unwrap().borrow().val;

        root.postorder(&mut |x, path| {
            if x == p_val {
                p_path = path.clone();
            }
            if x == q_val {
                q_path = path.clone();
            }
        }, &mut path);
        let mut i = 0;
        while i < p_path.len() && i < q_path.len() && p_path[i] == q_path[i] {
            i += 1;
        }
        Some(Rc::new(RefCell::new(TreeNode::new(p_path[i - 1]))))
    }
}
*/
use std::rc::Rc;
use std::cell::RefCell;
impl Solution {
    pub fn lowest_common_ancestor(root: Option<Rc<RefCell<TreeNode>>>, p: Option<Rc<RefCell<TreeNode>>>, q: Option<Rc<RefCell<TreeNode>>>) -> Option<Rc<RefCell<TreeNode>>> {
        let mut root = root;
        let p_val = p.unwrap().borrow().val;
        let q_val = q.unwrap().borrow().val;
        while let Some(node) = root.clone() {
            let mut _node = node.borrow_mut();
            let val = _node.val;
            if val > p_val && val > q_val {
                root = _node.left.take();
                continue;
            }
            if val < p_val && val < q_val {
                root = _node.right.take();
                continue;
            }
            _node.left.take();
            _node.right.take();
            break;
        }
        root
    }
}
// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_235() {
        assert_eq!(
            Solution::lowest_common_ancestor(tree![6,2,8,0,4,7,9,null,null,3,5],
                Some(Rc::new(RefCell::new(TreeNode::new(2)))),
                Some(Rc::new(RefCell::new(TreeNode::new(8))))),
                Some(Rc::new(RefCell::new(TreeNode::new(6)))));
        assert_eq!(
            Solution::lowest_common_ancestor(tree![6,2,8,0,4,7,9,null,null,3,5],
                Some(Rc::new(RefCell::new(TreeNode::new(2)))),
                Some(Rc::new(RefCell::new(TreeNode::new(4))))),
                Some(Rc::new(RefCell::new(TreeNode::new(2)))));
    }
}

```
