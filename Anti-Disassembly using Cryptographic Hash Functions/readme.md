Nguồn: https://ivanlef0u.fr/repo/madchat/vxdevl/avtech/Anti-Disassembly%20using%20Cryptographic%20Hash%20Functions.pdf

# Anti-Disassembly sử dụng hàm băm

Malware đôi khi sử dụng các kỹ thuật mã hóa nhằm gây khó khăn cho việc phân tích đối với các nhà nghiên cứu malware

Mục tiêu là sử dụng các hàm crypto khó phân tích và có thể được sử dụng để nhắm mục tiêu các máy tính hoặc người dùng cụ thể, code bị che đi và không có sẵn ở bất kỳ dạng có thể phân tích nào, ngay cả dạng được mã hóa, cho đến khi nó chạy thành công. 

Lấy challenge CTF làm ví dụ [link](https://play.picoctf.org/practice/challenge/151?category=3&page=3)

Hàm main nhận input, sau đó nối input 

![New Bitmap Image](https://user-images.githubusercontent.com/101321172/157843432-e5f36526-96d1-4c3c-b210-978415d51d7e.png)

Ý tưởng của bài sẽ là Lấy 4 byte dữ liệu của giá trị trả về của hàm MD5(), sau đó coi đó là opcode để thực hiện lệnh asm

Tác giả có gợi ý input có 4 kí tự là ```D1v1```

=> đoạn mã đầu sẽ là: MD5(D1v1GpLaMjEW)

![New Bitmap Image](https://user-images.githubusercontent.com/101321172/157844012-381a2d2c-f3a2-47ac-a2c0-a00cc84419c4.png)

Từ đó có được opcode: ```48``` ```89``` ```fe``` ```48```

![New Bitmap Image](https://user-images.githubusercontent.com/101321172/157844578-cd4da432-6711-4c3d-9e60-bff3208a2529.png)

Từ hàm kiểm tra thấy được tham số truyền vào được so sánh với ```0x7B3DC26F1```

![New Bitmap Image](https://user-images.githubusercontent.com/101321172/157844923-de9b6735-891f-41a1-9a6c-24ddfc78f587.png)

Vậy nhiệm vụ của chúng ta là tìm 12 ký tự còn lại 

Để đến được địa chỉ ```0x7B3DC26F1``` chúng ta có thể thực hiện lệnh sau

```nasm
mov rsi, rdi ;Có từ D1v1
mov rdi, 0x7B3DC26F1 ;Địa chỉ mong muốn đến được
call rsi ; call đến địa chỉ đấy
```

chuyển sang opcode thì sẽ được: 0xBF, 0xF1, 0x26, 0xDC, 0xB3, 0x07, 0x00, 0x00, 0x00, 0xFF, 0xD6

Đây là các byte cần bruteforce

Code bruteforce (lấy trên mạng :()

```python
import hashlib

requiredBytes = ["4889fe48", "bff126dc", "b3070000", "00ffd6"]
offsets = [8,2,7,1]
requiredString = ["GpLaMjEW", "pVOjnnmk", "RGiledp6", "Mvcezxls"]
found = False
password = []
for x in range(0, len(requiredString), 1):
    found = False
    #Generate 4 characters per iteration
    for a in range(33, 123, 1):
        for b in range(33, 123, 1):
            for c in range(33, 123, 1):
                for d in range(33, 123, 1):
                    hashThis = chr(a) + chr(b) + chr(c) + chr(d) + requiredString[x]
                    result = hashlib.md5(hashThis.encode()).hexdigest()
                    #print(result)
                    if (result[offsets[x]*2:offsets[x]*2+len(requiredBytes[x])] == requiredBytes[x]):
                        password.append(hashThis)
                        print("Found smth!")
                        print(hashThis[:4])
                        found = True
                        break

                if found:
                    break
            if found:
                break
        if found:
            break
```
