---
title: '[WEB] Lab: Finding and exploiting an unused API endpoint'
date: 2025-12-04T01:20:00+03:00
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
    - label: "Lab PortSwigger"
      url: "https://portswigger.net/web-security/learning-paths/api-testing/api-testing-identifying-and-interacting-with-api-endpoints/api-testing/lab-exploiting-unused-api-endpoint"
---  
  
**Задание: To solve the lab, exploit a hidden API endpoint to buy a Lightweight l33t Leather Jacket. You can log in to your own account using the following credentials: wiener:peter. (Нужно найти скрытый эндпоинт для покупки Lightweight l33t Leather Jacket)**  
Заходя на сайт, мы видим обычный магазин  
<img width="1919" height="944" alt="image" src="https://github.com/user-attachments/assets/4b400285-5a18-460b-a884-6646d5371a21" />  
Вставляем ссылку в браузер burp suit и начинаем сильно лазить по сайту, нажимать на все кнопки на сайте, исследовать его. Входим в учетную запись wiener:peter.  
<img width="323" height="583" alt="image" src="https://github.com/user-attachments/assets/2c3c03f4-9283-40d9-bd2a-4f99278dd691" />  
Правой кнопкой мыши -> Send to Repeater.  
Запрос PATCH является набором инструкций о том, как изменить ресурс. Поэтому заменяем GET в запросе на PATCH и получаем ответ: 
```json
{"type":"ClientError","code":400,"error":"Only 'application/json' Content-Type is supported"}
```
Теперь исходя из этого ответа меняем Content-Type:  
**Content-Type: application/json**  
А раз мы поменяли Content-Type на тип json, мы добавляем в тело **{}**.  
Получаем ответ: 
```json
{"type":"ClientError","code":400,"error":"'price' parameter missing in body"}
```
Изменяем наше тело запроса на  
```json
{
"price":0
}
```
И получаем ответ:  
```json
{"price":"$0.00"}
```
Теперь заходим в корзину, покупаем товар, цену которого мы сделали на 0 и вуаля - **Лаба пройдена :D**
