# Создаем свой Kanban-борд на HTML, CSS и JavaScript: Проще простого!

Привет, друзья! Сегодня мы с вами сделаем простую, но функциональную Kanban-доску прямо в браузере. Никаких сложных фреймворков, только чистый HTML, CSS и немного JavaScript. Это отличный способ понять, как работают веб-приложения и как можно организовать свои задачи наглядно.

**Что такое Kanban и зачем он нужен?**

Kanban – это метод управления задачами, который помогает визуализировать рабочий процесс. Представьте себе доску, разделенную на колонки: "Надо сделать", "В процессе" и "Готово". Вы берете карточку с задачей и перемещаете ее из одной колонки в другую, пока она не будет выполнена. Просто и эффективно!

**Шаг 1: HTML – Создаем структуру**

Первым делом нам нужно создать HTML-файл (например, `index.html`), который будет основой нашей доски. Вот его содержимое:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kanban Board</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <button id="theme-toggle">Переключить тему</button>
    <div class="container">
        <div class="column" id="todo" ondrop="drop(event)" ondragover="allowDrop(event)">
            <h2>Надо сделать</h2>
            <button onclick="addCard('todo')">Добавить карточку</button>
        </div>
        <div class="column" id="inprogress" ondrop="drop(event)" ondragover="allowDrop(event)">
            <h2>В процессе</h2>
            <button onclick="addCard('inprogress')">Добавить карточку</button>
        </div>
        <div class="column" id="done" ondrop="drop(event)" ondragover="allowDrop(event)">
            <h2>Готово</h2>
            <button onclick="addCard('done')">Добавить карточку</button>
        </div>
    </div>
    <script src="script.js"></script>
</body>
</html>
```

Что здесь происходит:

*   `<head>`: Здесь мы подключаем стили (`style.css`) и задаем заголовок страницы.
*   `<body>`: Основное содержимое страницы.
    *   `<button id="theme-toggle">`: Кнопка для переключения темной темы.
    *   `<div class="container">`: Контейнер для наших колонок.
    *   `<div class="column">`: Каждая колонка представляет собой отдельный блок.
        *   `<h2>`: Заголовок колонки (например, "Надо сделать").
        *   `<button onclick="addCard('todo')">`: Кнопка для добавления карточки в эту колонку.
        *   `ondrop="drop(event)" ondragover="allowDrop(event)"`: Атрибуты, необходимые для реализации Drag and Drop.
    *   `<script src="script.js">`: Подключаем наш JavaScript-файл.

**Шаг 2: CSS – Придаем красоту**

Теперь создадим файл `style.css` и добавим немного стилей, чтобы наша доска выглядела прилично:

```css
body {
    font-family: Arial, sans-serif;
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    align-items: center;
    min-height: 100vh;
    margin: 0;
    background-color: #f4f4f4;
    transition: background-color 0.3s ease;
}

body.dark-mode {
    background-color: #333;
    color: #fff;
}

.container {
    display: flex;
    width: 80%;
    height: 80%;
    background-color: #fff;
    border-radius: 10px;
    overflow: hidden;
    margin-top: 20px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    transition: background-color 0.3s ease;
}

body.dark-mode .container {
    background-color: #444;
    box-shadow: 0 4px 8px rgba(255, 255, 255, 0.1);
}

.column {
    flex: 1;
    padding: 20px;
    border-right: 1px solid #ddd;
    overflow-y: auto;
    transition: border-color 0.3s ease;
}

body.dark-mode .column {
    border-color: #666;
}

.column:last-child {
    border-right: none;
}

.column h2 {
    text-align: center;
    margin-bottom: 20px;
    color: #333;
    transition: color 0.3s ease;
}

body.dark-mode .column h2 {
    color: #fff;
}

.card {
    background-color: #f9f9f9;
    border: 1px solid #ddd;
    padding: 10px;
    margin-bottom: 10px;
    border-radius: 5px;
    cursor: grab;
    transition: background-color 0.3s ease, border-color 0.3s ease;
}

body.dark-mode .card {
    background-color: #555;
    border-color: #777;
    color: #fff;
}

.card:active {
    cursor: grabbing;
}

button {
    display: block;
    margin: 10px auto;
    padding: 8px 16px;
    background-color: #5cb85c;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    transition: background-color 0.3s ease;
}

body.dark-mode button {
    background-color: #777;
}

button:hover {
    background-color: #4cae4c;
}

body.dark-mode button:hover {
    background-color: #666;
}

#theme-toggle {
    position: fixed;
    top: 10px;
    right: 10px;
    padding: 8px 16px;
    background-color: #007bff;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    transition: background-color 0.3s ease;
}

