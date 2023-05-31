# bof7
## ý Tưởng.
![image](https://github.com/NTDtrytofullstack/bof7/assets/130078745/3d3f990b-17cc-4e82-8b3a-ee9872e30239)
- Ngày hôm nay ta sẽ tới 1 dạng mới đó là ret2libc phức tạp hơn , khó hơn.
- Nhìn vào bài hôm nay ta có thể thấy là ko có bất cứ 1 hàm nào tạo shell mà chuỗi **/bin/sh** lại đc lưu trong địa chỉ của libc thì đầu tiên ta cần tìm offset của địa chỉ save rip và leak địa chỉ của libc ra để tìm địa chỉ của hàm system và mục tiêu chính của chúng ta là thực hiện lệnh system **("/bin/sh")**, bởi vì địa chỉ libc động nên sẽ có phần khó nhằn hơnn :>>.
## Tự Hành.
- Sau khi check bit canary đã tắt thì ta có thể can thiệp và sử dụng đc phương pháp ret2libc.
![image](https://github.com/NTDtrytofullstack/bof7/assets/130078745/9feeebac-77d9-453d-9a6b-75069e590c1f)
- Tạo 120 byte để có cùng check xem offset.
![image](https://github.com/NTDtrytofullstack/bof7/assets/130078745/e8d8705e-ea03-416c-ada7-3776dd4d4606)
- Sau khi check thì offset ta tìm đc là 88 , ta khai thác ở hàm **puts** để leak đc địa chỉ. 
- Với 2 phương thức mới là **GOT** và **PLT**: GOT là global offset table nơi chứa địa chỉ của hàm libc và PLT là procedure linkage table hàm thực thi các lệnh đc chứa hay nằm trong GOT.
![image](https://github.com/NTDtrytofullstack/bof7/assets/130078745/af15232c-4151-4ed0-9020-d25ea985ad50)
- Hàm **puts** là 1 hàm in ra tất cả giá trị của 1 con trỏ mà nó lưu vì thể để có thể leak ra đc địa chỉ libc ta cần sử dụng gadget ( cụ thể là rdi như trong hình ) truyền vào đó địa chỉ của hàm **puts** để thực hiện in ra dữ liệu sau khi ta đã nhập vào đấy, sử dụng buffer overflow như 1 công cụ khai thác.
![image](https://github.com/NTDtrytofullstack/bof7/assets/130078745/e0d4486a-ab81-4ab0-9959-ec41d73eda6f)
- Tìm địa chỉ của hàm **puts**.
![image](https://github.com/NTDtrytofullstack/bof7/assets/130078745/ece79c75-e8d3-49d0-b292-60b85efc339e)
- Ta cùng tìm địa chỉ gadget **rdi** để của thể truyền vào đấy địa chỉ hàm **puts**.
![image](https://github.com/NTDtrytofullstack/bof7/assets/130078745/6c795f29-564f-4e1b-abc5-f3e5cefeb974)
- Viết tools để có thể nhận lại địa chỉ đc leak ra.
- Ta có 2 cách để lấy địa chỉ hàm **Puts**: 1 là cách thủ công như ở trên 2 là sử dụng lệnh sau **exe.got['puts']** để trực tiếp nhờ chương trính lấy dùm.
![image](https://github.com/NTDtrytofullstack/bof7/assets/130078745/a2488ac4-939a-4ae8-8076-aff3e066a9d2)
-Ta thực thi hàm thực thi **PLT** để thực hiện đc hàm **GOT**.
![image](https://github.com/NTDtrytofullstack/bof7/assets/130078745/9241feda-f1e5-4a81-acd5-43e13805e6fc)
-Khi chạy thử thì chương trình end luôn vì thể để mà leak đc địa chỉ ta cần phải để nó chạy tiếp tục vào hàm **main** để tiếp tục chương trình.
![image](https://github.com/NTDtrytofullstack/bof7/assets/130078745/fcae44a9-140b-48c3-b833-0c5c7910ad7e)
![image](https://github.com/NTDtrytofullstack/bof7/assets/130078745/a1b253e0-6d8d-4553-9767-14ce4a031a0d)
![image](https://github.com/NTDtrytofullstack/bof7/assets/130078745/410fe938-163f-4b38-8f10-76873a31514c)
![image](https://github.com/NTDtrytofullstack/bof7/assets/130078745/7b4e22e7-50bc-4eb9-a040-a00336ecd763)
-Trước khi đó ta cần nhập hàm puts và địa chỉ đc leak ra down về file libc , patched chúng lại , load lên tools file libc để có thể sử dụng địa chỉ có nó , kết nối với sever để có thể nhập đúng địa chỉ libc in ra đc shell ( cái khó ở đây là phải check từng file :(  ) .
![image](https://github.com/NTDtrytofullstack/bof7/assets/130078745/cce40c8b-30dd-43a6-82c8-abf08780ac76)
![image](https://github.com/NTDtrytofullstack/bof7/assets/130078745/d606fce9-adbe-4ca3-90b3-22f8418adedb)
- Sau khi check lại ta cần trừ bớt đi 1 lượng bằng với địa chỉ của hàm **puts** ta viết như sau.
![image](https://github.com/NTDtrytofullstack/bof7/assets/130078745/794bd844-d18a-496c-a479-f41aa3633d1e)
- Tiếp đến ta chỉ cần thực hiện lại chương trình , kẹp vào gadget **rdi** là địa chỉ của nơi chứa chuỗi */bin/sh** đc lưu trong libc , ta chỉ cần thực hiện hàm system nữa là xong.
![image](https://github.com/NTDtrytofullstack/bof7/assets/130078745/0886fd3f-00de-4228-a176-1613c0a95f5e)
-Ta thử kết nối sever và xem flag là gì nhóe .
-Sau khi kết nối vầ chạy tools ta có thể thấy file **flag.txt** , chỉ cần **cat flag.txt** là sẽ thu đc đc flag.
![image](https://github.com/NTDtrytofullstack/bof7/assets/130078745/ef92bb5f-3b81-4670-aba0-ecaa44864faa)
-Sau khi kết nối thì ta thu đc flag như sau.
![image](https://github.com/NTDtrytofullstack/bof7/assets/130078745/25c096a3-8418-48b1-9815-73dd4cbd212c)
