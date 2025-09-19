---
title: hungstones
published: 2025-09-19
tags: [Editorial, HNOI]
category: Editorial
draft: false
---

# Hướng dẫn giải cho PreHNOI1 

## Bài 4: hungstones

### Ý tưởng thuật toán

Bài toán yêu cầu tìm số lần di chuyển đá tối thiểu để tất cả ```K``` viên đá nằm trên một đường đi đơn. Một viên đá có thể được di chuyển đến một đỉnh kề.

Tổng chi phí di chuyển là tổng các khoảng cách từ vị trí ban đầu của mỗi viên đá đến vị trí mới của nó trên đường đi đã chọn. Để tối thiểu hóa tổng chi phí này, với một đường đi đơn ```P``` cho trước, vị trí mới tối ưu cho mỗi viên đá ```A_i``` là đỉnh trên đường đi ```P``` gần nhất với ```A_i```. Do đó, chi phí cho một đường đi ```P``` là tổng các khoảng cách ```dist(A_i, P)``` với mọi ```i``` từ ```1``` đến ```K```.

Bài toán trở thành tìm một đường đi đơn ```P``` có hai đầu mút ```s``` và ```t``` sao cho ```Cost(s, t) = sum_{i=1 to K} dist(A_i, path(s, t))``` là nhỏ nhất.

Trong một cây, khoảng cách từ một đỉnh x đến đường đi đơn giữa s và t được tính bằng công thức: 

```
dist(x, path(s, t)) = (dist(x, s) + dist(x, t) - dist(s, t)) / 2.
```

Áp dụng công thức này, tổng chi phí là:
```
Cost(s, t) = (1/2) * [ sum(dist(A_i, s)) + sum(dist(A_i, t)) - K * dist(s, t) ]
```

Đặt ```F(x) = sum_{i=1 to K} dist(A_i, x)```
Ta cần tối thiểu hóa ```F(s) + F(t) - K * dist(s, t)```

Để tính toán dễ dàng hơn, ta cố định một gốc cho cây (ví dụ đỉnh 1). Khi đó 
```
dist(u, v) = depth[u] + depth[v] - 2 * depth[lca(u, v)].
```

Biểu thức cần tối thiểu hóa trở thành:
```
F(s) + F(t) - K * (depth[s] + depth[t] - 2 * depth[lca(s, t)])
```
```
= (F(s) - K*depth[s]) + (F(t) - K*depth[t]) + 2*K*depth[lca(s, t)]
```

Đặt 
```
G(x) = F(x) - K*depth[x]
```

Ta cần tìm 
```min_{s, t} (G(s) + G(t) + 2*K*depth[lca(s, t)])```

Ta có thể giải quyết bài toán này bằng Quy hoạch động trên cây hoặc duyệt theo thứ tự sau (post-order traversal):

### Tiền xử lý: 
Dùng DFS từ gốc 1 để tính depth[u] (độ sâu của đỉnh u) và stones_in_subtree[u] (số lượng viên đá trong cây con gốc u).

### Tính F(x): 
Tính ```F(1) = sum(depth[A_i])```. Sau đó, dùng một lượt DFS thứ hai để tính ```F(u)``` cho mọi u dựa trên ```F``` của đỉnh cha: 
```
F(v) = F(u) - stones_in_subtree[v] + (K - stones_in_subtree[v])
```

### Tính G(x): 
Dễ dàng tính ```G(u) = F(u) - K*depth[u]``` cho mọi u.

### Tìm giá trị nhỏ nhất: 
Dùng một lượt DFS thứ ba (duyệt post-order). Với mỗi đỉnh ```u```, ta xem nó như là ```lca(s, t)```. Hai đỉnh ```s``` và ```t``` phải nằm ở các nhánh con khác nhau của ```u``` (hoặc một trong hai đỉnh là chính ```u```).

Tại mỗi đỉnh ```u```, ta tính ```minG_subtree[u]``` là giá trị ```G(x)``` nhỏ nhất trong cây con gốc ```u```.

Để tìm ```s``` và ```t``` tối ưu cho lca là ```u```, ta cần chọn ra hai giá trị nhỏ nhất từ tập ```{G(u), minG_subtree[c_1], minG_subtree[c_2], ...}``` với ```c_i``` là các con của ```u```.

Gọi hai giá trị nhỏ nhất này là m1 và m2. Ta cập nhật kết quả toàn cục với ```min_cost = min(min_cost, m1 + m2 + 2*K*depth[u])```.

Kết quả cuối cùng của bài toán sẽ là ```min_cost / 2```.

```markdown
---
title: hungstones
published: 2025-09-19
tags: [Editorial, HNOI]
category: Editorial
draft: false
---
