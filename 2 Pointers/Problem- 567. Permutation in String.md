Source: [[Leetcode]]
Difficulty: [[Medium]]
Topics: [[2 pointers]], [[Sliding Window]], [[Frequency Map]]

This question DID NOT feel like I'm solving a medium at all. I couldn't come up with the solution myself at all. I wasn't even able to do it with a little bit of guidance from neetcode. I had to watch the entire solution video, and still try to break the code down to understand the approach.

I'm still not 100% sure that I'll be able to solve a question like this yet if I get it in the future so I need to work more upon this.

In this [[DSA]] problem, given two strings `s1` and `s2`, return `true` if `s2` contains a permutation of `s1`, or `false` otherwise.

In other words, return `true` if one of `s1`'s permutations is the substring of `s2`.

**Example 1:**

**Input:** s1 = "ab", s2 = "eidbaooo"
**Output:** true
**Explanation:** s2 contains one permutation of s1 ("ba").

**Example 2:**

**Input:** s1 = "ab", s2 = "eidboaoo"
**Output:** false

## Approach- Using [[2 pointers]]
We want to find if *any* permutation of s1 exists *somewhere* in s2. 
