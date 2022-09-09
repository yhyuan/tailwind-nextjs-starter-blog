---
title: Leetcode 0071 simplify path
date: '2022-09-09'
tags: ['leetcode']
draft: false
summary: Solution for leetcode 0071 simplify path in Rust
---

## Solution in Rust

```rust
/**
 * [71] Simplify Path
 *
 * Given a string path, which is an absolute path (starting with a slash '/') to a file or directory in a Unix-style file system, convert it to the simplified canonical path.
 * In a Unix-style file system, a period '.' refers to the current directory, a double period '..' refers to the directory up a level, and any multiple consecutive slashes (i.e. '//') are treated as a single slash '/'. For this problem, any other format of periods such as '...' are treated as file/directory names.
 * The canonical path should have the following format:
 *
 * 	The path starts with a single slash '/'.
 * 	Any two directories are separated by a single slash '/'.
 * 	The path does not end with a trailing '/'.
 * 	The path only contains the directories on the path from the root directory to the target file or directory (i.e., no period '.' or double period '..')
 *
 * Return the simplified canonical path.
 *
 * Example 1:
 *
 * Input: path = "/home/"
 * Output: "/home"
 * Explanation: Note that there is no trailing slash after the last directory name.
 *
 * Example 2:
 *
 * Input: path = "/../"
 * Output: "/"
 * Explanation: Going one level up from the root directory is a no-op, as the root level is the highest level you can go.
 *
 * Example 3:
 *
 * Input: path = "/home//foo/"
 * Output: "/home/foo"
 * Explanation: In the canonical path, multiple consecutive slashes are replaced by a single one.
 *
 * Example 4:
 *
 * Input: path = "/a/./b/../../c/"
 * Output: "/c"
 *
 *
 * Constraints:
 *
 * 	1 <= path.length <= 3000
 * 	path consists of English letters, digits, period '.', slash '/' or '_'.
 * 	path is a valid absolute Unix path.
 *
 */
pub struct Solution {}

// problem: https://leetcode.com/problems/simplify-path/
// discuss: https://leetcode.com/problems/simplify-path/discuss/?currentPage=1&orderBy=most_votes&query=

// submission codes start here

impl Solution {
    pub fn simplify_path(path: String) -> String {
        let items = path.split("/").filter(|&str| str.len() > 0 && str != ".").collect::<Vec<&str>>();
        let mut stack: Vec<&str> = vec![];
        for i in 0..items.len() {
            let item = items[i];
            if item == ".." {
                if stack.len() > 0 {
                    stack.pop();
                }
            } else {
                stack.push(item);
            }
        }
        format!("/{}", stack.join("/"))
    }
}

// submission codes end

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    fn test_71() {
        assert_eq!(Solution::simplify_path("/home/".to_owned()), "/home");
        assert_eq!(Solution::simplify_path("/../".to_owned()), "/");
        assert_eq!(Solution::simplify_path("/a/./b/../../c/".to_owned()), "/c");
        assert_eq!(
            Solution::simplify_path("/a/../../b/../c//.//".to_owned()),
            "/c"
        );
        assert_eq!(
            Solution::simplify_path("/a//b////c/d//././/..".to_owned()),
            "/a/b/c"
        );
    }
}

```
