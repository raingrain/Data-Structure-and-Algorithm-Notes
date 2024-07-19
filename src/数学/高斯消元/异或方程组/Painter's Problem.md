# Painter's Problem

## [Painter's Problem](http://poj.org/problem?id=1681)

> - ***Question***
>   - 有一个 `n * n` 的二维网格，每个网格给定初始颜色，不是黄色就是白色，有一个神奇的刷子，一旦在某个网格处刷一下，那么该网格连同上下左右的网格，都会翻转颜色，黄变白、白变黄，现在想知道，如果想让最终所有网格都变成黄色，一定需要被刷的网格有几个，注意，答案是说，可能有很多方案都能做到所有网格变成黄色，但不管是哪一种方案，可能都有一些网格一定需要被刷，打印这个数量，如果所有方案都做不到，打印 `inf` 。
>   - ***tips:***
>     - `1 <= n <= 15`

---

## *Java*

> - ***高斯消元解决异或方程组***

```java
import java.io.*;
import java.util.StringTokenizer;

public class Main {

    public static int MAXS = 230;

    public static int[][] mat = new int[MAXS][MAXS];

    public static int[] dir = {0, 0, -1, 0, 1, 0};

    public static int n, m, s;

    public static void prepare() {
        for (int i = 1; i <= s; i++) {
            for (int j = 1; j <= s + 1; j++) {
                mat[i][j] = 0;
            }
        }
        int cur, row, col;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                cur = i * m + j + 1;
                for (int d = 0; d <= 4; d++) {
                    row = i + dir[d];
                    col = j + dir[d + 1];
                    if (row >= 0 && row < n && col >= 0 && col < m) {
                        mat[cur][row * m + col + 1] = 1;
                    }
                }
            }
        }
    }

    // 高斯消元解决异或方程组模版
    public static void gauss(int n) {
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= n; j++) {
                if (j < i && mat[j][j] == 1) {
                    continue;
                }
                if (mat[j][i] == 1) {
                    swap(i, j);
                    break;
                }
            }
            if (mat[i][i] == 1) {
                for (int j = 1; j <= n; j++) {
                    if (i != j && mat[j][i] == 1) {
                        for (int k = i; k <= n + 1; k++) {
                            mat[j][k] ^= mat[i][k];
                        }
                    }
                }
            }
        }
    }

    public static void swap(int a, int b) {
        int[] tmp = mat[a];
        mat[a] = mat[b];
        mat[b] = tmp;
    }

    public static boolean check(int i) {
        if (mat[i][i] != 1 || mat[i][s + 1] != 1) {
            return false;
        }
        for (int j = i + 1; j <= s; j++) {
            if (mat[i][j] != 0) {
                return false;
            }
        }
        return true;
    }

    public static void main(String[] args) throws IOException {
        Kattio io = new Kattio();
        int test = io.nextInt();
        char[] line;
        for (int t = 1; t <= test; t++) {
            n = io.nextInt();
            m = n;
            s = n * m;
            prepare();
            for (int i = 0, id = 1; i < n; i++) {
                line = io.next().toCharArray();
                for (int j = 0; j < m; j++, id++) {
                    if (line[j] == 'y') {
                        mat[id][s + 1] = 0;
                    } else {
                        mat[id][s + 1] = 1;
                    }
                }
            }
            gauss(s);
            int sign = 1;
            for (int i = 1; i <= s; i++) {
                if (mat[i][i] == 0 && mat[i][s + 1] == 1) {
                    sign = -1;
                    break;
                }
            }
            if (sign == -1) {
                io.println("inf");
            } else {
                int ans = 0;
                for (int i = 1; i <= s; i++) {
                    if (check(i)) {
                        ans++;
                    }
                }
                io.println(ans);
            }
        }
        io.flush();
        io.close();
    }

    // Kattio类IO效率很好，但还是不如StreamTokenizer
    // 只有StreamTokenizer无法正确处理时，才考虑使用这个类
    // 参考链接 : https://oi-wiki.org/lang/java-pro/
    public static class Kattio extends PrintWriter {

        private BufferedReader r;
        private StringTokenizer st;

        public Kattio() {
            this(System.in, System.out);
        }

        public Kattio(InputStream i, OutputStream o) {
            super(o);
            r = new BufferedReader(new InputStreamReader(i));
        }

        public Kattio(String intput, String output) throws IOException {
            super(output);
            r = new BufferedReader(new FileReader(intput));
        }

        public String next() {
            try {
                while (st == null || !st.hasMoreTokens())
                    st = new StringTokenizer(r.readLine());
                return st.nextToken();
            } catch (Exception e) {
            }
            return null;
        }

        public int nextInt() {
            return Integer.parseInt(next());
        }

        public double nextDouble() {
            return Double.parseDouble(next());
        }

        public long nextLong() {
            return Long.parseLong(next());
        }

    }

}
```

---

> ***last change: 2024/7/19***

---
