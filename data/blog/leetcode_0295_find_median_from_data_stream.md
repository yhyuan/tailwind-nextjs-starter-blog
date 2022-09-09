---
title: Leetcode 0295 find median from data stream
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0295 find median from data stream in Rust
---

## Solution in Rust

```rust
/**
 * [295] Find Median from Data Stream
 *
 * The median is the middle value in an ordered integer list. If the size of the list is even, there is no middle value and the median is the mean of the two middle values.
 *
 * 	For example, for arr = [2,3,4], the median is 3.
 * 	For example, for arr = [2,3], the median is (2 + 3) / 2 = 2.5.
 *
 * Implement the MedianFinder class:
 *
 * 	MedianFinder() initializes the MedianFinder object.
 * 	void addNum(int num) adds the integer num from the data stream to the data structure.
 * 	double findMedian() returns the median of all elements so far. Answers within 10^-5 of the actual answer will be accepted.
 *
 *
 * Example 1:
 *
 * Input
 * ["MedianFinder", "addNum", "addNum", "findMedian", "addNum", "findMedian"]
 * [[], [1], [2], [], [3], []]
 * Output
 * [null, null, null, 1.5, null, 2.0]
 * Explanation
 * MedianFinder medianFinder = new MedianFinder();
 * medianFinder.addNum(1);    // arr = [1]
 * medianFinder.addNum(2);    // arr = [1, 2]
 * medianFinder.findMedian(); // return 1.5 (i.e., (1 + 2) / 2)
 * medianFinder.addNum(3);    // arr[1, 2, 3]
 * medianFinder.findMedian(); // return 2.0
 *
 *
 * Constraints:
 *
 * 	-10^5 <= num <= 10^5
 * 	There will be at least one element in the data structure before calling findMedian.
 * 	At most 5 * 10^4 calls will be made to addNum and findMedian.
 *
 *
 * Follow up:
 *
 * 	If all integer numbers from the stream are in the range [0, 100], how would you optimize your solution?
 * 	If 99% of all integer numbers from the stream are in the range [0, 100], how would you optimize your solution?
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/find-median-from-data-stream/
// discuss: https://leetcode.com/problems/find-median-from-data-stream/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
use std::collections::BinaryHeap;
struct MedianFinder {
    large_heap: BinaryHeap<i32>,
    small_heap: BinaryHeap<i32>,
}


/**
 * `&self` means the method takes an immutable reference.
 * If you need a mutable reference, change it to `&mut self` instead.
 */
impl MedianFinder {

    /** initialize your data structure here. */
    fn new() -> Self {
        let large_heap: BinaryHeap<i32> = BinaryHeap::new();//reverse
        let small_heap: BinaryHeap<i32> = BinaryHeap::new();
        Self{small_heap, large_heap}
    }

    fn add_num(&mut self, num: i32) {
        if self.large_heap.len() == self.small_heap.len() {
            //add to small
            if self.small_heap.len() == 0 {
                self.small_heap.push(num);
                return;
            }
            let small_heap_max_val = *self.small_heap.peek().unwrap();
            if num < small_heap_max_val {
                self.small_heap.push(num);
            } else {
                self.large_heap.push(-num);
                let large_heap_min_val = -self.large_heap.pop().unwrap();
                self.small_heap.push(large_heap_min_val);
            }
        } else {
            let small_heap_max_val = *self.small_heap.peek().unwrap();
            if num < small_heap_max_val {
                self.small_heap.push(num);
                let small_val = self.small_heap.pop().unwrap();
                self.large_heap.push(-small_val);
            } else {
                self.large_heap.push(-num);
            }
        }
    }

    fn find_median(&self) -> f64 {
        if self.large_heap.len() == self.small_heap.len() {
            let large_val = -(*self.large_heap.peek().unwrap());
            let small_val = *self.small_heap.peek().unwrap();
            (large_val as f64 + small_val as f64) * 0.5f64
        } else {
            let small_val = *self.small_heap.peek().unwrap();
            small_val as f64
        }
    }
}

/**
 * Your MedianFinder object will be instantiated and called as such:
 * let obj = MedianFinder::new();
 * obj.add_num(num);
 * let ret_2: f64 = obj.find_median();
 */

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_295() {
        let mut obj = MedianFinder::new();
        obj.add_num(1);
        obj.add_num(2);
        assert_eq!(obj.find_median(), 1.5);
        obj.add_num(3);
        assert_eq!(obj.find_median(), 2_f64);
    }
}

```
