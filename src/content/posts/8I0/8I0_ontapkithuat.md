---
title: 8I0 - Ôn lại các kĩ thuật lập trình
published: 2025-09-27
description: Hướng dẫn giải bài tập 8I0 ngày 27-09-2025
tags: [Editorial]
category: 8I0
draft: false
---

# **Bài 1: Không sắp xếp**

**Yêu cầu đề bài**: Dãy không được phép sắp xếp tăng dần hay giảm dần:

### Ta xét các trường hợp sau:

### 1. Trường hợp dãy có 2 phần tử:
```
1 2
```
```
2 4 
```

Nhận thấy với n < 3, ta không có cách để làm cho dãy ko tăng hoặc ko giảm --> Kết quả luôn là -1

### 2. Trường hợp dãy chỉ chứa toàn các phần tử bằng nhau
```
1 1 1 1 1 1 1 1 1 
```
Dãy chỉ gồm các phần tử bằng nhau dẫn đến sắp xếp dãy như nào thì cũng chỉ chứa các phần tử xếp theo thứ tự tăng hoặc giảm dần.

### 3. Các trường hợp còn lại

Không cần quan tâm đến trạng thái ban đầu của dãy.
Ta sắp xếp dãy tăng dần, sau đó điều chỉnh dãy từ đã sắp xếp thành không được sắp xếp:

```
1 2 1 3 4 5
```

Để khiến dãy trở thành một dãy không được sắp xếp, chỉ cần tìm một vị trí ```i``` trong dãy đã sắp xếp sao cho ```a[i] < a[i + 1]``` và đổi chỗ chúng, ta được dãy không sắp xếp

Độ phức tạp: ```O(N)```

# **Bài 2: Bức tường**

# **Bài 3: Tích đoạn**

# **Bài 4: Bộ tứ**

# **Bài 5: Dạy học**

# **Bài 6: Chuyển dụng cụ**