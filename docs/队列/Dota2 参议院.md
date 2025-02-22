# Dota2 参议院

## [649. Dota2 参议院](https://leetcode.cn/problems/dota2-senate/)

> - **Question**
>   - `Dota2` 的世界里有两个阵营： `Radiant` （天辉）和 `Dire` （夜魇）
>   - `Dota2` 参议院由来自两派的参议员组成。现在参议院希望对一个 `Dota2` 游戏里的改变作出决定。他们以一个基于轮为过程的投票进行。在每一轮中，每一位参议员都可以行使两项权利中的一项：
>     - 禁止一名参议员的权利：参议员可以让另一位参议员在这一轮和随后的几轮中丧失所有的权利。
>     - 宣布胜利：如果参议员发现有权利投票的参议员都是同一个阵营的，他可以宣布胜利并决定在游戏中的有关变化。
>   - 给你一个字符串 `senate` 代表每个参议员的阵营。字母 `'R'` 和 `'D'` 分别代表了 `Radiant` （天辉）和 `Dire` （夜魇）。然后，如果有 `n` 个参议员，给定字符串的大小将是 `n` 。
>   - 以轮为基础的过程从给定顺序的第一个参议员开始到最后一个参议员结束。这一过程将持续到投票结束。所有失去权利的参议员将在过程中被跳过。
>   - 假设每一位参议员都足够聪明，会为自己的政党做出最好的策略，你需要预测哪一方最终会宣布胜利并在 `Dota2` 游戏中决定改变。输出应该是 `"Radiant"` 或 `"Dire"` 。
>   - **Tips**
>     - `n == senate.length`
>     - `1 <= n <= 10^4`
>     - `senate[i]` 为 `'R'` 或 `'D'`

## Java

> - **贪心 + 循环队列**
>   - ![image](./images/Dota2%20参议院.png)

```java
import java.util.*;

class Solution {

    public String predictPartyVictory(String senate) {
        int n = senate.length();
        ArrayDeque<Integer> radiant = new ArrayDeque<>();
        ArrayDeque<Integer> dire = new ArrayDeque<>();
        for (int i = 0; i < n; ++i) {
            if (senate.charAt(i) == 'R') {
                radiant.offer(i);
            } else {
                dire.offer(i);
            }
        }
        while (!radiant.isEmpty() && !dire.isEmpty()) {
            int radiantIndex = radiant.poll(), direIndex = dire.poll();
            if (radiantIndex < direIndex) {
                radiant.offer(radiantIndex + n);
            } else {
                dire.offer(direIndex + n);
            }
        }
        return !radiant.isEmpty() ? "Radiant" : "Dire";
    }

}
```
