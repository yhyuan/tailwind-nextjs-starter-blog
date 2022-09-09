---
title: Leetcode 0148 sort list
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0148 sort list in Rust
---

## Solution in Rust

```rust
/**
 * [148] Sort List
 *
 * Given the head of a linked list, return the list after sorting it in ascending order.
 * Follow up: Can you sort the linked list in O(n logn) time and O(1) memory (i.e. constant space)?
 *
 * Example 1:
 * <img alt="" src="https://assets.leetcode.com/uploads/2020/09/14/sort_list_1.jpg" style="width: 450px; height: 194px;" />
 * Input: head = [4,2,1,3]
 * Output: [1,2,3,4]
 *
 * Example 2:
 * <img alt="" src="https://assets.leetcode.com/uploads/2020/09/14/sort_list_2.jpg" style="width: 550px; height: 184px;" />
 * Input: head = [-1,5,3,4,0]
 * Output: [-1,0,3,4,5]
 *
 * Example 3:
 *
 * Input: head = []
 * Output: []
 *
 *
 * Constraints:
 *
 * 	The number of nodes in the list is in the range [0, 5 * 10^4].
 * 	-10^5 <= Node.val <= 10^5
 *
 */
pub struct Solution {}
use crate::util::linked_list::{ListNode, to_list};

// problem: https://leetcode.com/problems/sort-list/
// discuss: https://leetcode.com/problems/sort-list/discuss/?currentPage=1&orderBy=most_votes&query=

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
    pub fn sort_list(head: Option<Box<ListNode>>) -> Option<Box<ListNode>> {
        let mut p = head.as_ref();
        let mut values: Vec<i32> = vec![];
        while p.is_some() {
            let val = p.unwrap().val;
            values.push(val);
            p = p.unwrap().next.as_ref();
        }
        values.sort();
        let mut dummy_head = Some(Box::new(ListNode::new(0)));
        let mut tail = &mut dummy_head;
        for &val in values.iter() {
            //let node = Some(Box::new(ListNode::new(val)));
            tail.as_mut().unwrap().next = Some(Box::new(ListNode::new(val)));
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
    fn test_148() {
        assert_eq!(Solution::sort_list(linked![4,2,1,3]), linked![1,2,3,4]);
        assert_eq!(Solution::sort_list(linked![-1,5,3,4,0]), linked![-1,0,3,4,5]);
    }
}

```
