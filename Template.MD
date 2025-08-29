```markdown
# How to Solve Dynamic Programming (DP) Problems on Strings: A Template and 4-Step Approach (Java Focus)

This guide provides a structured approach to tackle Dynamic Programming problems involving strings, offering a template and a detailed 4-step process. While the original post used C++, this version focuses on Java implementations and provides more in-depth explanations for beginners.

## Table of Contents

*   [Introduction](#introduction)
*   [DP Template for String Problems](#dp-template-for-string-problems)
    *   [Template 1: Two Strings](#template-1-two-strings)
    *   [Template 2: Single String (Interval DP)](#template-2-single-string-interval-dp)
*   [The 4-Step Approach to Solving String DP Problems](#the-4-step-approach-to-solving-string-dp-problems)
    *   [Step 1: Understand the Problem (The Hardest Part)](#step-1-understand-the-problem-the-hardest-part)
    *   [Step 2: Pre-processing & Define Base Cases](#step-2-pre-processing--define-base-cases)
    *   [Step 3: Define the Recurrence Relation (Main Logic)](#step-3-define-the-recurrence-relation-main-logic)
    *   [Step 4: Implement and Return the Result](#step-4-implement-and-return-the-result)
*   [Problem Examples](#problem-examples)
    *   [Problem 1: Edit Distance](#problem-1-edit-distance)
    *   [Problem 2: Delete Operations for Two Strings](#problem-2-delete-operations-for-two-strings)
    *   [Problem 3: Minimum ASCII Delete Sum for Two Strings](#problem-3-minimum-ascii-delete-sum-for-two-strings)
    *   [Problem 4: Longest Palindromic Subsequence](#problem-4-longest-palindromic-subsequence)
    *   [Problem 5: Palindromic Substrings](#problem-5-palindromic-substrings)
*   [Additional Tips](#additional-tips)

---

## Introduction

Dynamic Programming (DP) is a powerful technique for solving problems by breaking them down into smaller, overlapping subproblems. When dealing with strings, DP often involves building up solutions from substrings. This guide aims to demystify string DP by providing a clear template and a systematic approach.

The templates and the 4-step process are inspired by excellent resources, including @aatalyk's general DP patterns.

## DP Template for String Problems

Before diving into specific problems, let's look at the common structural templates for iterating through string problems in a DP context.

### Template 1: Two Strings

This template is typically used when your DP state `dp[i][j]` depends on characters from two different strings, say `s1` and `s2`.

```java
/* Pre-processing: Initialize DP table and define base cases. */
// Assuming s1 has length 'm' and s2 has length 'n'
// DP table size will usually be (m+1) x (n+1) to handle empty string cases.
// dp[i][j] typically represents the solution for s1[0...i-1] and s2[0...j-1]
int[][] dp = new int[m + 1][n + 1];

// Base cases initialization (e.g., first row/column)

for (int i = 1; i <= m; i++) { // Iterate through string s1
    for (int j = 1; j <= n; j++) { // Iterate through string s2
        // Note: s1.charAt(i-1) and s2.charAt(j-1) are used to access current characters
        // because dp[i][j] corresponds to substrings of length i and j.
        if (s1.charAt(i - 1) == s2.charAt(j - 1)) {
            /* Your code for matching characters */
            // Often depends on dp[i-1][j-1]
        } else {
            /* Your code for non-matching characters */
            // Often depends on dp[i-1][j], dp[i][j-1], or dp[i-1][j-1]
        }
    }
}
// Return dp[m][n] or relevant final state
```

### Template 2: Single String (Interval DP)

This template is common when your DP state `dp[i][j]` represents a property of a substring `s[i...j]` within a single string `s`. The iteration typically goes by `length` of the substring first, then `start index`.

```java
/* Pre-processing: Initialize DP table and define base cases. */
// Assuming s has length 'n'
// dp[i][j] typically represents the solution for substring s[i...j]
boolean[][] dp = new boolean[n][n]; // Or int[][] depending on the problem

// Base cases for single characters (length 1)
for (int i = 0; i < n; i++) {
    dp[i][i] = /* Base value for single character */;
}

// Base cases for two characters (length 2) - if needed
// for (int i = 0; i < n - 1; i++) {
//     // dp[i][i+1] logic
// }

