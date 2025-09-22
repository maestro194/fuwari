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

## Chọn màu - MARBLES

## csphn_2d_beauty_str - Simple Math

Thay vì kiểm tra độ đẹp của từng đoạn xâu, ta có thể đổi thứ tự tính tổng: 

> Với mỗi nguyên âm ```a[i]```, ta tính lượng đóng góp của nó vào các xâu con chứa nó

$$
\sum{l<=i<=r} 1/(r - l + 1)
$$

Gọi ```A = i - 1``` và ```B = n - i``` lần lượt là số ký tự bên trái và bên phải của nguyên âm ```i```. Khi chọn xâu con chứa vị trí ```i```,  ta chọn ```a``` kí tự bên trái ```(0 <= a <= A)```, và ```b``` kí tự bên phải ```(0 <= b <= B)```. Độ dài xâu con là ```a+b+1```. Đóng góp của ```a[i]``` là:

$$
a=0∑A​b=0∑B​a+b+11​.
$$

Xét theo ```b```, ta có 

$$
\sum{a=0}^{A} ​(H_{a+B+1}​−H_a​),
$$

trong đó $$H_k = \sum(t=1)^k \frac{1}{t}$$

Đặt ```preH[k]``` là prefix sum từ 1 đến ```k``` của mảng H

Ta có công thức
```
vow[i] = preH[n] - preH[B] - preH[A],
```

với ```A = i - 1```, ```B = n - i```

Đáp số là tổng kết quả của các nguyên âm

### Code
```cpp
string s;
if(!(cin>>s)) return 0;
int n = s.size();
vector<long double> H(n+1);
H[0]=0.0L;
for(int i=1;i<=n;i++) H[i]=H[i-1]+1.0L/i;
vector<long double> SH(n+1);
SH[0]=H[0];
for(int i=1;i<=n;i++) SH[i]=SH[i-1]+H[i];
unordered_set<char> vowels = {'U','E','A','I','O','Y'};
long double ans = 0.0L;
for(int i=1;i<=n;i++){
    if(vowels.count(s[i-1])){
        int A = i-1;
        int B = n-i;
        ans += SH[n] - SH[B] - SH[A];
    }
}
cout.setf(std::ios::fixed); cout<<setprecision(10)<<(double)ans<<"\n";
```
