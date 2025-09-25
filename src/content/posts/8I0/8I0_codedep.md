---
title: Code đẹp và kỹ thuật
published: 2025-09-24
tags: [Editorial]
category: 8I0
draft: false
---

# Hướng dẫn giải cho Code đẹp 

## BPRIME

Sử dụng sàng nguyên tố để đếm số ước nguyên tố của 1 số
Dùng Prefix Sum để có thể thực hiện truy vấn từ a đến b trong O(1)

```cpp
vector<vector<ll>> sieve() {
    const int n = 1e6;
    vector<ll> p(n+1, 0);
    vector<vector<ll>> pf(8, vector<ll>(n+1));
    for(ll i = 2; i <= n; i++) {
        if(p[i] == 0) {
            for(ll j = i; j <= n; j += i) p[j]++;
        }
    }
    for(ll i = 1; i <= 7; i++) {
        for(ll j = 0; j <= n; j++) {
            if(j == 0) pf[i][j] = 0;
            else pf[i][j] = pf[i][j-1] + (p[j] == i ? 1 : 0);
        }
    }
    return pf;
}

ll ans(int a, int b, int k) {
    ll res = pf[k][b] - pf[k][a-1];
    return res;
}
```


```markdown
---
title: Code đẹp và kỹ thuật
published: 2025-09-24
tags: [Editorial]
category: 8I0
draft: false
---
