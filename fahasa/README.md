# DOM Based XSS BYPASS (BONUS \$$$$)

Endpoint : https://www.fahasa.com/search?in_stock=0&q=[xss bypass]

Inject : ```123\' }})}alert(1);{({///\```

Lỗi này sinh ra khi fahasa có sử dụng một extension js tìm kiếm và không may bộ phận sử lý lọc của extension trên đã không lọc kỹ ký tự ' nên đã tạo nên DOM XSS.

> Cách thức lọc của ets là chỉ thêm dấu escape `\` vào trước `'` để biến đổi ký tự đặc biệt thành ký tự thường ,ta bypass bằng cách nhập ```\'``` khi đó câu lệnh sẽ biến đổi thành` \\`' ký tự escape đã tự escape chính nó.

Dù vậy nhưng quá trình bypass cũng khá căng thẳng với một người ngu javascript như tôi vì khi một đoạn js trong script không hợp lệ thì cả file js đó sẽ không thực thi và báo lỗi trên con sole.


Bạn có thể xem bức ảnh và các bước dười đây:

![](https://github.com/VHAE04/Report_web_security_vulnerabilities/blob/main/fahasa/image/bypass.png?raw=true)

### Chú thích

#### 1 Thoát ra khỏi câu lệnh truy vấn.
#### 2 Kết thúc chuỗi truy vấn.
#### 3 Đóng câu lệnh của hàm vào để tệp js được thực thi.

# BUZZ !!! đi report thôi

![](https://github.com/VHAE04/Report_web_security_vulnerabilities/blob/main/fahasa/image/alert.png?raw=true)
