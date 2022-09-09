---
title: Leetcode 0023 merge k sorted lists
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0023 merge k sorted lists in Rust
---

## Solution in Rust

```rust
/**
 * [23] Merge k Sorted Lists
 *
 * You are given an array of k linked-lists lists, each linked-list is sorted in ascending order.
 * Merge all the linked-lists into one sorted linked-list and return it.
 *
 * Example 1:
 *
 * Input: lists = [[1,4,5],[1,3,4],[2,6]]
 * Output: [1,1,2,3,4,4,5,6]
 * Explanation: The linked-lists are:
 * [
 *   1->4->5,
 *   1->3->4,
 *   2->6
 * ]
 * merging them into one sorted list:
 * 1->1->2->3->4->4->5->6
 *
 * Example 2:
 *
 * Input: lists = []
 * Output: []
 *
 * Example 3:
 *
 * Input: lists = [[]]
 * Output: []
 *
 *
 * Constraints:
 *
 * 	k == lists.length
 * 	0 <= k <= 10^4
 * 	0 <= lists[i].length <= 500
 * 	-10^4 <= lists[i][j] <= 10^4
 * 	lists[i] is sorted in ascending order.
 * 	The sum of lists[i].length won't exceed 10^4.
 *
 */
pub struct Solution {}
use crate::util::linked_list::{ListNode, to_list};

// problem: https://leetcode.com/problems/merge-k-sorted-lists/
// discuss: https://leetcode.com/problems/merge-k-sorted-lists/discuss/?currentPage=1&orderBy=most_votes&query=

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
use std::collections::BinaryHeap;
use std::cmp::Ordering;
#[derive(Debug)]
pub struct Node (i32, usize);
impl Eq for Node {}
impl Ord for Node {
    fn cmp(&self, other: &Self) -> Ordering {
        self.0.cmp(&other.0).reverse()
    }
}
impl PartialEq for Node {
    fn eq(&self, other: &Self) -> bool {
        self.0.eq(&other.0)
    }
}
impl PartialOrd for Node {
    fn partial_cmp(&self, other: &Self) -> Option<Ordering> {
        Some(self.0.cmp(&other.0).reverse())
    }
}
impl Solution {
    pub fn merge_k_lists(lists: Vec<Option<Box<ListNode>>>) -> Option<Box<ListNode>> {
        let mut dummy_head: Option<Box<ListNode>> = Some(Box::new(ListNode::new(0)));
        let mut tail: &mut Option<Box<ListNode>> = &mut dummy_head;

        let mut points: Vec<Option<&Box<ListNode>>> = lists.iter()
            .filter(|list| list.is_some())
            .map(|list| list.as_ref())
            .collect();
        let values: Vec<Node> = (0..points.len()).into_iter()
            .map(|i| Node(points[i].unwrap().val, i))
            .collect();
        points = points.iter().map(|&p| p.unwrap().next.as_ref()).collect();
        let mut min_heap: BinaryHeap<Node> = BinaryHeap::from(values);
        loop {
            let item = min_heap.pop();
            if item.is_none() {
                break;
            }
            let node = item.unwrap();
            let value = node.0;
            let index = node.1;
            tail.as_mut().unwrap().next = Some(Box::new(ListNode::new(value)));
            tail = &mut tail.as_mut().unwrap().next;
            let mut p = points[index];
            if p.is_some() {
                min_heap.push(Node(p.unwrap().val, index));
                points[index] = p.unwrap().next.as_ref();
            }
        }
        dummy_head.unwrap().next
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_23() {
        assert_eq!(
            Solution::merge_k_lists(vec![
                to_list(vec![1, 4, 5]),
                to_list(vec![1, 3, 4]),
                to_list(vec![2, 6]),
            ]),
            to_list(vec![1, 1, 2, 3, 4, 4, 5, 6])
        );
        assert_eq!(Solution::merge_k_lists(vec![]), None);
    }
}

```
