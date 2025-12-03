---
title: '[WEB] "Javascript - Obfuscation 3"'
date: 2025-12-03T15:25:00+03:00
categories:
  - Lab
tags:
  - web
excerpt: " "
header:
  overlay_image: https://apprendre-avec-le-jeu.com/wp-content/uploads/2024/03/Root-me-Logo.png
  overlay_filter: 0.4
  actions:
    - label: "Lab Root-Me"
      url: "https://www.root-me.org/fr/Challenges/Web-Client/Javascript-Obfuscation-3"
---
**Описание: Полезный или бесполезный, вот в чем вопрос...**
Заходя на сайт, просмотрю сразу исходный код. Вижу скрипт js.  
<img width="1919" height="954" alt="image" src="https://github.com/user-attachments/assets/26bff843-44c8-42bd-a304-e9c0c83a7581" />  
Для деобфуксации вставлю скрипт на сайт **https://deobfuscate.io/**. Он вывел результат:   
```js
function dechiffre(pass_enc) {
  var pass = "70,65,85,88,32,80,65,83,83,87,79,82,68,32,72,65,72,65";
  var tab = pass_enc.split(",");
  var tab2 = pass.split(",");
  var i, j, k, l = 0, m, n, o, p = "";
  i = 0;
  j = tab.length;
  k = j + l + (n = 0);
  n = tab2.length;
  for (i = o = 0; i < (k = j = n); i++) {
    o = tab[i - l];
    p += String.fromCharCode(o = tab2[i]);
    if (i == 5) break;
  }
  for (i = o = 0; i < (k = j = n); i++) {
    o = tab[i - l];
    if (i > 5 && i < k - 1) p += String.fromCharCode(o = tab2[i]);
  }
  p += String.fromCharCode(tab2[17]);
  pass = p;
  return pass;
}
String.fromCharCode(dechiffre("55,56,54,79,115,69,114,116,107,49,50"));
h = window.prompt("Entrez le mot de passe / Enter password");
alert(dechiffre(h));
```
Контрольная строка "55,56,54,79,115,69,114,116,107,49,50" представляет собой массив ASCII-кодов, которые, будучи переведенными в символы, дают нужный пароль.  
```Флаг: 786OsErtk12```
