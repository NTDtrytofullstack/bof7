# bof7
## ý Tưởng.
![image](https://github.com/NTDtrytofullstack/bof7/assets/130078745/3d3f990b-17cc-4e82-8b3a-ee9872e30239)
- Ngày hôm nay ta sẽ tới 1 dạng mới đó là ret2libc phức tạp hơn , khó hơn.
- Nhìn vào bài hôm nay ta có thể thấy là ko có bất cứ 1 hàm nào tạo shell mà chuỗi **/bin/sh** lại đc lưu trong địa chỉ của libc thì đầu tiên ta cần tìm offset của địa chỉ save rip và leak địa chỉ của libc ra để tìm địa chỉ của hàm system và mục tiêu chính của chúng ta là thực hiện lệnh system **("/bin/sh")**, bởi vì địa chỉ libc động nên sẽ có phần khó nhằn hơnn :>>.
## Tự Hành.
- Sau khi check bit canary đã tắt thì ta có thể can thiệp và sử dụng đc phương pháp ret2libc.
- Tạo 120 byte để có cùng check xem offset.

- Sau khi check thì offset ta tìm đc là 88 , ta khai thác ở hàm **puts** để leak đc địa chỉ. 
- Với 2 phương thức mới là **GOT** và **PLT**: GOT là global offset table nơi chứa địa chỉ của hàm libc và PLT là procedure linkage table hàm thực thi các lệnh đc chứa hay nằm trong GOT.

- Ta cùng tìm địa chỉ gadget **rdi** để của thể truyền vào đấy địa chỉ hàm **puts**.

- Viết tools để có thể nhận lại địa chỉ đc leak ra.

-Sau khi tìm được địa chỉ leak ra ta cần phải so sánh lại đã chuẩn địa chỉ ta cần tìm chưa.

- Sau khi check lại ta cần trừ bớt đi 1 lượng bằng với địa chỉ của hàm **puts** ta viết như sau.

- Tiếp đến ta chỉ cần thực hiện lại chương trình , kẹp vào gadget **rdi** là địa chỉ của nơi chứa chuỗi */bin/sh** đc lưu trong libc , ta chỉ cần thực hiện hàm system nữa là xong.

-Thử debug xem thử cách bài này hoạt động như thể nào.
-Ta thử kết nối sever và xem flag là gì nhóe .

-Sau khi kết nối thì ta thu đc flag như sau.
