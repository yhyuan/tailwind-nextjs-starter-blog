---
title: Leetcode 0004 median of two sorted arrays
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0004 median of two sorted arrays in Rust
---

## Solution in Rust

```rust
/**
 * [4] Median of Two Sorted Arrays
 *
 * Given two sorted arrays nums1 and nums2 of size m and n respectively, return the median of the two sorted arrays.
 * The overall run time complexity should be O(log (m+n)).
 *
 * Example 1:
 *
 * Input: nums1 = [1,3], nums2 = [2]
 * Output: 2.00000
 * Explanation: merged array = [1,2,3] and median is 2.
 *
 * Example 2:
 *
 * Input: nums1 = [1,2], nums2 = [3,4]
 * Output: 2.50000
 * Explanation: merged array = [1,2,3,4] and median is (2 + 3) / 2 = 2.5.
 *
 * Example 3:
 *
 * Input: nums1 = [0,0], nums2 = [0,0]
 * Output: 0.00000
 *
 * Example 4:
 *
 * Input: nums1 = [], nums2 = [1]
 * Output: 1.00000
 *
 * Example 5:
 *
 * Input: nums1 = [2], nums2 = []
 * Output: 2.00000
 *
 *
 * Constraints:
 *
 * 	nums1.length == m
 * 	nums2.length == n
 * 	0 <= m <= 1000
 * 	0 <= n <= 1000
 * 	1 <= m + n <= 2000
 * 	-10^6 <= nums1[i], nums2[i] <= 10^6
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/median-of-two-sorted-arrays/
// discuss: https://leetcode.com/problems/median-of-two-sorted-arrays/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
/*
impl Solution {
    pub fn find_median_one_sorted_array(nums: &Vec<i32>) -> f64 {
        let n = nums.len();
        let n_2 = n / 2;
        if n % 2 == 1 {
            nums[n_2] as f64
        } else {
            0.5f64 * ((nums[n_2] + nums[n_2 - 1]) as f64)
        }
    }
    pub fn find_median_two_separated_sorted_array(nums1: &Vec<i32>, nums2: &Vec<i32>) -> f64 {
        let m = nums1.len();
        let n = nums2.len();
        let middle = (m + n) / 2;
        let median = if middle < m {
            nums1[middle] as f64
        } else {
            nums2[middle - m]  as f64
        };
        if (m + n) % 2 == 1 {
            return median;
        }
        let median_1 = if middle - 1 < m {
            nums1[middle - 1] as f64
        } else {
            nums2[middle - 1 - m]  as f64
        };
        return 0.5f64 * (median + median_1);
    }
    pub fn find_median_sorted_arrays(nums1: Vec<i32>, nums2: Vec<i32>) -> f64 {
        if nums1.len() == 0 {
            return Solution::find_median_one_sorted_array(&nums2);
        }
        if nums2.len() == 0 {
            return Solution::find_median_one_sorted_array(&nums1);
        }
        if nums1[nums1.len() - 1] <= nums2[0] {
            return Solution::find_median_two_separated_sorted_array(&nums1, &nums2);
        }
        if nums2[nums2.len() - 1] <= nums1[0] {
            return Solution::find_median_two_separated_sorted_array(&nums2, &nums1);
        }
        let m = nums1.len();
        let n = nums2.len();
        let middle = (m + n) / 2;
        let mut i = 0usize;
        let mut j = 0usize;
        let mut pre_median = i32::MIN;
        let mut median = i32::MIN;
        while i + j <= middle {
            if i < nums1.len() && j < nums2.len() {
                if nums1[i] <= nums2[j] {
                    pre_median = median;
                    median = nums1[i];
                    i = i + 1;
                } else {
                    pre_median = median;
                    median = nums2[j];
                    j = j + 1;
                }
            } else if i == nums1.len() {
                pre_median = median;
                median = nums2[j];
                j = j + 1;
            } else if j == nums2.len() {
                pre_median = median;
                median = nums1[i];
                i = i + 1;
            } else {
                panic!("Wrong data!");
            }
        }
        //println!("pre_median: {}", pre_median);
        //println!("median: {}", median);
        if (m + n) % 2 == 1 {
            median as f64
        } else {
            0.5f64 * (median as f64 + pre_median as f64)
        }
    }
}
*/
impl Solution {
    pub fn find_median_one_sorted_array(nums: &Vec<i32>) -> f64 {
        let n = nums.len();
        let n_2 = n / 2;
        if n % 2 == 1 {
            nums[n_2] as f64
        } else {
            0.5f64 * ((nums[n_2] + nums[n_2 - 1]) as f64)
        }
    }
    pub fn find_kth_number(nums1: &Vec<i32>, start_1: usize, end_1: usize, nums2: &Vec<i32>, start_2: usize, end_2: usize, k: usize) -> i32 {
        //println!("nums1: {:?}, nums2: {:?}, k: {}", &nums1[start_1..=end_1], &nums2[start_2..=end_2], k);
        if end_1 < start_1 {
            return nums2[start_2 + k - 1];
        }
        if end_2 < start_2 {
            return nums1[start_1 + k - 1];
        }
        if k == 1 { //end1 > start1, end2 > start2
            return i32::min(nums1[start_1], nums2[start_2]);
        }
        //start1 + k/2 - 1
        let index_1 = usize::min(start_1 + k/2 - 1, end_1);
        let index_2 = usize::min(start_2 + k/2 - 1, end_2);
        if nums1[index_1] > nums2[index_2] {
            if index_2 == end_2 {
                Solution::find_kth_number(nums1, start_1, end_1, nums2, start_2 + k/2, end_2, k - (end_2 - start_2 + 1))
            } else {
                Solution::find_kth_number(nums1, start_1, end_1, nums2, start_2 + k/2, end_2, k - k/2)
            }
        } else {
            if index_1 == end_1 {
                Solution::find_kth_number(nums1, start_1 + k/2, end_1, nums2, start_2, end_2, k - (end_1 - start_1 + 1))
            } else {
                Solution::find_kth_number(nums1, start_1 + k/2, end_1, nums2, start_2, end_2, k - k/2)
            }
        }
    }
    pub fn find_median_sorted_arrays(nums1: Vec<i32>, nums2: Vec<i32>) -> f64 {
        let m = nums1.len();
        let n = nums2.len();
        if m == 0 {
            return Solution::find_median_one_sorted_array(&nums2);
        }
        if n == 0 {
            return Solution::find_median_one_sorted_array(&nums1);
        }

        let val = Solution::find_kth_number(&nums1, 0, m - 1, &nums2, 0, n - 1, (m + n)/2 + 1);
        //println!("The {} th number is {}", (m + n) / 2, val);

        if  (m + n) % 2 == 1 {
            val as f64
        } else {
            let val2 = Solution::find_kth_number(&nums1, 0, m - 1, &nums2, 0, n - 1, (m + n)/2);
            0.5f64 * ((val + val2) as f64)
        }
    }
}
// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_4() {
        assert_eq!(
            Solution::find_median_sorted_arrays(
                vec![1, 3, 4, 9],
                vec![1, 2, 3, 4, 5, 6, 7, 8, 9, 10]),
            4.5
        );
        assert_eq!(
            Solution::find_median_sorted_arrays(vec![1, 3], vec![2, 4]),
            2.5
        );
        assert_eq!(
            Solution::find_kth_number(&vec![1, 3], 0, 1, &vec![2], 0, 0, 2),
            2
        );
        assert_eq!(
            Solution::find_median_sorted_arrays(vec![1, 3], vec![2]),
            2.0
        );
        assert_eq!(
            Solution::find_median_sorted_arrays(vec![1, 2], vec![3, 4]),
            2.5
        );
        assert_eq!(
            Solution::find_median_sorted_arrays(vec![1], vec![2, 3, 4, 5, 6]),
            3.5
        );
    }
}

```
