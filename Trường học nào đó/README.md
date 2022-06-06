# Broken Access Control

Đây là một lỗ hổng khá phổ biến nó đã ở trong top 1 `OWASP` năm 2021 

### Target: http://[secret].vn

khi ta truy cập vào web ngay lập tức ta được chuyển hướng về trang login 

`http://[secret].vn/webapp/cp/auth/login.html`

Ở đây mình đã thử fuzzing url thì phát hiện ra lỗ hổng phân quyền trên web được truy cập home ở quyền admin ở 2 url

`http://[secret].vn/webapp/cp/users.html` ==> 403

![](https://github.com/VHAE04/Report_web_security_vulnerabilities/blob/main/Tr%C6%B0%E1%BB%9Dng%20h%E1%BB%8Dc%20n%C3%A0o%20%C4%91%C3%B3/1.PNG?raw=true )

`http://[secret].vn/webapp/cp/auth/login` ==> home page admin

![](https://github.com/VHAE04/Report_web_security_vulnerabilities/blob/main/Tr%C6%B0%E1%BB%9Dng%20h%E1%BB%8Dc%20n%C3%A0o%20%C4%91%C3%B3/2.PNG?raw=true )

`http://[secret].vn/webapp/cp/users` ==> page add users

![](https://github.com/VHAE04/Report_web_security_vulnerabilities/blob/main/Tr%C6%B0%E1%BB%9Dng%20h%E1%BB%8Dc%20n%C3%A0o%20%C4%91%C3%B3/3.PNG?raw=true )

Đến đây ta chỉ cần thêm sửa users và có thể tiếp tục khai thác sâu hơn qua các hàm chức năng upload lịch trình ....
