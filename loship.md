# Xss stored (BONUS $$$$)

Cách tôi khai thác xss stored qua các subdoamin của loship.
Trước hết tôi đã lưu được đoạn js độc vào database của loship thông qua cách chỉnh sửa phần name ở domain : **[https://loship.vn/tai-khoan](https://loship.vn/tai-khoan)**

## payload ```VHAE"><Script>alert`xss Vhae`</script>```
![](https://lh5.googleusercontent.com/cuUa-1JgDA3LSeC-UfjeP1dPGcHnQ346AmaFpswTc1jk5kp2v0OWqz7pxrWAsNO8x1JOl4HFTXXlJFZvlHr6uPDKg7vnhWMcd3XTXQ0BMkjxc8b7twVQ5skrTfnuNp6I5YaGTVUV9_sRNEmvzpUD3A)

như các bạn thấy lúc đổ dữ liệu ra trang web thì đã không thực thi xss vì trước khi chuyển tên dữ liệu mình ra web họ đã encode một số thành phần như ký tự " từ đó câu lệnh của mình không thể escape khỏi thẻ name nhưng minh đã scan các subdomain của loship thì có một domain mà phần tên mình đổ ra nhưng không được lọc đó là https://hoidap.loship.vn khi đó đoạn script đã được escape và thực thi vì cả 2 web đều sử dụng cookie nên ta có thể đánh cắp để xác thực và các tk còn lại:
**![](https://lh3.googleusercontent.com/mFL2PKr12Ngo5Zq5rLAqLzBhOoGa638cCiJBl-53-_CIh631LIKnkJ9tYQaph3VOSoAmbWnpMj5pQ9kR9XKDdEk1d-JC1TZdeBi_xAtpGvwYTnCfuIOkMxNnealOsRSGKPvHN7cmqoaHmK6PucDmRw) 

## Bài học:
Từ đó ta thấy được sự nguy hiểm của stored khi chỉ khác phục bằng cách lọc dữ liệu khi đổ ra thay vì lọc ngay từ khi ghi dữ liệu vào db.

._. 