for (int len = 2; len <= n; len++) { // Iterate through substring lengths
    for (int i = 0; i <= n - len; i++) { // Iterate through start indices
        int j = i + len - 1; // Calculate end index
        // Access characters s.charAt(i) and s.charAt(j)
        if (s.charAt(i) == s.charAt(j)) {
            /* Your code for matching characters */
            // Often depends on dp[i+1][j-1]
        } else {
            /* Your code for non-matching characters */
            // Often depends on dp[i+1][j] or dp[i][j-1]
        }
    }
}
// Return dp[0][n-1] or relevant final state
```

---

## The 4-Step Approach to Solving String DP Problems

This systematic approach helps break down complex DP problems into manageable steps.

### Step 1: Understand the Problem (The Hardest Part)

*   **Read Carefully:** Fully grasp what the problem is asking. What are the inputs? What is the desired output?
*   **Identify Operations/Constraints:** What actions can be performed? Are there any limitations (e.g., minimum operations, maximum length)?
*   **Examples:** Work through the provided examples manually to solidify your understanding. If no examples are given, create a simple one. This helps in visualizing the problem and verifying your logic later.

### Step 2: Pre-processing & Define Base Cases

*   **Tabulation:** DP problems are usually solved using tabulation (bottom-up approach) where results of smaller subproblems are stored in a table (usually a 2D array, `dp[][]`). Each cell `dp[i][j]` should represent the solution for a specific subproblem. Clearly define what `dp[i][j]` means in the context of your problem.
*   **Base Cases:** These are the smallest, simplest subproblems whose solutions are known without further computation. They form the foundation of your DP table.
    *   **Empty Strings:** What happens if one or both strings are empty?
    *   **Single Characters/Substrings:** What's the solution for a single character or a very short substring?
    *   **Initialization:** Populate the first row(s) and/or column(s) of your DP table based on these base cases.

### Step 3: Define the Recurrence Relation (Main Logic)

*   This is the core of your DP solution. It describes how to compute `dp[i][j]` using already computed values from smaller subproblems.
*   **Consider the Current Characters:** Focus on `s1.charAt(i-1)` and `s2.charAt(j-1)` (for two strings) or `s.charAt(i)` and `s.charAt(j)` (for a single string).
*   **Matching Characters:** If the current characters match, how does this simplify the problem? Often, the solution depends on the subproblem *without* these characters (e.g., `dp[i-1][j-1]` or `dp[i+1][j-1]`).
*   **Non-Matching Characters:** If the current characters don't match, what are the possible operations or choices? You'll typically take the `min` or `max` of the results from these choices.
    *   **Deletion/Insertion:** Consider removing a character from `s1` (`dp[i-1][j]`) or `s2` (`dp[i][j-1]`).
    *   **Replacement:** Consider changing one character to match the other (`dp[i-1][j-1]`).
    *   **Skipping:** For subsequence problems, you might skip one character to match the other.

### Step 4: Implement and Return the Result

*   **Translate to Code:** Write the Java code, following your defined DP table structure, base cases, and recurrence relation.
*   **Loop Order:** Ensure your loops iterate in an order that guarantees all dependencies for `dp[i][j]` are already computed. (e.g., `i-1, j-1` must be computed before `i, j`).
*   **Final Result:** The answer to the overall problem will usually be in a specific cell of your DP table (e.g., `dp[m][n]` or `dp[0][n-1]`).

---

## Problem Examples

Let's apply these steps to common string DP problems.

### Problem 1: Edit Distance

*   **Problem Statement:** Given two strings `word1` and `word2`, find the minimum number of operations required to convert `word1` to `word2`. You have three operations permitted on a word: insert a character, delete a character, or replace a character.

*   **Example:** `word1 = "horse"`, `word2 = "ros"` -> Output: `3`

*   **Step 1: Understand the Problem**
    *   We want the minimum operations.
    *   Operations: Insert, Delete, Replace.
    *   Goal: Transform `word1` into `word2`.

*   **Step 2: Pre-processing & Base Cases**
    *   `dp[i][j]` will represent the minimum operations to convert `word1[0...i-1]` to `word2[0...j-1]`.
    *   **`dp` table size:** $(m+1) \times (n+1)$ where $m$ is `word1.length()` and $n$ is `word2.length()`.
    *   **Base Case 1:** `dp[0][0] = 0`. (Empty string to empty string requires 0 operations).
    *   **Base Case 2:** If `word1` is empty ($i=0$), to convert it to `word2[0...j-1]`, we need $j$ insertions. So, `dp[0][j] = j`.
    *   **Base Case 3:** If `word2` is empty ($j=0$), to convert `word1[0...i-1]` to an empty string, we need $i$ deletions. So, `dp[i][0] = i`.

*   **Step 3: Define the Recurrence Relation**
    *   Iterate $i$ from $1$ to $m$, $j$ from $1$ to $n$.
    *   **If `word1.charAt(i-1) == word2.charAt(j-1)` (Matching characters):**
        *   No operation needed for these characters. The cost is the same as converting `word1[0...i-2]` to `word2[0...j-2]`.
        *   `dp[i][j] = dp[i-1][j-1]`
    *   **If `word1.charAt(i-1) != word2.charAt(j-1)` (Non-matching characters):**
        *   We need to perform one operation. We choose the minimum cost among the three possibilities:
            1.  **Insert:** Insert `word2.charAt(j-1)` into `word1`. This is equivalent to solving `dp[i][j-1]` (where `word1[0...i-1]` is matched against `word2[0...j-2]`) and then adding 1 for the insertion. `dp[i][j-1] + 1`
            2.  **Delete:** Delete `word1.charAt(i-1)`. This is equivalent to solving `dp[i-1][j]` (where `word1[0...i-2]` is matched against `word2[0...j-1]`) and then adding 1 for the deletion. `dp[i-1][j] + 1`
            3.  **Replace:** Replace `word1.charAt(i-1)` with `word2.charAt(j-1)`. This is equivalent to solving `dp[i-1][j-1]` (where both characters are now considered "matched" after replacement) and then adding 1 for the replacement. `dp[i-1][j-1] + 1`
        *   `dp[i][j] = \min(dp[i-1][j-1], dp[i][j-1], dp[i-1][j]) + 1`

*   **Step 4: Implement and Return**

```java
class Solution {
    public int minDistance(String word1, String word2) {
        int m = word1.length();
        int n = word2.length();

        // dp[i][j] stores the minimum operations to convert word1[0...i-1] to word2[0...j-1]
        int[][] dp = new int[m + 1][n + 1];

        // Base cases:
        // If word1 is empty, we need to insert all characters of word2
        for (int j = 0; j <= n; j++) {
            dp[0][j] = j;
        }
        // If word2 is empty, we need to delete all characters of word1
        for (int i = 0; i <= m; i++) {
            dp[i][0] = i;
        }

        // Fill the DP table
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                    // Characters match, no operation needed for current characters
                    dp[i][j] = dp[i - 1][j - 1];
                } else {
                    // Characters do not match, choose minimum of three operations + 1
                    int insert = dp[i][j - 1];   // Cost to insert word2.charAt(j-1)
                    int delete = dp[i - 1][j];   // Cost to delete word1.charAt(i-1)
                    int replace = dp[i - 1][j - 1]; // Cost to replace word1.charAt(i-1) with word2.charAt(j-1)
                    dp[i][j] = Math.min(insert, Math.min(delete, replace)) + 1;
                }
            }
        }

        // The result for converting word1 to word2 is at dp[m][n]
        return dp[m][n];
    }
}
```

### Problem 2: Delete Operations for Two Strings

*   **Problem Statement:** Given two strings `word1` and `word2`, return the minimum number of steps required to make `word1` and `word2` the same. In one step, you can delete exactly one character from either string.

*   **Quick Explanation:** This problem is equivalent to finding the Longest Common Subsequence (LCS) length. If LCS length is $L$, and string lengths are $m$ and $n$, then the minimum deletions needed are $(m - L) + (n - L)$. However, we can also solve it directly with DP.

*   **Step 1: Understand the Problem**
    *   Goal: Make `word1` and `word2` identical by deleting characters.
    *   Operation: Delete character from either string.
    *   Find minimum deletions.

*   **Step 2: Pre-processing & Base Cases**
    *   `dp[i][j]` will represent the minimum deletions required to make `word1[0...i-1]` and `word2[0...j-1]` equal.
    *   **`dp` table size:** $(m+1) \times (n+1)$.
    *   **Base Case 1:** `dp[0][0] = 0`. (Empty string to empty string requires 0 deletions).
    *   **Base Case 2:** If `word1` is empty ($i=0$), to make it equal to `word2[0...j-1]`, we must delete all $j$ characters from `word2`. So, `dp[0][j] = j$.
    *   **Base Case 3:** If `word2` is empty ($j=0$), to make it equal to `word1[0...i-1]`, we must delete all $i$ characters from `word1`. So, `dp[i][0] = i`.

