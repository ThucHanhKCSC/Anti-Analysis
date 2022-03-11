Nguồn: https://blog.malwarebytes.com/threat-analysis/2014/09/five-anti-debugging-tricks-that-sometimes-fool-analysts/

# Cổng I/O của Vmware

Đây là kĩ thuật AntiVM, (Anti virtual machine)

Chương trình chạy máy ảo sử dụng cổng vào/ra (I/O) đặc biệt đẻ giao tiếp giữa máy ảo và máy vật lý, Malware có thể lợi dụng điều này để phát hiện bản thân đang chạy trong máy ảo, từ đó tắt đi một số tính năng

![vmware](https://user-images.githubusercontent.com/101321172/157830984-77b767f7-17b5-4831-889c-719b1f703c8a.png)

Nếu đoạn code này chạy trên Vmware, sau khi chạy xong, ebx sẽ giữ cái magic number. Đây là 1 cách hiệu quả để cho thấy tiến trình đang chạy bằng máy ảo Vmware
