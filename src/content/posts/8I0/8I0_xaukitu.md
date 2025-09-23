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
\sum_{l<=i<=r} \frac{1}{r - l + 1}
$$

Gọi ```A = i - 1``` và ```B = n - i``` lần lượt là số ký tự bên trái và bên phải của nguyên âm ```i```. Khi chọn xâu con chứa vị trí ```i```,  ta chọn ```a``` kí tự bên trái ```(0 <= a <= A)```, và ```b``` kí tự bên phải ```(0 <= b <= B)```. Độ dài xâu con là ```a+b+1```. Đóng góp của ```a[i]``` là:

$$
\sum_{a=0}^A \sum_{b=0}^B ​a+b+1​
$$

Xét theo ```b```, ta có 

$$
\sum_{a=0}^{A} ​(H_{a+B+1}​−H_a​)
$$

trong đó 
$$
H_k = \sum_{t=1}^k \frac{1}{t}
$$

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
for(int i = 1;i <= n;i ++) 
    H[i] = H[i - 1] + 1.0L / i;

vector<long double> SH(n+1);
SH[0]=H[0];
for(int i = 1; i <= n; i ++) 
    SH[i] = SH[i - 1] + H[i];

unordered_set<char> vowels = {'U','E','A','I','O','Y'};
long double ans = 0.0L;
for(int i = 1; i <= n; i ++){
    if(vowels.count(s[i-1])){
        int A = i - 1;
        int B = n - i;
        ans += SH[n] - SH[B] - SH[A];
    }
}
cout.setf(std::ios::fixed); 
cout<<setprecision(10)<<(double)ans<<"\n";
```

## string_strcmp - So sánh xâu

### Subtask 1: 

Ta có thể thực hiện so sánh 2 xâu trong mọi truy vấn và đưa ra kết quả

Độ phức tạp: ```O(nq)```

### Subtask 2:

Với việc chỉ có 2 kí tự trong xâu ```S```, ta có thể tìm được xâu nhỏ hơn bằng cách tìm xem xâu nào chứa kí tự nhỏ hơn ở index nhỏ hơn

Không mất tính tổng quát, giả sử xâu ```S``` chỉ chứa kí tự ```a``` và ```b```, với mỗi xâu con ```[l, r]```, ta tìm xem ```a``` xuất hiện lần đầu tiên ở vị trí nào

Xâu ```[l,r]``` sẽ bé hơn theo thứ tự từ điển so với xâu ```[u,v]``` khi:
- Index ```i``` của ```'a'``` trong xâu ```[l,r]``` bé hơn so với index ```j``` của ```'a'``` trong xâu ```[u,v]```.
- Xâu ```[u,v]``` không chứa ```'a'``` nhưng ```[l,r]``` thì có
- Xâu ```[l,r]``` là xâu con tính từ đầu xâu ```[u,v]```

Độ phức tạp: ```O(qlogn)```

### Subtask 3:

Việc so sánh trên với nhiều ký tự tỏ ra không khả thi với độ phức tạp gốc. Vì thế, ta có thể áp dụng kỹ thuật hashing, mã hóa xâu thành số nguyên, để thực hiện so sánh xâu nhanh hơn.

Từ đề bài, ta thấy rằng, chỉ cần ta tìm được vị trí đầu tiên mà ```[l,r][i] != [u,v][j]```, ta có thể đưa ra đáp án.

#### Hashing
Kỹ thuật hashing dựa trên việc ta có thể chuyển các giá trị xâu kí tự thành các số nguyên và thực hiện so sánh trên chúng.

Với bài toán này, ta thực hiện hashing như sau:

```cpp
long long base = 311;
long long mod = 1e9 + 7;
long long prefix_hash[200005];
long long base_power[200005];

int n, q;
string s;

long long powmod(long long a, long long b) {
    if (b <= 1) return b == 1 ? b : 1;
    long long g = powmod(a, b >> 1);
    g = g * g % mod;
    if (b & 1) g = g * a % mod;
    return g;
}

void build_hash() {
    base_power[0] = 1;
    for(int i = 1; i <= n; i ++) {
        prefix_hash[i] = prefix_hash[i - 1] * base + (s[i - 1] - 'a' + 1);
        /*
            hash có dạng:
            hash[1] = base^0 * s[0]
            hash[2] = base^1 * s[0] + base^0 * s[1]
            hash[3] = base^2 * s[0] + base^1 * s[1] + base^0 * s[2]
            hash[4] = base^3 * s[0] + base^2 * s[1] + base*1 * s[2] + base^0 * s[3]
            và cứ như vậy
        */
        base_power[i] = base_power[i - 1] * base % mod;
    }
}
```

Tại sao ta lại xây dựng hashing bằng Prefix Sum? Vậy nếu truy vấn một xâu con ```[l,r]``` thì ta lấy giá trị của xâu con đó như thế nào?

Cách tính của hashing giống với Prefix Sum, nhưng có 1 chút khác biệt. Ví dụ trong đoạn code dưới đây:

```cpp
long long get_hash(int l, int r) {
    return (prefix_hash[r] - prefix_hash[l - 1] * base_power[r - l + 1] + mod) % mod;
}
```

Với cách tính trên, ta có thể thực hiện truy vấn như sau:

```cpp
while(q--)
{
    int l , r , u , v;
    cin >> l >> r >> u >> v;
    l --, r --, u --, v --;

    int res = 0 , L = 0 , R = min(r - l , v - u);

    while(L <= R){
        int mid = (L+R)/2;

        if(get_hash(l , l+mid-1) == get_hash(u , u+mid-1)){
            res = mid;
            L = mid + 1;
        }
        else R = mid - 1;
    }

    // Nếu độ dài 2 xâu con bằng nhau và không có vị trí khác nhau
    if(r - l + 1 == v - u + 1 && res == r - l + 1) {
        cout << 2 << '\n';
    }
    // Nếu độ dài 2 xâu con khác nhau và không có vị trí khác nhau
    else if(res == min(r - l + 1 , v - u + 1)) {
        if(r - l + 1 < v - u + 1) cout << 0 << '\n';
        else cout << 1 << '\n';
    }
    // Nếu có vị trí khác nhau
    else{
        if(s[l + res] < s[u + res]) cout << 0 << '\n';
        else cout << 1 << '\n';
    }
}
```

#### Pseudocode: 
- Hashing xâu ban đầu
- Với mỗi truy vấn, tìm vị trí đầu tiên có sự khác biệt giữa 2 xâu con
- So sánh vị trí khác nhau và đưa ra kết quả

Độ phức tạp: ```O(qlogn)```