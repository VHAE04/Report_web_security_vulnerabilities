# Cuộc trò chuyện ẩn trong zalo.

Sau khi tìm tòi ở các file zalo tôi thấy được tệp xử lý `js` mã pincode của phần ẩn tin nhắn ở mục sau:

![](https://github.com/VHAE04/Report_web_security_vulnerabilities/blob/main/Pincode_zalo/1soucethuthi.PNG?raw=true)

Ta sẽ thấy khi pincode thay zalo sẽ mã hoá `AES` của 2 key `new_pin` = `md5(pincode)` và `imei` ở đây chính là `uuid` được tạo ra.

![](https://github.com/VHAE04/Report_web_security_vulnerabilities/blob/main/Pincode_zalo/2_encode_AES_parram.PNG?raw=true)

Sau đó sẽ được `post` lên server dưới dạng `base64`

![](https://github.com/VHAE04/Report_web_security_vulnerabilities/blob/main/Pincode_zalo/4post_changepin.PNG?raw=true)


Ta thể tự tạo hàm mã hoá AES để đổi mã pin mà không cần mã pin cũ ._. nhưng cách này khá lười nên tôi sẽ dạy bạn đặt breakpoint cho nhanh.

# Sau một hồi debug tôi tìm được các hàm thú vị có thể đặt breakpoint:

-----
 
### 1 Thay đổi pincode một cách tuỳ ý (mặc định phải là số) nhưng ta có thể đặt mọi ký tự để tăng tính bảo mật :D.

Lưu ý vì pincode được hash bằng `md5` nên ta có thể đặt pin code tuỳ ý tuy nhiên hàm check pincode có CheckLength = 4 nên ta chỉ có thể đặt 4 ký tự

![](https://github.com/VHAE04/Report_web_security_vulnerabilities/blob/main/Pincode_zalo/4checklength.PNG?raw=true)

Ta tiến hành đặt pincode bình thường và ẩn tin nhắn đi, đặt `breakpoint` ở dòng `98226` sau đó ta vào thay đổi mã pincode sau khi bị ngắt lệnh ta vào bảng console thay đổi giá trị ` e="[4kytu]"` và ta cho chạy tiếp code.

### 2 Lấy hash md5 pincode.

Ta tiến hành đặt `breakpoint` ở dòng `98259` và nhập vào ô tìm kiếm lệnh sẽ bị ngắt và ta có thể lấy được `md5(pincode)` ở `return value` phần `Scope` vì chỉ có 4 số nên có thể copy ném lên google để ra mã ngay.

![](https://github.com/VHAE04/Report_web_security_vulnerabilities/blob/main/Pincode_zalo/5_getpin.PNG?raw=true)

### 3 Lấy toàn bộ cuộc trò truyện ẩn mà ko cần pincode.

Ta tiến hành đặt `breakpoint` ở dòng `98262` và nhập vào ô tìm kiếm lệnh sẽ bị ngắt sau đó ta thay đổi giá trị của ` return` thành `true` và cho câu lệnh tiếp tục chạy.

![](https://github.com/VHAE04/Report_web_security_vulnerabilities/blob/main/Pincode_zalo/6bypasspin.PNG?raw=true)

Vì nó hơi khó hiểu với một số người nên tôi đã làm video các bạn có thể tham khảo:

1  [Change pincode](https://www.youtube.com/watch?v=mP8z5SNq7qw)

2  [Get pincode](https://www.youtube.com/watch?v=ihBFYr7X9wQ)

3  [Bypass pincode](https://www.youtube.com/watch?v=Wo_AxGmlZd8)

Bài viết được tham khảo từ [nextheia](https://nextheia.com/blog/con-cua-ban-toi-dung-zalo/)

#### Lưu ý: Đây không phải là một lỗi bảo mật của Zalo.


