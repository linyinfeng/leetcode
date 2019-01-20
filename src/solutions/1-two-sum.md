# Two Sum

Given an array of integers, return **indices** of the two numbers such that they add up to a specific target.

You may assume that each input would have **exactly** one solution, and you may not use the same element twice.

**Example:**

```ignore
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```

## Solutions

### Solution 1

Time complexity: \\( \mathcal{O} ( n ^ 2 ) \\)

Space complexity: \\( \mathcal{O} ( 1 ) \\)

```rust, edition2018, no_run
pub fn two_sum(nums: Vec<i32>, target: i32) -> Vec<i32> {
    for (num, i) in nums.iter().zip(0..) {
        for j in i + 1..nums.len() {
            if num + nums[j] == target {
                return vec![i as i32, j as i32];
            }
        }
    }
    panic!("no solution")
}

assert_eq!(two_sum(vec![2, 7, 11, 15], 9), vec![0, 1]);
assert_eq!(two_sum(vec![3, 2, 4], 6), vec![1, 2]);
```

### Solution 2

Time complexity: \\( \mathcal{O} ( n ) \\)

Space complexity: \\( \mathcal{O} ( n ) \\)

```rust, edition2018, no_run
use std::collections::HashMap;

pub fn two_sum(nums: Vec<i32>, target: i32) -> Vec<i32> {
    let visited: HashMap<_, _> = nums.iter().cloned().zip(0..).collect();

    for (num, i) in nums.into_iter().zip(0..) {
        let pair = target - num;
        match visited.get(&pair) {
            Some(j) if i != *j => return vec![i, *j],
            _ => (),
        }
    }

    panic!("no solution")
}

assert_eq!(two_sum(vec![2, 7, 11, 15], 9), vec![0, 1]);
assert_eq!(two_sum(vec![3, 2, 4], 6), vec![1, 2]);
```

### Solution 3

Time complexity: \\( \mathcal{O} ( n ) \\)

Space complexity: \\( \mathcal{O} ( n ) \\)

```rust, edition2018, no_run
use std::collections::HashMap;

pub fn two_sum(nums: Vec<i32>, target: i32) -> Vec<i32> {
    let mut visited = HashMap::with_capacity(nums.len());

    for (num, i) in nums.into_iter().zip(0..) {
        let pair = target - num;
        if let Some(j) = visited.get(&pair) {
            return vec![*j, i];
        }
        visited.insert(num, i);
    }

    panic!("no solution")
}

assert_eq!(two_sum(vec![2, 7, 11, 15], 9), vec![0, 1]);
assert_eq!(two_sum(vec![3, 2, 4], 6), vec![1, 2]);
```
