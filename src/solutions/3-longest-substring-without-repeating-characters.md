# Longest Substring Without Repeating Characters

<https://leetcode.com/problems/longest-substring-without-repeating-characters/>

Difficulty: Medium

## Description

Given a string, find the length of the **longest substring** without repeating characters.

### Examples

#### Example 1

```ignore
Input: "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.
```

#### Example 2

```ignore
Input: "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
```

#### Example 3

```ignore
Input: "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.
             Note that the answer must be a substring, "pwke" is a subsequence and not a substring.
```

## Solutions

### Solution 1

Time complexity: \\( \mathcal{O} ( n ) \\)

Space complexity: \\( \mathcal{O} ( m ) \\)

where \\( n \\) is the length of string,  \\( m \\) is the number of unique characters in the string.

```rust, edition2018, no_run
use std::collections::HashMap;

pub fn length_of_longest_substring(s: String) -> i32 {
    let mut longest = 0;
    let mut last_location = HashMap::new();
    let mut first = 0;
    for (i, c) in s.chars().enumerate() {
        match last_location.get(&c) {
            Some(conflict) if *conflict >= first => {
                longest = longest.max(i - first);
                first = conflict + 1;
            },
            _ => (),
        };
        last_location.insert(c, i);
    }
    longest.max(s.len() - first) as i32
}

assert_eq!(length_of_longest_substring(String::from("")), 0);
assert_eq!(length_of_longest_substring(String::from("abcabcbb")), 3);
assert_eq!(length_of_longest_substring(String::from("bbbbb")), 1);
assert_eq!(length_of_longest_substring(String::from("pwwkew")), 3);
assert_eq!(length_of_longest_substring(String::from("aab")), 2);
assert_eq!(length_of_longest_substring(String::from("cdd")), 2);
assert_eq!(length_of_longest_substring(String::from("abba")), 2);
assert_eq!(length_of_longest_substring(String::from("ohomm")), 3);
```