*   **Step 3: Define the Recurrence Relation**
    *   Iterate $i$ from $1$ to $m$, $j$ from $1$ to $n$.
    *   **If `word1.charAt(i-1) == word2.charAt(j-1)` (Matching characters):**
        *   No deletion needed for these characters. The cost is the same as making `word1[0...i-2]` and `word2[0...j-2]` equal.
        *   `dp[i][j] = dp[i-1][j-1]`
    *   **If `word1.charAt(i-1) != word2.charAt(j-1)` (Non-matching characters):**
        *   We must delete one of the current characters. We choose the option that leads to minimum deletions:
            1.  Delete `word1.charAt(i-1)`: Cost is $1 + dp[i-1][j]$ (solve for `word1[0...i-2]` and `word2[0...j-1]`).
            2.  Delete `word2.charAt(j-1)`: Cost is $1 + dp[i][j-1]$ (solve for `word1[0...i-1]` and `word2[0...j-2]`).
        *   `dp[i][j] = \min(dp[i-1][j], dp[i][j-1]) + 1`

*   **Step 4: Implement and Return**

```java
class Solution {
    public int minDistance(String word1, String word2) {
        int m = word1.length();
        int n = word2.length();

        // dp[i][j] stores the minimum deletions to make word1[0...i-1] and word2[0...j-1] equal
        int[][] dp = new int[m + 1][n + 1];

        // Base cases:
        for (int i = 0; i <= m; i++) {
            dp[i][0] = i; // Delete all chars from word1 to match empty word2
        }
        for (int j = 0; j <= n; j++) {
            dp[0][j] = j; // Delete all chars from word2 to match empty word1
        }

        // Fill the DP table
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                    // Characters match, no deletion needed for current chars
                    dp[i][j] = dp[i - 1][j - 1];
                } else {
                    // Characters don't match, we must delete one of them
                    dp[i][j] = Math.min(dp[i - 1][j], dp[i][j - 1]) + 1;
                }
            }
        }
        return dp[m][n];
    }
}
```

