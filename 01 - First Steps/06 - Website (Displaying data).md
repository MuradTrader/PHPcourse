## Подробный разбор урока: Динамические заголовки, фоновые изображения и добавление новой страницы

### Основные концепции урока:

1. **Динамическое управление контентом** через переменные PHP
2. **Переиспользование компонентов** с помощью include
3. **Структура проекта** и работа с путями
4. **Практическое применение** в реальном проекте

---

### 1. Динамическое изменение заголовка страницы

#### Проблема:

- Заголовок страницы (`<title>`) жёстко закодирован в `header.inc.php`
- На всех страницах отображается "Our Restaurant • Our Mission"

#### Решение:

1. В каждом файле страницы **перед включением header** определяем переменную:

```php
// ingredients.php
<?php
$pageTitle = "Ingredients"; // Устанавливаем значение
include 'inc/header.inc.php';
?>
```

2. В `header.inc.php` заменяем статичный текст на переменную:

```php
<!-- header.inc.php -->
<title>Our Restaurant • <?php echo $pageTitle; ?></title>
```

#### Механизм работы:

- Переменные PHP сохраняются в **глобальной области видимости**
- Включаемые файлы имеют доступ к переменным текущего контекста
- При переходе на страницу без $pageTitle возникает ошибка - необходимо определить переменную во всех страницах

---

### 2. Динамическое фоновое изображение

#### Проблема:

- Фоновое изображение жёстко задано в CSS внутри `header.inc.php`
- Нужны разные изображения для разных страниц

#### Решение:

1. Добавляем новую переменную в файлах страниц:

```php
// mission.php
<?php
$pageTitle = "Our Mission";
$headerImg = "images/mission-bg.jpg"; // Путь к изображению
include 'inc/header.inc.php';
?>
```

2. Модифицируем CSS в `header.inc.php`:

```php
<style>
.header {
  background-image: url('<?php echo $headerImg; ?>');
}
</style>
```

#### Важное замечание о путях:

- **Браузер интерпретирует пути относительно текущего URL**
- Путь указывается относительно корня сайта, а не расположения PHP-файла
- Пример корректного пути: `images/wood-fire.jpg`

---

### 3. Структура проекта (как должно быть организовано)

```
project-root/
├── images/
│   ├── mission-bg.jpg
│   ├── ingredients-bg.jpg
│   └── menu-bg.jpg
├── inc/
│   ├── header.inc.php
│   ├── nav.inc.php
│   └── footer.inc.php
├── ingredients.php
├── mission.php
└── menu.php (новая страница)
```

---

### 4. Преимущества использованного подхода

1. **Единая точка изменения:**

   - Изменения в шапке/подвале делаются в одном месте
   - Автоматически применяются ко всем страницам

2. **Консистентность дизайна:**

   - Все страницы имеют одинаковую структуру
   - Навигация всегда актуальна

3. **Эффективная разработка:**

   - Новые страницы создаются за минуты
   - Минимизация дублирования кода

4. **Гибкость:**
   - Легко добавлять новые переменные для кастомизации
   - Возможность расширения функционала

---

### Ключевые выводы:

1. Переменные PHP + include = мощный инструмент для управления контентом
2. Правильная организация файлов - основа поддерживаемости проекта
3. Пути всегда указываются относительно корня сайта, а не файловой структуры PHP
4. Динамическая генерация контента - основное преимущество PHP перед статичным HTML
