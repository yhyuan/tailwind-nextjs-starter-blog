---
title: Leetcode 0146 lru cache
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0146 lru cache in Rust
---

## Solution in Rust

```rust
/**
 * [146] LRU Cache
 *
 * Design a data structure that follows the constraints of a <a href="https://en.wikipedia.org/wiki/Cache_replacement_policies#LRU" target="_blank">Least Recently Used (LRU) cache</a>.
 * Implement the LRUCache class:
 *
 * 	LRUCache(int capacity) Initialize the LRU cache with positive size capacity.
 * 	int get(int key) Return the value of the key if the key exists, otherwise return -1.
 * 	void put(int key, int value) Update the value of the key if the key exists. Otherwise, add the key-value pair to the cache. If the number of keys exceeds the capacity from this operation, evict the least recently used key.
 *
 * The functions <code data-stringify-type="code">get and <code data-stringify-type="code">put must each run in O(1) average time complexity.
 *
 * Example 1:
 *
 * Input
 * ["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
 * [[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
 * Output
 * [null, null, null, 1, null, -1, null, -1, 3, 4]
 * Explanation
 * LRUCache lRUCache = new LRUCache(2);
 * lRUCache.put(1, 1); // cache is {1=1}
 * lRUCache.put(2, 2); // cache is {1=1, 2=2}
 * lRUCache.get(1);    // return 1
 * lRUCache.put(3, 3); // LRU key was 2, evicts key 2, cache is {1=1, 3=3}
 * lRUCache.get(2);    // returns -1 (not found)
 * lRUCache.put(4, 4); // LRU key was 1, evicts key 1, cache is {4=4, 3=3}
 * lRUCache.get(1);    // return -1 (not found)
 * lRUCache.get(3);    // return 3
 * lRUCache.get(4);    // return 4
 *
 *
 * Constraints:
 *
 * 	1 <= capacity <= 3000
 * 	0 <= key <= 10^4
 * 	0 <= value <= 10^5
 * 	At most 2 * 10^5 calls will be made to get and put.
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/lru-cache/
// discuss: https://leetcode.com/problems/lru-cache/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
use std::collections::HashMap;
struct LRUCache {
    values_hashmap: HashMap<i32, i32>,
    priority_hashmap: HashMap<i32, i32>,
    capacity: usize,
    max_priority: i32,
}


/**
 * `&self` means the method takes an immutable reference.
 * If you need a mutable reference, change it to `&mut self` instead.
 */
impl LRUCache {

    fn new(capacity: i32) -> Self {
        let capacity = capacity as usize;
        let values_hashmap: HashMap<i32, i32> = HashMap::with_capacity(capacity);
        let priority_hashmap: HashMap<i32, i32> = HashMap::with_capacity(capacity);
        let max_priority = i32::MIN;
        Self {capacity, values_hashmap, priority_hashmap, max_priority}
    }
    fn update_priority(&mut self) {
        let mut hashmap: HashMap<i32, i32> = HashMap::with_capacity(self.capacity);
        let mut priority_vec: Vec<i32> = vec![];
        for (&k, &priority) in self.priority_hashmap.iter() {
            hashmap.insert(priority, k);
            priority_vec.push(priority);
        }
        priority_vec.sort();
        let mut new_priority_hashmap: HashMap<i32, i32> = HashMap::with_capacity(self.capacity);
        for i in 0..priority_vec.len() {
            let old_priority = priority_vec[i];
            let value = hashmap[&old_priority];
            new_priority_hashmap.insert(value, i32::MIN + i as i32);
        }
        self.priority_hashmap = new_priority_hashmap;
    }
    fn get(&mut self, key: i32) -> i32 {
        if !self.values_hashmap.contains_key(&key) {
            return -1;
        }
        if self.max_priority == i32::MAX {
            self.update_priority();
        }
        let max_priority = self.max_priority + 1;
        self.priority_hashmap.insert(key, max_priority);
        self.max_priority = max_priority;
        self.values_hashmap[&key]
    }

    fn put(&mut self, key: i32, value: i32) {
        if self.max_priority == i32::MAX {
            self.update_priority();
        }
        let max_priority = self.max_priority + 1;
        self.max_priority = max_priority;
        if self.values_hashmap.len() < self.capacity {
            self.priority_hashmap.insert(key, max_priority);
            self.values_hashmap.insert(key, value);
        } else {
            if self.values_hashmap.contains_key(&key) {
                self.priority_hashmap.insert(key, max_priority);
                self.values_hashmap.insert(key, value);
            } else {
                let mut min_priority = i32::MAX;
                let mut min_priority_key = 0i32;
                for (&k, &priority) in self.priority_hashmap.iter() {
                    if priority < min_priority {
                        min_priority_key = k;
                        min_priority = priority;
                    }
                }
                self.values_hashmap.remove_entry(&min_priority_key);
                self.priority_hashmap.remove_entry(&min_priority_key);

                self.priority_hashmap.insert(key, max_priority);
                self.values_hashmap.insert(key, value);
            }
        }
    }
}

/**
 * Your LRUCache object will be instantiated and called as such:
 * let obj = LRUCache::new(capacity);
 * let ret_1: i32 = obj.get(key);
 * obj.put(key, value);
 */

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_146() {
        let mut cache: LRUCache = LRUCache::new(2);
        cache.put(1, 1); // {1: 1}
        cache.put(2, 2); // {1: 1, 2: 2}
        assert_eq!(cache.get(1), 1);
        cache.put(3, 3); // {1: 1, 3: 3}
        assert_eq!(cache.get(2), -1);
        cache.put(4, 4);
        assert_eq!(cache.get(1), -1);
        assert_eq!(cache.get(3), 3);
        assert_eq!(cache.get(4), 4);


        let mut cache: LRUCache = LRUCache::new(2);
        assert_eq!(cache.get(2), -1);
        cache.put(2, 6);
        assert_eq!(cache.get(1), -1);
        cache.put(1, 5);
        cache.put(1, 2);
        assert_eq!(cache.get(1), 2);
        assert_eq!(cache.get(2), 6);
    }
}

```
