# Kiểm tra bảng register

Trong hệ thống có:
Local Descriptor Table Register (LDTR): bảng mô tả thanh ghi cục bộ
Global Descriptor Table Register (GDTR): bảng mô tả thanh ghi toàn cục
Interrupt Descriptor Table Register (IDTR): 

Chúng phải được chuyển đến một vị trí khác khi hệ điều hành khách đang chạy để tránh xung đột với máy chủ. 

Malware sẽ sử dụng các lệnh
```nasm
SLDT reg ;Store Local Descriptor Table Register: (	Stores segment selector from LDTR in r/m16.) Lưu giá trị LDTR vào reg/dịa chỉ cho trc

SGDT reg ;Store global Descriptor Table Register:

SIDT reg ;Store interrupt Descriptor Table Register: 
```
Để nhận giá trị trong các thanh ghi này

![descriptor-registers](https://user-images.githubusercontent.com/101321172/157834901-d9072d49-95ba-41b3-8da0-ee2917cc6dd9.png)

