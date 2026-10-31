# Design and Analysis of Algorithms — Dynamic Programming

This repository houses verified C language solutions using the **Dynamic Programming (DP)** paradigm. Each solution breaks down multi-stage decision problems into optimal subproblems, leveraging techniques such as tabulating iterative states, using memoization arrays, or saving space with sliding dimensions to significantly lower computational overhead.

---

## 🚀 Problems Overview & Complexity Matrix

| Problem ID | Problem Description | Strategy / Approach | Time Complexity | Space Complexity |
| :--- | :--- | :--- | :--- | :--- |
| **Question 1** | Playing with Numbers | 1D Tabulation Linear DP | $O(n)$ | $O(n)$ |
| **Question 2** | Playing with Chessboard | 2D Grid Grid Traversal DP | $O(n^2)$ | $O(n^2)$ |
| **Question 3** | Longest Common Subsequence | 2-Row Optimized Matrix (LCS) | $O(n \times m)$ | $O(m)$ |
| **Question 4** | Longest Non-decreasing Subsequence | Binary Search Tails Tracking (LIS Variation) | $O(n \log n)$ | $O(n)$ |

---

## 🛠️ Detailed Implementations

### Question 1: Playing with Numbers
Given a target integer $n$, find the number of unique combinations that sum up to $n$ using only the values `1` and `3`. The state transition relies on the relation:  
$$\text{dp}[i] = \text{dp}[i-1] + \text{dp}[i-3]$$

```c
#include <stdio.h>

int main() {
    int n;
    scanf("%d", &n);

    long long dp[n + 1];
    dp[0] = 1;

    for (int i = 1; i <= n; i++) {
        dp[i] = dp[i - 1];
        if (i >= 3) dp[i] += dp[i - 3];
    }

    printf("%lld\n", dp[n]);
    return 0;
}
```

### Question 2: Playing with Chessboard
Given an $n \times n$ matrix of cell monetary gains, find a trajectory route running from the top-left coordinate $(0,0)$ out to the bottom-right terminal point $(n-1, n-1)$. Movement rules are restricted to traversing only down or right.

```c
#include <stdio.h>

int main() {
    int n;
    scanf("%d", &n);

    int a[n][n];
    long long dp[n][n];

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            scanf("%d", &a[i][j]);
        }
    }

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            if (i == 0 && j == 0) {
                dp[i][j] = a[i][j];
            } else if (i == 0) {
                dp[i][j] = dp[i][j - 1] + a[i][j];
            } else if (j == 0) {
                dp[i][j] = dp[i - 1][j] + a[i][j];
            } else {
                long long best = dp[i - 1][j] > dp[i][j - 1] ? dp[i - 1][j] : dp[i][j - 1];
                dp[i][j] = best + a[i][j];
            }
        }
    }

    printf("%lld\n", dp[n - 1][n - 1]);
    return 0;
}
```

### Question 3: Longest Common Subsequence (LCS)
Computes the length of the matching longest character sequence between two strings where letters do not need to sit continuously. Space complexity is optimized down to $O(m)$ by storing only two alternate matrix rows.

```c
#include <stdio.h>
#include <string.h>

int main() {
    static char s1[10001], s2[10001];
    scanf("%10000s %10000s", s1, s2);

    int n = strlen(s1), m = strlen(s2);
    static int dp[2][10001];

    for (int i = 1; i <= n; i++) {
        int cur = i & 1, prev = cur ^ 1;
        for (int j = 1; j <= m; j++) {
            if (s1[i - 1] == s2[j - 1])
                dp[cur][j] = dp[prev][j - 1] + 1;
            else
                dp[cur][j] = dp[prev][j] > dp[cur][j - 1] ? dp[prev][j] : dp[cur][j - 1];
        }
    }

    printf("%d\n", dp[n & 1][m]);
    return 0;
}
```

### Question 4: Longest Non-decreasing Subsequence
Calculates the sequence length of elements that do not decrease across an array sequence. It solves this in an optimized $O(n \log n)$ runtime bracket using tail elements tracking paired with internal binary searches.

```c
#include <stdio.h>

int main() {
    int n;
    scanf("%d", &n);

    int a[n], tails[n], len = 0;

    for (int i = 0; i < n; i++) {
        scanf("%d", &a[i]);

        int lo = 0, hi = len;
        while (lo < hi) {
            int mid = (lo + hi) / 2;
            if (tails[mid] <= a[i]) 
                lo = mid + 1;
            else 
                hi = mid;
        }
        tails[lo] = a[i];
        if (lo == len) len++;
    }

    printf("%d\n", len);
    return 0;
}
```

---

## 📋 Compilation & Execution

Compile any target problem with standard code flags via the local Terminal window:

```bash
# Compile code file using gcc
gcc question3.c -o dynamic_runner

# Execute program with mock test parameters
./dynamic_runner
```