### Problem 3: Minimum ASCII Delete Sum for Two Strings

*   **Problem Statement:** Given two strings `s1` and `s2`, return the lowest ASCII sum of deleted characters to make two strings equal.

*   **Similarity to Problem 2:** This is very similar to "Delete Operations for Two Strings". The key difference is that instead of counting $1$ for each deletion, we sum the ASCII values of the deleted characters.

*   **Step 1: Understand the Problem**
    *   Goal: Make `s1` and `s2` identical.
    *   Operation: Delete character.
    *   Find minimum *ASCII sum* of deleted characters.

*   **Step 2: Pre-processing & Base Cases**
    *   `dp[i][j]` will represent the minimum ASCII sum of deleted characters to make `s1[0...i-1]` and `s2[0...j-1]` equal.
    *   **`dp` table size:** $(m+1) \times (n+1)$.
    *   **Base Case 1:** `dp[0][0] = 0`.
    *   **Base Case 2:** If `s1` is empty ($i=0$), to make it equal to `s2[0...j-1]`, we must delete all characters from `s2`. The sum of ASCII values for `s2[0...j-1]` is needed. So, `dp[0][j] = dp[0][j-1] + s2.charAt(j-1)`.
    *   **Base Case 3:** If `s2` is empty ($j=0$), to make it equal to `s1[0...i-1]`, we must delete all characters from `s1`. The sum of ASCII values for `s1[0...i-1]` is needed. So, `dp[i][0] = dp[i-1][0] + s1.charAt(i-1)`.

*   **Step 3: Define the Recurrence Relation**
    *   Iterate $i$ from $1$ to $m$, $j$ from $1$ to $n$.
    *   **If `s1.charAt(i-1) == s2.charAt(j-1)` (Matching characters):**
        *   No deletion needed for these characters.
        *   `dp[i][j] = dp[i-1][j-1]`
    *   **If `s1.charAt(i-1) != s2.charAt(j-1)` (Non-matching characters):**
        *   We must delete one of the current characters. We add its ASCII value to the subproblem's cost.
            1.  Delete `s1.charAt(i-1)`: Cost is `s1.charAt(i-1) + dp[i-1][j]`.
            2.  Delete `s2.charAt(j-1)`: Cost is `s2.charAt(j-1) + dp[i][j-1]`.
        *   `dp[i][j] = \min(s1.charAt(i-1) + dp[i-1][j], s2.charAt(j-1) + dp[i][j-1])`

*   **Step 4: Implement and Return**

