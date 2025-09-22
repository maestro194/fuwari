---
title: 8I0 - Xâu kí tự
published: 2025-09-22
description: Hướng dẫn giải bài tập Xâu kí tự
tags: [Editorial]
category: 8I0
draft: false
---

# 8I0 - Xâu kí tự

## Tráo bài - CARDSEZ

Với giới hạn thấp, ta có thể thực hiện tất cả ```m``` lần tráo bài trong bộ bài

```cpp
void perform_shuffle(vector<int>& cards, int i, int m, int j) {
    i--; j--;
    vector<int> taken(cards.begin() + i, cards.begin() + i + m);
    cards.erase(cards.begin() + i, cards.begin() + i + m);
    if (j == cards.size()) {
        cards.insert(cards.end(), taken.begin(), taken.end());
    } else {
        cards.insert(cards.begin() + j, taken.begin(), taken.end());
    }
}
```

## Xóa chữ số - DELDIGIT

Ta sẽ dùng stack/greedy với tính đúng đắn được chứng minh như sau:

Để tạo số lớn nhất, tại mỗi vị trí ta muốn chữ số càng lớn càng tốt ở bên trái.

Nếu có một chữ số nhỏ đứng trước một chữ số lớn hơn và ta chưa hết quota xóa, thì luôn có lợi khi loại bỏ chữ số nhỏ đó. Khi duyệt hết, nếu ```k``` chưa hết thì xóa từ cuối (vì xóa các chữ số ở cuối/chữ số bé hơn làm số lớn hơn so với xóa ở giữa).

### Pseudocode
- Duyệt từ trái qua phải các chữ số.
- Dùng một stack giữ kết quả.
- Khi gặp chữ số mới ```c```:
    - Trong khi stack chưa rỗng, ```top < c``` và vẫn còn ```k > 0```, ta pop bớt nhỏ hơn đứng trước ```c``` để nhường cho ```c``` tạo số lớn hơn.
- Sau khi duyệt hết, nếu vẫn chưa xóa đủ ```k```, ta xóa tiếp ở cuối.
- Kết quả là các chữ số còn lại trong stack.

### Code
```cpp
void solve(){
    string s;
    int k;
    cin >> s >> k;
    int n = s.size();
    stack<char>st;
    for (int i = 0; i < n; i++){
        while (!st.empty() && st.top() < s[i] && k > 0)
            st.pop(), k--;
        st.push(s[i]);
    }
    while (k > 0 && !st.empty())
        st.pop(), k--;
    
    string res;
    while (!st.empty()) {
        res.pb(st.top());
        st.pop();
    }
    reverse(res.begin(), res.end());
    int cnt = 0;
    while (cnt < res.size() && res[cnt] == '0')
        cnt++;
    if (cnt == res.size())
        cout << 0;
    else
        cout << res.substr(cnt);
}
```