---
title: 8I0 - Quay lui
published: 2025-10-01
description: Hướng dẫn giải bài tập 8I0 - Quay lui ngày 01-10-2025
tags: [Editorial]
category: 8I0
draft: false
---

# **Bài 1: Chia bò sữa**

Với giới hạn ```N <= 20``` là một giới hạn thấp, ta có thể thực hiện duyệt tất cả các trường hợp của bài toán như sau

Với con bò thứ ```i```, ta có 2 cách chọn cho con bò này: máy 1 hoặc máy 2. Như vậy với ```N``` con bò, ta sẽ có nhiều nhất là $$2^{20} = 1024576$$ trường hợp khác nhau. Ta có thể thực hiện duyệt như sau: 

```markdown
- Với con bò thứ `i`, ta quyết định nó được vắt sữa bởi từng máy, máy 1 -> máy 2, duy trì tổng lượng sữa mà mỗi máy vắt
- Đến con bò thứ ```N```, ta kiểm tra xem 2 máy có vắt cùng một lượng sữa không, nếu có thì ta đã tìm được 1 kết quả.
- Thực hiện tìm kiếm với con bò tiếp theo, trong trường hợp đã tính cả 2 trường hợp, ta quay lại con bò trước nó
- Việc kiểm tra sẽ dừng lại khi trường hợp ```222...222``` được kiểm tra
```

### Mã nguồn: 

```cpp
void backtrack(int id, long long sum1, long long sum2, string ans) {
    if(id > n) { // nếu đã duyệt đủ n con bò
        if(sum1 == sum2) {
            cout << s << endl;
            ok = true; // flag ok nếu bằng false thì in ra -1
            return;
        }
        else return;
    }
    backtrack(id + 1, sum1 + a[id], sum2, s + "1"); // cho con bò tiếp theo vào máy 1
    backtrack(id + 1, sum1, sum2 + a[id], s + "2"); // cho con bò tiếp theo vào máy 2
}
void solve() {
    cin >> n;
    for(int i = 1; i <= n; i ++) 
        cin >> a[i];
    backtrack(1, 0, 0, ""); // trạng thái bắt đầu
    if(!ok)  // flag ok kiểm tra có bất kỳ đáp án nào đúng không
        cout << -1;
}
```

# **Bài 2: Phân tích số**

Ý tưởng xử lý của bài này giống như bài 1, nhưng thay vì ta có 2 lựa chọn là 1 hoặc 2, giờ ta cần xây dựng 1 dãy không giảm có tổng bằng ```N```

Ta có thể thực hiện duyệt như sau

```
- Với số thứ i, ta lần lượt gán cho số thứ i giá trị từ max(ans[i - 1], 1)
- Sau khi thêm số thứ i, ta kiểm tra xem tổng các số hiện tại đã vượt quá n chưa, nếu bằng n -> 1 kết quả, nếu quá n thì quay lại
```

### Mã nguồn
```cpp
void backtrack(int sum, vector<int> v) {
    if(sum == n) { // tìm được 1 đáp án
        cout << n << " = ";
        for(int i = 0; i < v.size(); i ++) {
            cout << v[i];
            if(i != v.size() - 1)
                cout << "+";
        }
        cout << endl;
        return;
    } else if (sum > n) {
        return;
    }
    int start = 1;
    if(v.size()) start = max(start, v[v.size() - 1]);
    // tối đa là đến n - sum
    for(int num = start; num <= n - sum; num ++) {
        v.push_back(num);
        backtrack(sum + num, v);
        v.pop_back();
    }
}
void solve() {
    cin >> n;
    vector<int> v(0);
    backtrack(0, v);
}
```

# **Bài 3: Sinh tổ hợp**
# **Bài 4: Tổ hợp nguyên tố**
# **Bài 5: Nuôi bò**