#theme-toggle:hover {
    background-color: #0056b3;
}

body.dark-mode #theme-toggle {
    background-color: #222;
    color: #fff;
}

body.dark-mode #theme-toggle:hover {
    background-color: #555;
}

.delete-button {
    background-color: #d9534f;
    color: white;
    border: none;
    padding: 5px 10px;
    border-radius: 4px;
    cursor: pointer;
    margin-left: 5px;
    transition: background-color 0.3s ease;
}

.delete-button:hover {
    background-color: #c9302c;
}

body.dark-mode .delete-button {
    background-color: #999;
}

body.dark-mode .delete-button:hover {
    background-color: #888;
}
```

Здесь мы задаем:

*   Шрифт и цвет фона для всего сайта.
*   Стили для контейнера, колонок и карточек.
*   Цвета для темной темы (класс `.dark-mode`).
*   Стили для кнопок.

**Шаг 3: JavaScript – Оживляем доску**

Теперь создадим файл `script.js` и добавим JavaScript-код, который будет отвечать за добавление карточек, Drag and Drop и переключение темы:

```javascript
let cardIdCounter = 0;

function addCard(columnId) {
    const column = document.getElementById(columnId);
    const card = document.createElement('div');
    card.classList.add('card');
    card.draggable = true;
    card.id = 'card-' + cardIdCounter++;

    // Color picker
    const colorPicker = document.createElement('input');
    colorPicker.type = 'color';
    colorPicker.value = '#f9f9f9'; // Default card color
    colorPicker.style.marginRight = '5px';
    colorPicker.addEventListener('change', function(event) {
        card.style.backgroundColor = event.target.value;
    });
    card.appendChild(colorPicker);

    const cardText = document.createElement('span');
    cardText.textContent = 'Новая карточка';
    cardText.addEventListener('click', function() {
        const newText = prompt('Введите название карточки:', cardText.textContent);
        if (newText) {
            cardText.textContent = newText;
        }
    });
    card.appendChild(cardText);

    const deleteButton = document.createElement('button');
    deleteButton.classList.add('delete-button');
    deleteButton.textContent = 'Удалить';
    deleteButton.onclick = function() {
        card.remove();
    };
    card.appendChild(deleteButton);

    card.addEventListener('dragstart', dragStart);
    column.appendChild(card);
}

function allowDrop(event) {
    event.preventDefault();
}

function dragStart(event) {
    event.dataTransfer.setData('text/plain', event.target.id);
}

function drop(event) {
    event.preventDefault();
    const cardId = event.dataTransfer.getData('text/plain');
    const card = document.getElementById(cardId);
    const column = event.target;

    if (column.classList.contains('column')) {
        column.appendChild(card);
    }
}

document.getElementById('theme-toggle').addEventListener('click', function() {
    document.body.classList.toggle('dark-mode');
});
```

Разберем код:

*   `addCard(columnId)`: Функция, которая добавляет новую карточку в указанную колонку.
    *   Создаем элементы `div` (карточка), `input` (выбор цвета), `span` (текст карточки) и `button` (удаление).
    *   Добавляем обработчики событий для изменения цвета, редактирования текста и удаления карточки.
    *   Делаем карточку перетаскиваемой (`draggable = true`).
*   `allowDrop(event)`: Разрешает перетаскивание элемента над колонкой.
*   `dragStart(event)`: Запоминает ID перетаскиваемой карточки.
*   `drop(event)`: Перемещает карточку в колонку, над которой ее отпустили.
*   `document.getElementById('theme-toggle').addEventListener('click', ...)`:  Переключает темную тему при нажатии на кнопку.

**Как это работает?**

1.  Откройте `index.html` в браузере.
2.  Нажимайте на кнопки "Добавить карточку" в каждой колонке, чтобы создать новые карточки.
3.  Кликайте на текст карточки, чтобы изменить ее название.
4.  Используйте Drag and Drop, чтобы перемещать карточки между колонками.
5.  Нажимайте на кнопку "Удалить", чтобы удалить карточку.
6.  Нажимайте на кнопку "Переключить тему", чтобы изменить тему страницы.

**Заключение**

Вот и все! Мы создали простую Kanban-доску с базовыми функциями. Конечно, это только начало. Вы можете добавить больше возможностей, например:

*   Сохранение данных в LocalStorage, чтобы доска не сбрасывалась при перезагрузке страницы.
*   Редактирование описания карточки.
*   Назначение приоритетов задачам.
*   И многое другое!

Надеюсь, эта статья была полезной и вдохновила вас на создание собственных веб-приложений. Удачи!
