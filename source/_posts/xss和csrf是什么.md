---
title: "xss和csrf是什么"
catalog: true
date: 2022-04-03 21:16:28
subtitle: "学习经验."
header-img: 
tags:
- HTTP
catagories:
- HTTP
---

### 笔记描述
>  xss和csrf的概念以及应对方法

### XSS
`Cross-Site Scripting(跨站脚本攻击)`，简称XSS，是一种代码注入攻击。攻击者通过在目标网站上注入恶意脚本，使之在用户的浏览器上运行。利用这些恶意脚本，攻击者可获取用户的敏感信息如Cookie、SessionID等，进而危害数据安全
>简单来说，任何可以输入的地方都有可能引起XSS攻击，包括URL
    + 在HTML内嵌的文本中，恶意内容以script标签形成注入
    + 在标签的 href、src 等属性中，包含 javascript: (伪协议)等可执行代码。
    + 在 onload、onerror、onclick 等事件中，注入不受控制代码。
    + 在 style 属性和标签中，包含类似 background-image:url("javascript:..."); 

### CSRF
`Cross-site request forgery(跨站请求伪造)`，是一种挟持用户在当前已登陆的Web应用程序上执行非本意的操作的攻击方法。绕过后台的用户验证，达到冒充用户对被攻击的网站执行某项操作的目的。
>受害者必须依次完成两个步骤：
1.登录受信任网站A，并在本地生成Cookie。
2.在不登出A的情况下，访问危险网站B。


### 防御方法
`xss`:利用模板引擎避免内联事件、避免拼接、HTML增加攻击难度，降低攻击后果、主动检测和发现
过滤或移除特殊的 html 标签：<script>、<iframe>等。（黑名单）
过滤 js 事件的标签：onclick、onerror、onfocus等。（黑名单）

`CSRF`:使用token验证、使用验证码等



