---
title: HD Giải - (27/9/2025) Ôn lại các kĩ thuật lập trình
published: 2025-09-27
description: Hướng dẫn giải bài tập 8I0 ngày 27-09-2025
tags: [Editorial]
category: 8I0
draft: false
---

# Hướng dẫn giải cho (27/9/2025) Ôn lại các kĩ thuật lập trình 

## **Bài 1: Không sắp xếp**

### **Yêu cầu đề bài**: Dãy không được phép sắp xếp tăng dần hay giảm dần:

Ta xét 2 trường hợp

Trường hợp dãy có 2 phần tử:
1 2
2 4 
-> n < 3: ta không có cách để làm cho dãy ko tăng hoặc ko giảm.
1 1 1 1 1 1 1 1 1 
-> dãy chỉ gồm các phần tử bằng nhau -> không có cách
Hai tình huống này sẽ in ra -1.
Không cần quan tâm đến trạng thái ban đầu của dãy.
ta cứ sx dãy tăng dần -> nếu in dãy đã sx là sai 
Để ko sai ta điều chỉnh dãy từ đã sx thành không được sx:
1 2 1 3 4 5
Tóm lại:
Đọc dãy
SX dãy tăng dần
KIểm tra xem: dãy ít hơn 3 phần tử hoặc dãy bằng nhau thì in ra -1
Còn không dựa trên dãy đã sx tăng dần ta tìm 2 vị trí cạnh nhau mà a[i] < a[i + 1], để đổi chỗ vị trí hai phần tử này
Sau đó in KQ.
2 1 1 1 1


```markdown
---
title: Code đẹp và kỹ thuật
published: 2025-09-27
description: Hướng dẫn giải bài tập 8I0 ngày 24-09-2025
tags: [Editorial]
category: 8I0
draft: false
---
