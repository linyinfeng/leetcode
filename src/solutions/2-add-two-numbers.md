# Add Two Numbers

You are given two **non-empty** linked lists representing two non-negative integers. The digits are stored in **reverse order** and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Example:**

```ignore
Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
Explanation: 342 + 465 = 807.
```

## Solutions

### Solution 1

Time complexity: \\( \mathcal{O} ( max ( m , n ) ) \\)

Space complexity: \\( \mathcal{O} ( max ( m , n ) ) \\)

Where \\( m \\) and \\( n \\) is the length of the input lists.

```rust, edition2018, no_run
# #[derive(PartialEq, Eq, Debug)]
# pub struct ListNode {
#     pub val: i32,
#     pub next: Option<Box<ListNode>>,
# }
#
# impl ListNode {
#     #[inline]
#     pub fn new(val: i32) -> Self {
#         ListNode { next: None, val }
#     }
# }
#
pub fn add_two_numbers(
    l1: Option<Box<ListNode>>,
    l2: Option<Box<ListNode>>,
) -> Option<Box<ListNode>> {
    let mut result = None;

    let mut current: &mut Option<Box<ListNode>> = &mut result;
    let mut p1: &Option<Box<ListNode>> = &l1;
    let mut p2: &Option<Box<ListNode>> = &l2;

    let mut carry = 0;
    loop {
        // check finished
        if p1.is_none() && p2.is_none() && carry == 0 { break; }

        let mut sum = carry;
        if let Some(node) = p1 { sum += node.val; }
        if let Some(node) = p2 { sum += node.val; }
        let value = sum % 10;
        carry = sum / 10;

        // create new node
        *current = Some(Box::new(ListNode::new(value)));

        // move references
        current = &mut current.as_mut().unwrap().next;
        if p1.is_some() { p1 = &p1.as_ref().unwrap().next; }
        if p2.is_some() { p2 = &p2.as_ref().unwrap().next; }
    }
    match result {
        None => Some(Box::new(ListNode::new(0))), // result is zero
        Some(_) => result,
    }
}

# macro_rules! linked_list {
#     ($($v:literal)->*) => {
#         {
#             let mut list: Option<Box<ListNode>> = None;
#             let p = &mut list;
#             $(
#                 *p = Some(Box::new(ListNode::new($v)));
#                 let p = &mut p.as_mut().unwrap().next;
#             )*
#             *p = None; // suppress unused warning
#             list
#         }
#     }
# }
#
# macro_rules! test {
#     (($($v1:literal)->*) + ($($v2:literal)->*) = ($($res:literal)->*)) => {
#         assert_eq!(
#             add_two_numbers(linked_list!($($v1)->*), linked_list!($($v2)->*)),
#             linked_list!($($res)->*)
#         )
#     }
# }
#
test!((2 -> 4 -> 3) + (5 -> 6 -> 4) = (7 -> 0 -> 8));
test!((0 -> 1) + (0 -> 1 -> 2) = (0 -> 2 -> 2));
test!(() + (0 -> 1) = (0 -> 1));
test!((9 -> 9) + (1) = (0 -> 0 -> 1));
```
