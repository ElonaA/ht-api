# 📋 Завдання до модуля 8 "Робота з API"

## 📄 Опис завдання:

Необхідно налаштувати інтеграцію для передачі даних з форми на лендінгу на сервер Партнерської Програми AffScale через наданий API. Для лендінгу, який ми зробили у попередньому модулі.

## 🔧 Технічні вимоги:

### API для передачі даних з лендінгу:

- **Endpoint:** `https://tracking.affscalecpa.com/api/v2/affiliate/leads?api-key=adsbdb45dhnjcbd4567ghjdd`
- **Метод:** `POST`
- **Заголовки:** `'Content-Type: application/json'`
- **Параметри:**
  - `goal_id` (required) - Offer goal_id з AffScale
  - `aff_click_id` (required) - Affiliate Click ID
  - `firstname` (required) - Ім'я ліда
  - `phone` (required) - Номер телефону
  - `sub_id1` (required) - Media Buyer ID/Traffic Source ID
  - `custom1` (optional) - Номер телефону 2
  - `sub_id2` до `sub_id5` (optional) - Можуть використовуватися для ваших особистих цілей

    
### Приклад коду обробника:
```php
<?php

if (!empty($_POST['phone'])) {
    send_the_order ($_POST);
}

function send_the_order ($post){
    $params=array(
        'flow_hash' => "qQSS",
        'referrer' => $ref,
        'phone' => $post['phone'],
        'name' => $post['name'],
        'country' => "ES",
        'sub1' => $post['sub1'],
        'sub2' => $post['sub2'],
        'sub3' => $post['sub3'],

    );

    $url = 'http://wapi.leadbit.com/api/pub/new-order/_66279fccd3b10256089676';
    $ch = curl_init();
    curl_setopt($ch, CURLOPT_URL, $url);
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
    curl_setopt($ch, CURLOPT_FOLLOWLOCATION, 1);
    curl_setopt($ch, CURLOPT_USERAGENT, $_SERVER['HTTP_USER_AGENT']);
    curl_setopt($ch, CURLOPT_REFERER, $url);
    curl_setopt($ch, CURLOPT_POST, 1);
    curl_setopt($ch, CURLOPT_POSTFIELDS, $params);
    $return= curl_exec($ch);
    curl_close($ch);

    $array=json_decode($return, true);

}
?>
```

### Формат даних відправляємих данних:

> [!NOTE]
> Дані повинні передаватися у форматі JSON. Спробуйте 2 варінти, коли ми передаємо `статичні дані` та погратися з інпутами для передачі `динамічних даних`.


**Статичні дані для відправки з форми:**

```json
{
  "goal_id": 83,
  "sub_id1": "traff",
  "sub_id2": "fb",
  "aff_click_id": "156484efwe4re98b4rev4wr84"
}
```

## ✏️ Зміни на лендінгу:

> [!NOTE]
> Берем ленд, який у нас вийшов після минулого домашнього завдання 7, та налаштовуємо до нього передачу лідів.

1. Створити файл `api.php` та реалізувати функцію відправки даних з форми на сервер AffScale через розроблений API обробник.
2. Ім'я та телефон мають динамічно відправлятися з відповідних інпутів форми.
4. Створити сторінку `success.php`, на яку має перекидувати після успішної відправки форми.
5. Додати на сторінку `success.php` код пікселя, айді якого має передаватися з обробника на сторінку.

> [!CAUTION]
> У вас буде невалідний `api key`, тому перевіряти саму відправку по API вам не треба. Тільки написати код, який на ваши думку має `100% працювати`.


### 📊 FB Pixel 
Додати код пікселя на сторінку "Дякуємо" `success.php`. Приклад коду пікселя:

```html
<script>
  (function(f,b,e,v,n,t,s) {
    if(f.fbq)return; n=f.fbq=function() {
      n.callMethod ? n.callMethod.apply(n, arguments) : n.queue.push(arguments)
    };
    if(!f._fbq)f._fbq=n; n.push=n; n.loaded=!0; n.version='2.0';
    n.queue=[]; t=b.createElement(e); t.async=!0;
    t.src=v; s=b.getElementsByTagName(e)[0];
    s.parentNode.insertBefore(t,s)
  })(window, document,'script','https://connect.facebook.net/en_US/fbevents.js');
  fbq('init', '111111111111');
  fbq('track', 'PageView');
</script>
<noscript>
  <img height="1" width="1" style="display:none"
       src="https://www.facebook.com/tr?id=111111111111&ev=PageView&noscript=1"/>
</noscript>
```

> [!IMPORTANT]
> Ідентифікатор `id` пікселя має передаватися з форми замовлення лендінгу через прихований інпут `name="pixel"`. Івент/Подія має бути `Lead`. Спробуйте передавати статично, та динамічний варінт також.


## ⚠️ Обробка помилок:
Розробити обробку помилок з відображенням користувачу повідомлень про проблеми (наприклад, некоректні дані, не введено обов'язкові поля). Маска як додатковий спосіб валідації буде доречна.


## 💡 Додаткове завдання (необов'язкове, для PRO-рівня):
- Реалізувати маску для номера телефону під гео Мексика.
> [!NOTE]
> Маска для номера телефону Мексика: +52-ХXXXXXXXXX (10 цифр після коду країни).

- Написати скрипт для заборони відправки дубльованих замовлень (відправляти повторно дані тільки якщо пройшло більше 5 хвилин з останньої відправки).
- Реалізувати анімацію під час відправки даних (наприклад, спіннер).

### ⚠️ Вимоги до виконання домашнього завдання:
1. Взяти ленд з минулого домашнього завдання та налаштувати апі.
2. Виконане домашнє завдання запакувати у архів та відправити на перевірку.
3. Термін виконання домашнього завдання `5 днів`.

Бажаємо успіху! 🚀