```java
class Solution {
    public int minimumDeleteSum(String s1, String s2) {
        int m = s1.length();
        int n = s2.length();

        // dp[i][j] stores the minimum ASCII sum of deleted characters
        // to make s1[0...i-1] and s2[0...j-1] equal.
        int[][] dp = new int[m + 1][n + 1];

        // Base cases:
        // If s1 is empty, delete all characters from s2
        for (int j = 1; j <= n; j++) {
            dp[0][j] = dp[0][j - 1] + s2.charAt(j - 1);
        }
        // If s2 is empty, delete all characters from s1
        for (int i = 1; i <= m; i++) {
            dp[i][0] = dp[i - 1][0] + s1.charAt(i - 1);
        }

        // Fill the DP table
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (s1.charAt(i - 1) == s2.charAt(j - 1)) {
                    // Characters match, no deletion cost for current chars
                    dp[i][j] = dp[i - 1][j - 1];
                } else {
                    // Characters don't match, delete one of them (add its ASCII value)
                    dp[i][j] = Math.min(
                        s1.charAt(i - 1) + dp[i - 1][j], // Delete s1.charAt(i-1)
                        s2.charAt(j - 1) + dp[i][j - 1]  // Delete s2.charAt(j-1)
                    );
                }
            }
        }
        return dp[m][n];
    }
}
```

### Problem 4: Longest Palindromic Subsequence

*   **Problem Statement:** Given a string `s`, find the length of the longest palindromic subsequence. A subsequence is a sequence that can be derived from another sequence by deleting some or no characters without changing the order of the remaining elements.

*   **Step 1: Understand the Problem**
    *   Goal: Find the *length* of the longest palindromic *subsequence*.
    *   Subsequence: Characters don't need to be contiguous.
    *   Palindrome: Reads the same forwards and backwards.

*   **Step 2: Pre-processing & Base Cases**
    *   This is a single-string problem, so we use the Interval DP template.
    *   `dp[i][j]` will represent the length of the longest palindromic subsequence within the substring `s[i...j]`.
    *   **`dp` table size:** $n \times n$ where $n$ is `s.length()`.
    *   **Base Case 1 (Length 1):** Every single character is a palindrome of length 1.
        *   `dp[i][i] = 1` for all $i$ from $0$ to $n-1$.

*   **Step 3: Define the Recurrence Relation**
    *   Iterate `len` from $2$ to $n$ (length of substring).
    *   Iterate $i$ from $0$ to $n - len$ (start index).
    *   $j = i + len - 1$ (end index).
    *   **If `s.charAt(i) == s.charAt(j)` (Outer characters match):**
        *   These two characters can form part of the palindrome. We add 2 to the length of the palindromic subsequence found in the inner substring `s[i+1...j-1]`.
        *   If `len == 2`, `s[i+1...j-1]` is empty, so `dp[i+1][j-1]` would be 0. This correctly gives $2$.
        *   `dp[i][j] = 2 + dp[i+1][j-1]`
    *   **If `s.charAt(i) != s.charAt(j)` (Non-matching characters):**
        *   We cannot include both `s.charAt(i)` and `s.charAt(j)` in the palindrome.
        *   We have two options:
            1.  Exclude `s.charAt(i)`: Find the LPS in `s[i+1...j]`. This is `dp[i+1][j]`.
            2.  Exclude `s.charAt(j)`: Find the LPS in `s[i...j-1]`. This is `dp[i][j-1]`.
        *   Take the maximum of these two options: `dp[i][j] = \max(dp[i+1][j], dp[i][j-1])`

*   **Step 4: Implement and Return**

```java
class Solution {
    public int longestPalindromeSubseq(String s) {
        int n = s.length();
        // dp[i][j] stores the length of the longest palindromic subsequence in s[i...j]
        int[][] dp = new int[n][n];

        // Base case: every single character is a palindrome of length 1
        for (int i = 0; i < n; i++) {
            dp[i][i] = 1;
        }

        // Fill the DP table for substrings of length 2 up to n
        // 'len' represents the current length of the substring
        for (int len = 2; len <= n; len++) {
            // 'i' represents the starting index of the substring
            for (int i = 0; i <= n - len; i++) {
                int j = i + len - 1; // 'j' represents the ending index of the substring

                if (s.charAt(i) == s.charAt(j)) {
                    // If outer characters match, they contribute 2 to the palindrome length
                    // plus the LPS of the inner substring s[i+1...j-1]
                    // If len == 2, then i+1 > j-1 (empty substring), dp[i+1][j-1] is 0
                    dp[i][j] = 2 + dp[i + 1][j - 1];
                } else {
                    // If outer characters don't match, we must exclude one of them
                    // Take the maximum LPS from excluding s.charAt(i) or s.charAt(j)
                    dp[i][j] = Math.max(dp[i + 1][j], dp[i][j - 1]);
                }
            }
        }

        // The result for the entire string s[0...n-1] is at dp[0][n-1]
        return dp[0][n - 1];
    }
}
```

