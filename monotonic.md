### **Understanding Monotonic Sequences**
A **monotonic sequence** is a sequence that is either **non-decreasing** (increasing) or **non-increasing** (decreasing).

1. **Strictly Increasing**: \( a_1 < a_2 < a_3 < ... < a_n \)  
2. **Strictly Decreasing**: \( a_1 > a_2 > a_3 > ... > a_n \)  
3. **Non-decreasing**: \( a_1 \leq a_2 \leq a_3 \leq ... \leq a_n \)  
4. **Non-increasing**: \( a_1 \geq a_2 \geq a_3 \geq ... \geq a_n \)  

**Monotonic subarrays** refer to contiguous subarrays that follow any of the above conditions.

---

## **Common Variations of Monotonic Sequence Problems**
There are multiple types of problems based on monotonic sequences, each requiring a different approach.

---

### **1. Finding the Longest Monotonic Subarray**
**Problem:** Given an array, find the length of the longest contiguous increasing or decreasing subarray.

#### **Approach:**
- Use **two counters** (`inc` and `dec`) while traversing the array.
- Reset the counter if the condition breaks.

**Optimized Code (O(n)):**
```java
class Solution {
    public int longestMonotonicSubarray(int[] nums) {
        int n = nums.length, maxLen = 1, inc = 1, dec = 1;
        
        for (int i = 1; i < n; i++) {
            if (nums[i] > nums[i - 1]) {
                inc++;
                dec = 1;  // Reset decreasing counter
            } else if (nums[i] < nums[i - 1]) {
                dec++;
                inc = 1;  // Reset increasing counter
            } else {
                inc = dec = 1;
            }

            maxLen = Math.max(maxLen, Math.max(inc, dec));
        }
        
        return maxLen;
    }
}
```
✅ **Time Complexity:** \(O(n)\)  
✅ **Space Complexity:** \(O(1)\)

---

### **2. Checking if an Array is Monotonic**
**Problem:** Determine whether the entire array is either increasing or decreasing.

#### **Approach:**
- Use two flags: `increasing` and `decreasing`.
- Check each pair of adjacent elements.

**Optimized Code (O(n)):**
```java
class Solution {
    public boolean isMonotonic(int[] nums) {
        boolean inc = true, dec = true;

        for (int i = 1; i < nums.length; i++) {
            if (nums[i] > nums[i - 1]) dec = false;  
            if (nums[i] < nums[i - 1]) inc = false;  
        }
        
        return inc || dec;
    }
}
```
✅ **Time Complexity:** \(O(n)\)  
✅ **Space Complexity:** \(O(1)\)

---

### **3. Longest Monotonic Subsequence (LIS & LDS)**
**Problem:** Find the length of the **longest increasing** (or decreasing) **subsequence** (not necessarily contiguous).

#### **Approach (Dynamic Programming - O(n²))**:
- Maintain a `dp[i]` where `dp[i]` is the length of the longest increasing subsequence ending at index `i`.
- Update `dp[i]` by checking all previous elements.

**Optimized Code (LIS using Binary Search - O(n log n)):**
```java
import java.util.*;

class Solution {
    public int lengthOfLIS(int[] nums) {
        ArrayList<Integer> lis = new ArrayList<>();

        for (int num : nums) {
            int idx = Collections.binarySearch(lis, num);
            if (idx < 0) idx = -idx - 1;

            if (idx == lis.size()) lis.add(num);
            else lis.set(idx, num);
        }

        return lis.size();
    }
}
```
✅ **Time Complexity:** \(O(n \log n)\) using binary search  
✅ **Space Complexity:** \(O(n)\)

👉 **Variation**: The **longest decreasing subsequence (LDS)** can be found by reversing the array.

---

### **4. Finding All Monotonic Subarrays**
**Problem:** Count the number of increasing or decreasing subarrays.

#### **Approach:**
- Maintain a counter that increases as long as the sequence is monotonic.
- Use **prefix sum concept** to count valid subarrays.

**Optimized Code (O(n)):**
```java
class Solution {
    public long countMonotonicSubarrays(int[] nums) {
        long count = 0;
        int inc = 1, dec = 1;
        
        for (int i = 1; i < nums.length; i++) {
            if (nums[i] > nums[i - 1]) {
                inc++;
                dec = 1;
            } else if (nums[i] < nums[i - 1]) {
                dec++;
                inc = 1;
            } else {
                inc = 1;
                dec = 1;
            }
            
            count += Math.max(inc, dec); // Count subarrays ending at `i`
        }
        
        return count;
    }
}
```
✅ **Time Complexity:** \(O(n)\)  
✅ **Space Complexity:** \(O(1)\)

---

### **5. Longest Bitonic Subarray**
A **bitonic subarray** first increases, then decreases.

#### **Approach (Two Pass O(n)):**
1. Compute **LIS left to right**.
2. Compute **LDS right to left**.
3. The result for each index \( i \) is `LIS[i] + LDS[i] - 1`.

```java
class Solution {
    public int longestBitonicSubarray(int[] nums) {
        int n = nums.length;
        int[] inc = new int[n];
        int[] dec = new int[n];

        Arrays.fill(inc, 1);
        Arrays.fill(dec, 1);

        for (int i = 1; i < n; i++) 
            if (nums[i] > nums[i - 1]) inc[i] = inc[i - 1] + 1;

        for (int i = n - 2; i >= 0; i--) 
            if (nums[i] > nums[i + 1]) dec[i] = dec[i + 1] + 1;

        int maxLen = 1;
        for (int i = 0; i < n; i++)
            maxLen = Math.max(maxLen, inc[i] + dec[i] - 1);

        return maxLen;
    }
}
```
✅ **Time Complexity:** \(O(n)\)  
✅ **Space Complexity:** \(O(n)\)

---

### **Summary of Different Monotonic Sequence Problems**
| Problem | Approach | Time Complexity | Space Complexity |
|---------|---------|----------------|-----------------|
| Longest Monotonic Subarray | Two counters (inc, dec) | \(O(n)\) | \(O(1)\) |
| Check if Array is Monotonic | Boolean flags | \(O(n)\) | \(O(1)\) |
| Longest Increasing Subsequence (LIS) | DP / Binary Search | \(O(n \log n)\) | \(O(n)\) |
| Count All Monotonic Subarrays | Prefix-like counting | \(O(n)\) | \(O(1)\) |
| Longest Bitonic Subarray | LIS + LDS | \(O(n)\) | \(O(n)\) |

---

### **Final Thoughts**
- **For contiguous monotonic subarrays:** Use **greedy two-pointer or counters**.
- **For subsequences (not contiguous):** Use **dynamic programming or binary search**.
- **For counting monotonic subarrays:** Use a **prefix-like approach**.
- **For bitonic sequences:** Combine **LIS & LDS**.

Would you like a **specific variation implemented** or an explanation of another approach? 🚀
