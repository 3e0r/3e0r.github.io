---
title: '[WEB] Lab: Exploiting an API endpoint using documentation '
date: 2025-12-03T11:02:00+03:00
categories:
  - PortSwigger
  - Lab
tags:
  - web
  - API
excerpt: " "
header:
  overlay_color: "#ffffff"
  overlay_image: https://upload.wikimedia.org/wikipedia/commons/thumb/f/f2/Logo_of_PortSwigger.svg/2560px-Logo_of_PortSwigger.svg.png
  overlay_filter: 0.4
  actions:
    - label: "Lab PortSwigger"https://portswigger.net/web-security/learning-paths/api-testing/api-testing-api-documentation/api-testing/lab-exploiting-api-endpoint-using-documentation
      url: "https://portswigger.net/web-security/learning-paths/api-testing/api-testing-identifying-and-interacting-with-api-endpoints/api-testing/lab-exploiting-unused-api-endpoint"
---  

**Задание: To solve the lab, find the exposed API documentation and delete carlos. You can log in to your own account using the following credentials: wiener:peter. (Удалить user Carlos)**  
В самом начале мы видим вот такой сайт
<img width="1919" height="938" alt="image" src="https://github.com/user-attachments/assets/6ca33b69-6284-4fcc-ab57-7377d8833829" />  
максимально стараемся просмотреть всё и нажать на все кнопки  
Заходим в burpsuite target sitemap. Здесь мы можем увидеть api/user...   
<img width="331" height="393" alt="image" src="https://github.com/user-attachments/assets/6fdf34d6-791d-4bdd-83d5-e219c9841066" />  
Правой кнопкой мыши -> Send To Repeater  
Затем пробуем нащупать ендпойнт с методом GET, в данном случае ендпойнт - /api/  
И мы видим:

|  Verb  |	        Endpoint         |	Parameters        |	Response       |
|:------:|:-------------------------:|:------------------:|:--------------:|
|  GET   | /user/[username: String]  |	{ }               |	200 OK, User   |
| DELETE |	/user/[username: String] |	{ }               |	200 OK, Result |
| PATCH  |	/user/[username: String] |	{"email": String}	| 200 OK, User   |  

Как мы видим, для того, чтобы удалить пользователя, нужно применить метод DELETE, так и сделаем :D  
Получает ответ:  
<img width="612" height="251" alt="image" src="https://github.com/user-attachments/assets/985733a6-9d9d-4e65-870a-ab1f0629e132" />  
Лаба выполнена!)
