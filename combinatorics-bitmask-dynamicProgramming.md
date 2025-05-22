There are 100 different types of caps each having a unique id from 1 to 100. also , there are 'n' persons each having a collection of a variable number of caps. One day all of these persons decide to go in a party wearning a cap but to look unique they decide that none of them will wear the same type of cap. so count the total number of arrangements or ways such that none of them is wearing the same type of cap.

This is a classic **combinatorics + bitmask + dynamic programming** problem. You are asked to compute the total number of valid assignments of **caps to people** such that:

* Each person wears **only one cap**.
* No two people wear the **same cap**.
* Each person has their own **list of cap preferences**.

---

### 🧠 Key Idea:

We solve this by:

* Representing each **state** by a bitmask of which persons have been assigned caps.
* Iterating cap by cap (1 to 100), and for each cap, trying to assign it to any person who wants it and hasn't got one yet.

---

### ✅ Java Implementation

```java
import java.util.*;

public class UniqueCapAssignments {
    static final int MOD = 1_000_000_007;
    static int[][] dp = new int[101][1 << 11]; // max 10 persons -> 2^10 possible masks
    static List<Integer>[] capList = new ArrayList[101];

    static int countWaysUtil(int cap, int mask, int allMask, int n) {
        if (mask == allMask) return 1; // all persons have caps
        if (cap > 100) return 0; // no more caps to assign
        if (dp[cap][mask] != -1) return dp[cap][mask];

        // Skip this cap
        long ways = countWaysUtil(cap + 1, mask, allMask, n);

        // Try assigning cap to anyone who wants it and is not assigned
        for (int person : capList[cap]) {
            if ((mask & (1 << person)) == 0) {
                ways += countWaysUtil(cap + 1, mask | (1 << person), allMask, n);
                ways %= MOD;
            }
        }

        return dp[cap][mask] = (int) ways;
    }

    static int countWays(List<List<Integer>> personCaps, int n) {
        // Initialize capList
        for (int i = 0; i <= 100; i++) {
            capList[i] = new ArrayList<>();
        }

        for (int person = 0; person < n; person++) {
            for (int cap : personCaps.get(person)) {
                capList[cap].add(person);
            }
        }

        for (int[] row : dp) Arrays.fill(row, -1);
        int allMask = (1 << n) - 1; // all persons assigned

        return countWaysUtil(1, 0, allMask, n);
    }

    public static void main(String[] args) {
        // Example: 3 persons
        List<List<Integer>> personCaps = new ArrayList<>();
        personCaps.add(Arrays.asList(5, 100, 1));
        personCaps.add(Arrays.asList(2, 100, 3));
        personCaps.add(Arrays.asList(5, 3, 4));

        int ways = countWays(personCaps, personCaps.size());
        System.out.println("Total ways to assign caps uniquely: " + ways);
    }
}
```

---

### 📌 Notes:

* This works efficiently up to **n = 10 persons** due to `2^n` states.
* `capList[i]` contains all persons who like cap `i`.
* `dp[cap][mask]` stores number of ways to assign caps starting from `cap` given current assignment state `mask`.



This exact problem **does not appear directly on LeetCode** under a specific problem number, but it is a **classic variation** of problems involving:

* **Bitmask Dynamic Programming (Bitmask DP)**
* **Assignment problems / Matching**
* **Combinatorics over subsets**

---

### 💡 Related problems on LeetCode:

Here are some similar LeetCode problems that touch on related concepts:

| LeetCode #                                                             | Title                            | Relevance                                                         |
| ---------------------------------------------------------------------- | -------------------------------- | ----------------------------------------------------------------- |
| [698](https://leetcode.com/problems/partition-to-k-equal-sum-subsets/) | Partition to K Equal Sum Subsets | Uses Bitmask DP over subsets                                      |
| [847](https://leetcode.com/problems/shortest-path-visiting-all-nodes/) | Shortest Path Visiting All Nodes | Bitmask DP over graph traversal                                   |
| [1014](https://leetcode.com/problems/best-sightseeing-pair/)           | Best Sightseeing Pair            | Basic DP logic                                                    |
| [526](https://leetcode.com/problems/beautiful-arrangement/)            | Beautiful Arrangement            | Recursive + Backtracking with permutations                        |
| [691](https://leetcode.com/problems/stickers-to-spell-word/)           | Stickers to Spell Word           | Bitmask DP with strings                                           |
| [1125](https://leetcode.com/problems/smallest-sufficient-team/)        | Smallest Sufficient Team         | Bitmask DP to cover all required people (inverse of your problem) |

---

### 📘 Competitive Programming Reference

Your exact problem (unique cap assignments for people) **is a known problem from**:

* **TopCoder**
* **SPOJ** — \[CAPCITY or similar]
* **GeeksForGeeks** article titled:
  🔗 [Count ways to assign unique cap to every person](https://www.geeksforgeeks.org/count-ways-assign-cap-every-person-unique/)

---

If you're looking to practice similar logic on LeetCode, I’d recommend starting with problems like **LeetCode 526**, **698**, and **1125**, all of which involve permutations or subset coverage using bitmasks.

Let me know if you’d like a walkthrough or solution for one of these related LeetCode problems!

