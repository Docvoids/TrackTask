#  Завдання 1 — Передача query-параметрів

##  Яке завдання

Є URL виду:

```
site.com/?utm_source=facebook&utm_campaign=test&utm_content=ad1
```

Потрібно:
Спочатку зчитати всі параметри які знаходяться в посиланні, після цього передати їх на інший домен (`https://offer.com`)

---

## Для чого це використовується

Це використовується для:

* передачі UTM-міток між сайтами
* трекінгу реклами
* збереження інформації про джерело трафіку

---

## Як це працює

1. Беремо параметри з URL:

```
window.location.search
```

2. Додаємо їх до нового домену:

```
https://offer.com + params
```

3. Робимо редірект

---

## Код

```html
<script>
function redirectWithParams() {
    const params = window.location.search; //взяли параметри
    const newUrl = "https://offer.com" + params; //створили нове посилання
    window.location.href = newUrl; // перейшли по ньому
}
</script>
```

---

##  Як запустити цей приклад

### Запустити сервер:

```
python -m http.server 3000
```
Тут приклад на пайтоні, можна адаптувати під будь яку іншу мову яка має відповідний інструментарій.
### Відкрити:

```
http://localhost:3000/index.html?utm_source=facebook
```

---

# Завдання 2 — Збереження параметрів + postback

## Що потрібно зробити

1. Користувач заходить з параметрами:

```
/landing.html?utm_source=facebook
```

2. Переходить на:

```
/success.html
```

3. Потрібно:

* зберегти параметри
* відправити запит:

```
https://tracker.com/postback?status=lead&source=facebook
```

---

## Навіщо це потрібно

Це основа трекінгу в рекламі:

* визначити звідки прийшов користувач
* зафіксувати конверсію
* передати дані в аналітику

---

## Як це працює

### 1. Landing сторінка

* читаємо параметри
* зберігаємо в `localStorage`

```js
const params = window.location.search;
localStorage.setItem("utm_params", params);
```

---

### 2. Success сторінка

* дістаємо параметри
* парсимо
* формуємо URL
* відправляємо запит

```js
const saved = localStorage.getItem("utm_params");
const urlParams = new URLSearchParams(saved);

const source = urlParams.get("utm_source");

const url = `https://tracker.com/postback?status=lead&source=${source}`;

fetch(url, { method: "GET", mode: "no-cors" });
```

---

## Що таке Postback

**Postback** — це HTTP-запит, який повідомляє систему про подію.

 У цьому випадку:

* `status=lead` → була конверсія
* `source=facebook` → джерело

---

## Для чого використовується

* трекінг реклами
* CPA/affiliate системи
* аналітика
* оптимізація кампаній

---

## Як запустити

### 1. Створити файли

* landing.html
* success.html

---

### 2. Запустити сервер

```
python -m http.server 3000
```

---

### 3. Відкрити

```
http://localhost:3000/landing.html?utm_source=facebook
```

---

### 4. Перейти на success

* натиснути кнопку
* відкриється `/success.html`
* відправиться postback

---
