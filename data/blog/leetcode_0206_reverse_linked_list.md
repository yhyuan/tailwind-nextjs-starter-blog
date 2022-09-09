---
title: Leetcode 0206 reverse linked list
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0206 reverse linked list in Rust
---

## Solution in Rust

```rust
/**
 * [206] Reverse Linked List
 *
 * Given the head of a singly linked list, reverse the list, and return the reversed list.
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2021/02/19/rev1ex1.jpg" style="width: 542px; height: 222px;" />
 * Input: head = [1,2,3,4,5]
 * Output: [5,4,3,2,1]
 *
 * Example 2:
 * <img alt="" src="https://assets.leetcode.com/uploads/2021/02/19/rev1ex2.jpg" style="width: 182px; height: 222px;" />
 * Input: head = [1,2]
 * Output: [2,1]
 *
 * Example 3:
 *
 * Input: head = []
 * Output: []
 *
 *
 * Constraints:
 *
 * 	The number of nodes in the list is the range [0, 5000].
 * 	-5000 <= Node.val <= 5000
 *
 *
 * Follow up: A linked list can be reversed either iteratively or recursively. Could you implement both?
 *
 */
pub struct Solution {}
use crate::util::linked_list::{ListNode, to_list};

// problem: https://leetcode.com/problems/reverse-linked-list/
// discuss: https://leetcode.com/problems/reverse-linked-list/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

// Definition for singly-linked list.
// #[derive(PartialEq, Eq, Clone, Debug)]
// pub struct ListNode {
//   pub val: i32,
//   pub next: Option<Box<ListNode>>
// }
//
// impl ListNode {
//   #[inline]
//   fn new(val: i32) -> Self {
//     ListNode {
//       next: None,
//       val
//     }
//   }
// }
impl Solution {
    pub fn reverse_list(head: Option<Box<ListNode>>) -> Option<Box<ListNode>> {
        let mut p = head.as_ref();
        let mut values: Vec<i32> = vec![];
        while p.is_some() {
            let value = p.unwrap().val;
            values.push(value);
            p = p.unwrap().next.as_ref();
        }
        values.reverse();
        let mut dummy_head: Option<Box<ListNode>> = Some(Box::new(ListNode::new(0)));
        let mut tail: &mut Option<Box<ListNode>> = &mut dummy_head;
        for value in values {
            tail.as_mut().unwrap().next = Some(Box::new(ListNode::new(value)));
            tail = &mut tail.as_mut().unwrap().next;
        }
        dummy_head.unwrap().next
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_206() {
        assert_eq!(Solution::reverse_list(linked![1,2,3,4,5]), linked![5,4,3,2,1]);
    }
}

```
