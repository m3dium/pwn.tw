# pwn.tw_START

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

Ngắt 0x80 đầu tiên, al =4 (eax = 4) chương trình gọi lệnh sys_write() để print vào stdout (ebx = 1) 20 kí tự (edx = 0x14) tại địa chỉ esp (ecx = esp) (tương ứng với hình bên dưới), mục đích là in dòng ‘Let’s start the CTF:’ ra màn hình

![image](https://user-images.githubusercontent.com/72652376/182280055-340f9987-d7b6-4751-a850-02c29f017eea.png)


![image](https://user-images.githubusercontent.com/72652376/182280283-0d5011ec-3e1e-4978-950c-5cec95dcc04b.png)

Ngắt 0x80 thứ hai, al=3 (eax = 3) chương trình gọi lệnh sys_read() đọc tối đa 60 kí tự (edx = 0x3c) từ stdin (ebx = 0), lưu vào stack tại vị trí esp (ecx = esp).

Tăng giá trị esp lên 20 (0x14) và return

Chương trình cho đọc vào đến 60 kí tự vào stack, trong khi sau đó chỉ tăng esp lên 20 và return. Ở đây chúng ta có thể Buffer Overflow xảy ra

Cơ bản của việc khai thác BO thành công là:

1. Shell code hợp lý với input vào

2. Khi có shell code phù hợp thì có thể điều khiển return address trỏ về đúng shellcode của ta.

Vào bài toán trên nếu ta điều khiển return address để chương trình return về 0x08048087 bằng cách gửi payload = ‘a’*0x14 + ‘\x87\x80\x04\x08’, 
chương trình sau khi return về sẽ thực hiện sys_write() thêm lần nữa, in ra 20 bytes trên stack. 

Vì 4 bytes đầu tiên trên stack lúc này chính là esp nên coi như ta đã leak được địa chỉ esp. Chương trình sẽ tiếp tục với một lệnh sys_read() nữa, ta sẽ gửi payload thứ hai: 

payload = ‘a’*0x14 + (esp+20) + shellcode

lúc này chương trình sẽ return về đúng shellcode mà ta cần.

![image](https://user-images.githubusercontent.com/72652376/182299839-369d0de7-931a-4cff-8c61-3fb74df6db2b.png)


![image](https://user-images.githubusercontent.com/72652376/182299808-5607507e-d5ef-4cfe-8e39-f4a49f03dd6e.png)

Sau đó có được flag
