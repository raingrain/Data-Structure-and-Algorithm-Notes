# [数组中的较小累加和]()

### 题目描述
1. 给你一个数组arr
2. 请你将数组中的数分成两部分，使这两部分的和最接近并返回和较小的那一个
3. 对这两部分的长度没有硬性要求，大于1即可

---

### 解题思路
1. 从左往右的尝试模型

---

### 代码

```java
// java暴力递归转动态规划
public class Solution {

    public static int ByRecursion(int[] arr) {
        if (arr == null || arr.length < 2) {
            return 0;
        } else {
            // 整个数组的累加和
            int sum = 0;
            for (int num : arr) {
                sum += num;
            }
            // 由简单的数学逻辑推断，这个累加和一定不会超过sum/2
            // 注意没有负数
            return recursion(arr, 0, sum / 2);
        }
    }

    // 在arr[index……]上自由选择，请返回累加和尽量接近rest，但不能超过rest的，最接近rest的
    // 从左往右的尝试模型
    public static int recursion(int[] arr, int index, int rest) {
        if (index == arr.length) {
            return 0;
        } else {
            // 不选arr[index]
            int p1 = recursion(arr, index + 1, rest);
            int p2 = 0;
            // 选arr[index]，但要保证
            if (arr[index] <= rest) {
                p2 = arr[index] + recursion(arr, index + 1, rest - arr[index]);
            }
            return Math.max(p1, p2);
        }
    }

    public static int ByDP1(int[] arr) {
        int sum = 0;
        for (int num : arr) {
            sum += num;
        }
        int[][] dp = new int[arr.length + 1][sum / 2 + 1];
        for (int index = arr.length - 1; index >= 0; index--) {
            for (int rest = 0; rest <= (sum / 2); rest++) {
                int p1 = recursion(arr, index + 1, rest);
                int p2 = 0;
                if (arr[index] <= rest) {
                    p2 = arr[index] + dp[index + 1][rest - arr[index]];
                }
                dp[index][rest] = Math.max(p1, p2);
            }
        }
        return dp[0][sum / 2];
    }

    public static void main(String[] args) {
        int[] arr = new int[]{33, 24, 35, 30, 27, 26};
        System.out.println(ByRecursion(arr));
        System.out.println(ByDP1(arr));
    }
}
```
