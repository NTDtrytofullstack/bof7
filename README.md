# bof7
## ý Tưởng.
![image](https://github.com/NTDtrytofullstack/bof7/assets/130078745/3d3f990b-17cc-4e82-8b3a-ee9872e30239)
- Ngày hôm nay ta sẽ tới 1 dạng mới đó là ret2libc phức tạp hơn , khó hơn.
- Nhìn vào bài hôm nay ta có thể thấy là ko có bất cứ 1 hàm nào tạo shell mà chuỗi **/bin/sh** lại đc lưu trong địa chỉ của libc thì đầu tiên ta cần tìm offset của địa chỉ save rip và leak địa chỉ của libc ra để tìm địa chỉ của hàm system và mục tiêu chính của chúng ta là thực hiện lệnh system **("/bin/sh")**, bởi vì địa chỉ libc động nên sẽ có phần khó nhằn hơnn :>>.
## Tự Hành.
-
