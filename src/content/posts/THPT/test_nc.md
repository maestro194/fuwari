---
title: Test nâng cao
published: 2026-01-13
description: Hướng dẫn giải bài tập trên test nâng cao
tags: [Editorial]
category: THPT
draft: false
---

# **R - Chia kẹo**

### Subtask 1 (20%): $N, Q \le 10^3$

Với mỗi truy vấn, ta duyệt $i$ từ $l$ đến $r$ và thêm $x$ vào số kẹo của học sinh $i$ (gọi là `b[i]`) nếu `a[i]` chia hết cho $x$.

Độ phức tạp: $O(N * Q)$

### Subtask 2 (20%): $x = 1$

Lúc này tất cả học sinh có số thứ tự từ $l$ đến $r$ đều nhận được kẹo, do đó ta sử dụng mảng hiệu và cập nhật `b[l] += v, b[r + 1] -= v` trong mỗi bước sau đó cộng dồn lại để được kết quả.

Độ phức tạp: $O(N + Q)$

### Subtask 3 (20%): $x \le 2$

Với $x = 1$, ta xử lý như Subtask 2

Với $x = 2$, ta lưu thêm một mảng `c` là số kẹo của các học sinh có số thứ tự chẵn và cập nhật `c[l] += v, c[r + 1] -= v` tương tự như trên. Sau đó ta cộng dồn mảng `c` và thêm `c[i]` vào `b[i]` nếu $i$ chẵn.

Độ phức tạp: $O(N + Q)$

### Subtask 4 (20%): $l = 1, r = N$

Vì $l = 1, r = N$ nên trong mỗi truy vấn, tất cả các học sinh có $a_i$ chia hết cho $x$ đều sẽ nhận được $v$ cái kẹo.

Gọi `f[i]` là tổng số kẹo của các truy vấn có $x = i$, để tìm $f$ ta chỉ cần duyệt các truy vấn và cập nhật `f[x] += v`

Để tìm số kẹo của mỗi học sinh, với học sinh $i$ ta duyệt các ước $j$ của $a_i$ và thêm `f[j]` vào kết quả. Nhưng cần chú ý $N \le 2 * 10^5, a_i \le 5 * 10^5$ nên nếu ta duyệt từ $1$ đến $\sqrt{a_i}$ thì sẽ không chạy kịp trong thời gian cho phép. Thay vào đó, ta xây dựng sẵn dãy ước của các số từ $1$ đến $5 * 10^5$ bằng cách duyệt $i$ từ $1$ đến $5 * 10^5$ sau đó thêm $i$ vào dãy ước của các bội của $i$.

Độ phức tạp $O(N * max\{U(a_i)\} + Q)$

<details>
<summary>Code tham khảo:</summary>

```cpp
#include<bits/stdc++.h>
#define el '\n'
using namespace std;

const int MN = 2e5 + 5;
const int N = 5e5 + 10;

int64_t ans[MN] , f[N];
int a[MN];
bool exist[N];
vector<int> divi[N];

int32_t main (){
    ios_base::sync_with_stdio(0);
    cin.tie(0);
    int n , q;
    cin >> n >> q;
    assert(n <= (int)2e5);
    for ( int i = 1 ; i <= n ; i++ ){
        cin >> a[i];
        exist[a[i]] = true;
    }
    int mx = *max_element(a + 1 , a + n + 1);
    for ( int64_t i = 1 ; i <= mx ; i++ ){
        for ( int64_t j = i ; j <= mx ; j += i ){
            if (exist[j]) divi[j].push_back(i);
        }
    }
    for ( int i = 1 ; i <= q ; i++ ){
        int l , r , x , y;
        cin >> l >> r >> x >> y;
        f[x] += y;
    }
    for ( int i = 1 ; i <= n ; i++ ){
        for ( int j = 0 ; j < divi[a[i]].size() ; j++ ){
            ans[i] += f[divi[a[i]][j]];
        }
    }
    for ( int i = 1 ; i <= n ; i++ ) cout << ans[i] << " ";
}
```
</details>

### Subtask 5 (20%): Không có ràng buộc gì thêm

Ta cải tiến từ Subtask 4 và sử dụng một kỹ thuật tương tự mảng hiệu

Vì lúc này không còn ràng buộc $l = 1, r = N$ nên mảng $f$ không còn giống nhau với tất cả học sinh, nhưng nếu ta tính mảng $f$ lần lươt cho các học sinh từ $1$ đến $N$ thì với mỗi truy vấn $l, r, x, v$, `f[x]` được cộng thêm $v$ với tất cả học sinh từ $l$ đến $r$, nên ta chỉ cần cập nhật `f[x] += v` khi xử lý đến học sinh $l$ và cập nhật `f[x] -= v` khi xử lý đến học sinh $r + 1$.

Từ ý tưởng trên, đầu tiên với mỗi truy vấn $l, r, x, v$ ta chia thành hai truy vấn: `f[x] += v` được lưu vào danh sách các truy vấn tại $l$ và `f[x] -= v` được lưu vào danh sách các truy vấn tại $r + 1$. Sau đó, ta duyệt $i$ từ $1$ đến $N$ và lấy ra các truy vấn đã được lưu từ trước để cập nhật mảng $f$, sau đó duyệt các ước của học sinh $i$ để tìm số kẹo của học sinh $i$ tương tự như Subtask 4.

Độ phức tạp: $O(N * max\{U(a_i)\} + Q)$

<details>
<summary>Code tham khảo:</summary>

```cpp
#include<bits/stdc++.h>
#define el '\n'
using namespace std;

const int MN = 2e5 + 5;
const int N = 5e5 + 10;

int64_t ans[MN] , f[N];
int a[MN];
bool exist[N];
vector<pair<int , int>> queries[MN];
vector<int> divi[N];

int32_t main (){
    ios_base::sync_with_stdio(0);
    cin.tie(0);
    int n , q;
    cin >> n >> q;
    assert(n <= (int)2e5);
    for ( int i = 1 ; i <= n ; i++ ){
        cin >> a[i];
        exist[a[i]] = true;
    }
    int mx = *max_element(a + 1 , a + n + 1);
    for ( int64_t i = 1 ; i <= mx ; i++ ){
        for ( int64_t j = i ; j <= mx ; j += i ){
            if (exist[j]) divi[j].push_back(i);
        }
    }
    for ( int i = 1 ; i <= q ; i++ ){
        int l , r , x , y;
        cin >> l >> r >> x >> y;
        queries[l].push_back(make_pair(x , y));
        queries[r + 1].push_back(make_pair(x , -y));
    }
    for ( int i = 1 ; i <= n ; i++ ){
        for ( int j = 0 ; j < queries[i].size() ; j++ ){
            f[queries[i][j].first] += queries[i][j].second;
        }
        for ( int j = 0 ; j < divi[a[i]].size() ; j++ ){
            ans[i] += f[divi[a[i]][j]];
        }
    }
    for ( int i = 1 ; i <= n ; i++ ) cout << ans[i] << " ";
}
```
</details>