### Problem 5: Palindromic Substrings

*   **Problem Statement:** Given a string `s`, return the number of palindromic substrings in it. A substring is a contiguous sequence of characters within a string.

*   **Step 1: Understand the Problem**
    *   Goal: Count *all* palindromic *substrings*.
    *   Substring: Must be contiguous.
    *   Palindrome: Reads the same forwards and backwards.

*   **Step 2: Pre-processing & Base Cases**
    *   This is also a single-string problem (Interval DP).
    *   `dp[i][j]` will be a boolean value: `true` if `s[i...j]` is a palindrome, `false` otherwise.
    *   **`dp` table size:** $n \times n$.
    *   **Base Case 1 (Length 1):** Every single character is a palindrome.
        *   `dp[i][i] = true` for all $i$ from $0$ to $n-1$. Initialize `count = n`.
    *   **Base Case 2 (Length 2):** Check adjacent characters.
        *   If `s.charAt(i) == s.charAt(i+1)`, then `s[i...i+1]` is a palindrome.
        *   `dp[i][i+1] = true`. Increment `count`.

*   **Step 3: Define the Recurrence Relation**
    *   Iterate `len` from $3$ to $n$.
    *   Iterate $i$ from $0$ to $n - len$.
    *   $j = i + len - 1$.
    *   **Condition for `s[i...j]` to be a palindrome:**
        1.  `s.charAt(i) == s.charAt(j)` (Outer characters must match).
        2.  The inner substring `s[i+1...j-1]` must also be a palindrome. This is `dp[i+1][j-1]`.
    *   `dp[i][j] = (s.charAt(i) == s.charAt(j) \land dp[i+1][j-1])`
    *   If `dp[i][j]` is `true`, increment `count`.

*   **Step 4: Implement and Return**

```java
class Solution {
    public int countSubstrings(String s) {
        int n = s.length();
        if (n == 0) {
            return 0;
        }

        // dp[i][j] is true if s[i...j] is a palindrome, false otherwise
        boolean[][] dp = new boolean[n][n];
        int count = 0; // To store the total number of palindromic substrings

        // Base cases:
        // All single characters are palindromes
        for (int i = 0; i < n; i++) {
            dp[i][i] = true;
            count++;
        }

        // Check for palindromes of length 2
        for (int i = 0; i < n - 1; i++) {
            if (s.charAt(i) == s.charAt(i + 1)) {
                dp[i][i + 1] = true;
                count++;
            }
        }

        // Check for palindromes of length 3 or more
        // 'len' represents the current length of the substring
        for (int len = 3; len <= n; len++) {
            // 'i' represents the starting index of the substring
            for (int i = 0; i <= n - len; i++) {
                int j = i + len - 1; // 'j' represents the ending index of the substring

                // A substring s[i...j] is a palindrome if:
                // 1. Its outer characters s.charAt(i) and s.charAt(j) match.
                // 2. The inner substring s[i+1...j-1] is also a palindrome.
                if (s.charAt(i) == s.charAt(j) && dp[i + 1][j - 1]) {
                    dp[i][j] = true;
                    count++;
                }
            }
        }
        return count;
    }
}
```

---

## Additional Tips

*   **Draw the DP Table:** For smaller examples, physically draw out the DP table and fill it cell by cell. This helps in understanding the dependencies and verifying your recurrence relation.
*   **Space Optimization:** After solving a problem, consider if the DP table can be optimized for space (e.g., if `dp[i][j]` only depends on the previous row, you might only need two rows instead of $m$ rows).
*   **Top-Down (Memoization) vs. Bottom-Up (Tabulation):** While this guide focuses on tabulation, understanding memoization (recursive DP with caching) can sometimes be more intuitive for deriving the recurrence relation. Practice both!
*   **Identify the State:** The most crucial step is defining what `dp[i]`, `dp[i][j]`, or `dp[i][j][k]` represents. This defines your subproblems.
*   **Look for Overlapping Subproblems and Optimal Substructure:** These are the two properties that indicate a problem can be solved with DP.

I hope this detailed guide helps you in mastering Dynamic Programming problems on strings! Feel free to upvote and bookmark for future reference. If you spot any errors or have suggestions, please let me know.
```
