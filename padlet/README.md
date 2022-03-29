# Tiểu sự về xss padlet

Đây là một web tạo sơ đồ hoặc lịch trình và note cho nhiều người dùng.
Và đây cũng là web trường mình chọn để cho các học sinh nộp bài tập online.
Vào hôm chiều lá rơi mình được 4đ lý nên mang web ra chọc chọc.
## Lưới qua các chức năng cơ bản có các api điển hình như

 - https://padlet.com/api/5/comments để đăng bài
 - https://padlet.com/api/5/contributing_status để upload bài
 - .....

Tất cả các dữ liệu đều được post lên cdn và khi load trang data sẽ được kéo về qua script 
--> upload inject không khả khi mấy 
--> mình chuyển hướng test qua các case xss

## Khai thác

### Ở api đăng bài (status) 

- Thì được filter khá chặt nhưng phần post được public cho mọi người nên có thể lợi dụng để đăng ảnh tài liệu free ._. vì nó không có giới hạn dữ liệu.

### Ở api comment

- Ở đây mình test ngay case đầu
`<h1>vhae</h1>`
- quá ngon vậy thì bị xss inject rồi mlem.
- Nhưng điều khó khăn ở đây là nó chỉ dùng được các thẻ tinh chỉnh chữ 
`<p><h1><h2>...` còn các thẻ nguy hiểm như `script` hoặc gọi đến event `onerror` đều bị WAF chặn không cho kéo data về.
- Đến bước này mình test tiếp các case và một cái đã được đi qua đó là trỏ hàm về `keyframes`
ex 
```
<style>@keyframes slidein {}</style>
<xss style="animation-duration:1s;animation-name:slidein;animation-iteration-count:2" onanimationiteration="alert(`VHAE`)"></xss>
```

Mình đã report lên email sup của họ và đã nhận được 

![pad xss](pad.png)

và ngay sau đó mình đã thực hiện trên đường dẫn nộp lớp học :) 

![anh xss](car.png)

Câu truyện hôm sau :<

![](cogiao.png)
![](voz.png)
![](xinloi.png)