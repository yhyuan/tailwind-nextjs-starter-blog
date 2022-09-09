---
title: Leetcode 2156 find substring with given hash value
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 2156 find substring with given hash value in Rust
---

## Solution in Rust

```rust
/**
2156. Find Substring With Given Hash Value
Medium

166

274

Add to List

Share
The hash of a 0-indexed string s of length k, given integers p and m, is computed using the following function:

hash(s, p, m) = (val(s[0]) * p0 + val(s[1]) * p1 + ... + val(s[k-1]) * pk-1) mod m.
Where val(s[i]) represents the index of s[i] in the alphabet from val('a') = 1 to val('z') = 26.

You are given a string s and the integers power, modulo, k, and hashValue. Return sub, the first substring of s of length k such that hash(sub, power, modulo) == hashValue.

The test cases will be generated such that an answer always exists.

A substring is a contiguous non-empty sequence of characters within a string.



Example 1:

Input: s = "leetcode", power = 7, modulo = 20, k = 2, hashValue = 0
Output: "ee"
Explanation: The hash of "ee" can be computed to be hash("ee", 7, 20) = (5 * 1 + 5 * 7) mod 20 = 40 mod 20 = 0.
"ee" is the first substring of length 2 with hashValue 0. Hence, we return "ee".
Example 2:

Input: s = "fbxzaad", power = 31, modulo = 100, k = 3, hashValue = 32
Output: "fbx"
Explanation: The hash of "fbx" can be computed to be hash("fbx", 31, 100) = (6 * 1 + 2 * 31 + 24 * 312) mod 100 = 23132 mod 100 = 32.
The hash of "bxz" can be computed to be hash("bxz", 31, 100) = (2 * 1 + 24 * 31 + 26 * 312) mod 100 = 25732 mod 100 = 32.
"fbx" is the first substring of length 3 with hashValue 32. Hence, we return "fbx".
Note that "bxz" also has a hash of 32 but it appears later than "fbx".


Constraints:

1 <= k <= s.length <= 2 * 104
1 <= power, modulo <= 109
0 <= hashValue < modulo
s consists of lowercase English letters only.
The test cases are generated such that an answer always exists.
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/groups-of-strings/
// discuss: https://leetcode.com/problems/groups-of-strings/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here
impl Solution {
    pub fn sub_str_hash(s: String, power: i32, modulo: i32, k: i32, hash_value: i32) -> String {
   let chars: Vec<i64> = s.chars().into_iter().map(|ch| ch as i64 - 'a' as i64 + 1).collect();
    //println!("{:?}", chars);
    let n = s.len();
    let k = k as usize;
    let power = power as i64;
    let modulo = modulo as i64;
    let hash_value = hash_value as i64;
    let mut powers: Vec<i64> = vec![1];
    for _ in 0..k {
        let v = ( powers.last().unwrap() * power ) % (modulo);
        powers.push(v);
    }
    let mut h = 0;
    for i in n - k..n {
        h = (h + chars[i] * powers[i - (n - k)]) % modulo;
    }
    let mut hash_values: Vec<i64> = vec![h];
    for i in (0..n - k).rev() {
        // i + k will be out, i will be in.
        let x = (h + modulo - (chars[i + k] * powers[k - 1] % modulo)) % modulo;
        h = (chars[i] + power * x) % modulo;
        // println!("h: {:?}", h);
        hash_values.insert(0, h);
    }
    // println!("hash_values: {:?}", hash_values);
    for i in 0..hash_values.len() {
        if hash_values[i] == hash_value {
            let s: String = format!("{}", &s[i..i + k]);
            return s;
        }
    }

    "".to_string()
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_2156() {
        assert_eq!(Solution::sub_str_hash("leetcode".to_string(), 7, 20, 2, 0), "ee".to_string());
        assert_eq!(Solution::sub_str_hash("fbxzaad".to_string(), 31, 100, 3, 32), "fbx".to_string());
    }
}
```
