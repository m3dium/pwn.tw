# pwn.tw

CHALLENGES 1:

![image](https://user-images.githubusercontent.com/72652376/182270406-ee0f9e95-35cd-470f-9260-ee15df123ed6.png)

![image](https://user-images.githubusercontent.com/72652376/182270492-0225ff27-1296-460a-839d-82d1eeec757b.png)

Kiểm tra và chạy thử chương trình

NOTE: ELF 32-bit là đuôi thực thi của linux

Dùng R2 để xem file

![image](https://user-images.githubusercontent.com/72652376/182274042-7dd88a74-58f0-4ad7-afe1-14b3c0336aa3.png)

Phân tích chương trình:

Push esp: đẩy giá trị esp vào stack
Push loc._exit: đẩy địa chỉ hàm thoát vào stack để kết thúc chương trình khi gọi return

4 dòng tiếp theo là khai báo các thanh ghi (xor eax, eax nghĩa là eax = 0)

5 lệnh tiếp theo có chức năng đưa chuỗi "Let’s start the CTF:’" vào stack

![image](https://user-images.githubusercontent.com/72652376/182275258-cb80b032-e64f-4e07-92d5-8feb4922852b.png)

Cần chú ý tiếp theo là int 0x80 nó là dấu hiệu của việc ngắt hàm

![image](https://user-images.githubusercontent.com/72652376/182275307-36e2bdea-7f8c-4f3b-9905-457e307bfc09.png)

Bảng các thanh ghi:

![image](https://user-images.githubusercontent.com/72652376/182276212-6d19d041-4f64-469c-aea9-4643afca989e.png)

![image](https://user-images.githubusercontent.com/72652376/182278251-1dd078f4-9110-452f-9718-abca6829d4fb.png)

