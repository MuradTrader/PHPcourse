### Создание ссылок с параметрами в PHP: `http_build_query()`

**Проблема:** При ручном добавлении параметров в URL возникают ошибки из-за спецсимволов.

---

#### ❌ Проблемный подход (ручное формирование)

```php
<!-- Ссылка с параметром без кодирования -->
<a href="query_string.php?book=Harry Potter">Harry Potter</a>

<!-- Проблемная ссылка с символом & -->
<a href="query_string.php?book=Beauty & the Beast">Beauty & the Beast</a>
```

**Результат в браузере для второй ссылки:**

```url
query_string.php?book=Beauty & the Beast
```

**Ошибка в `$_GET`:**

```php
print_r($_GET);
/* Вывод:
Array (
    [book] => "Beauty "
    [the] => "Beast"
    [0] => "" // Пустой параметр
) */
```

**Причина:**  
Символ `&` интерпретируется как разделитель параметров, а пробелы портят значения.

---

#### ✅ Правильное решение: `http_build_query()`

```php
<?php
// Для одной книги
$harryPotter = http_build_query(['book' => 'Harry Potter']);
echo "<a href='query_string.php?$harryPotter'>Harry Potter</a>";

// Для книги с символом &
$beautyBeast = http_build_query(['book' => 'Beauty & the Beast']);
echo "<a href='query_string.php?$beautyBeast'>Beauty & the Beast</a>";
```

**Результат в HTML:**

```html
<!-- Пробелы заменены на + -->
<a href="query_string.php?book=Harry+Potter">Harry Potter</a>

<!-- Символ & закодирован как %26 -->
<a href="query_string.php?book=Beauty+%26+the+Beast">Beauty & the Beast</a>
```

**Корректный вывод `$_GET`:**

```php
Array (
    [book] => "Harry Potter" // Верное значение
)

Array (
    [book] => "Beauty & the Beast" // Верное значение
)
```

---

#### ✨ Пример с несколькими параметрами

```php
<?php
$params = [
    'book'   => 'Beauty & the Beast',
    'author' => 'Disney',
    'year'   => 1991
];

$query = http_build_query($params);
echo "<a href='details.php?$query'>Подробнее</a>";
```

**Сгенерированный URL:**

```
details.php?book=Beauty+%26+the+Beast&author=Disney&year=1991
```

---

#### 📌 Best Practices

1. **Всегда используйте `http_build_query()`** для:
   - Автоматического кодирования спецсимволов (` ` → `+`, `&` → `%26`).
   - Корректной обработки значений в `$_GET`.
2. **Не формируйте query string вручную** – это приводит к:
   - Ошибкам разбора параметров.
   - Некорректной работе в разных браузерах.
3. **Преимущества функции:**
   - Соответствие стандартам URL (RFC 3986).
   - Поддержка массивов и сложных структур.
   - Защита от случайных ошибок с разделителями.

> **Ключевой вывод:** `http_build_query()` гарантирует, что параметры URL будут правильно интерпретированы PHP и браузерами независимо от спецсимволов.
