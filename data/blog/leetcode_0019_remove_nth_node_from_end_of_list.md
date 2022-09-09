---
title: Leetcode 0019 remove nth node from end of list
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0019 remove nth node from end of list in Rust
---

## Solution in Rust

```rust
/**
 * [19] Remove Nth Node From End of List
 *
 * Given the head of a linked list, remove the n^th node from the end of the list and return its head.
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2020/10/03/remove_ex1.jpg" style="width: 542px; height: 222px;" />
 * Input: head = [1,2,3,4,5], n = 2
 * Output: [1,2,3,5]
 *
 * Example 2:
 *
 * Input: head = [1], n = 1
 * Output: []
 *
 * Example 3:
 *
 * Input: head = [1,2], n = 1
 * Output: [1]
 *
 *
 * Constraints:
 *
 * 	The number of nodes in the list is sz.
 * 	1 <= sz <= 30
 * 	0 <= Node.val <= 100
 * 	1 <= n <= sz
 *
 *
 * Follow up: Could you do this in one pass?
 *
 */
pub struct Solution {}
use crate::util::linked_list::{ListNode, to_list};

// problem: https://leetcode.com/problems/remove-nth-node-from-end-of-list/
// discuss: https://leetcode.com/problems/remove-nth-node-from-end-of-list/discuss/?currentPage=1&orderBy=most_votes&query=

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
    pub fn remove_nth_from_end(head: Option<Box<ListNode>>, n: i32) -> Option<Box<ListNode>> {
        let mut dummy_head: Option<Box<ListNode>> = Some(Box::new(ListNode{val: 0, next: head}));
        let mut len = 0;
        {
            let mut p: Option<&Box<ListNode>> = dummy_head.as_ref();
            while p.unwrap().next.is_some() { // p->next
                len += 1;
                p = p.unwrap().next.as_ref(); // p = p->next
            }
        }
        {
            let mut p: Option<&mut Box<ListNode>> = dummy_head.as_mut();
            for _ in 0..len - n {
                p = p.unwrap().next.as_mut();
            }
            let next = p.as_mut().unwrap().next.as_mut().unwrap().next.take();
            p.as_mut().unwrap().next = next;
        }
        dummy_head.unwrap().next
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_19() {
        assert_eq!(
            Solution::remove_nth_from_end(to_list(vec![1, 2, 3, 4, 5]), 2),
            to_list(vec![1, 2, 3, 5])
        );
        assert_eq!(Solution::remove_nth_from_end(to_list(vec![1]), 1), None);
    }
}

```
