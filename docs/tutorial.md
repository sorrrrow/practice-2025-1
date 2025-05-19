# Туториал: Как создать простой HTTP-сервер с веб-интерфейсом на Python

## Что мы будем делать
В этом руководстве ты создашь:
- HTTP-сервер на Python с нуля (без фреймворков)
- HTML-сайт про кота по имени Персик
- Мини-игру "Котозмейка"

---

## 1. Структура проекта
Создай следующую структуру:
```
папка-проекта/
├── webserver.py
├── index.html
├── about.html
├── game.html
├── js/
│ └── cat_snake.js
├── css/
│ └── style.css
├── images/
│ └── IMG_....JPG, cat.png, olive.png, persik_bg.jpg
```

## 2. Создание сервера
В `webserver.py`:

```python
import socket, os, threading

def handle_client(conn, addr):
    request = conn.recv(1024).decode()
    path = request.split(' ')[1]
    if path == "/": path = "/index.html"
    file_path = "www" + path

    if os.path.isfile(file_path):
        with open(file_path, 'rb') as f:
            body = f.read()
        header = "HTTP/1.1 200 OK\r\n"
    else:
        body = b"<h1>404 Not Found</h1>"
        header = "HTTP/1.1 404 Not Found\r\n"

    conn.sendall((header + "\r\n").encode() + body)
    conn.close()

server = socket.socket()
server.bind(("127.0.0.1", 8080))
server.listen(5)
print("Server started on http://127.0.0.1:8080")

while True:
    conn, addr = server.accept()
    threading.Thread(target=handle_client, args=(conn, addr)).start()
```

## 3. HTML-страницы

```index.html
<h1>Это Персик!</h1>
<p>Кот с характером, обожает маслины и кофе.</p>
<a href="/about.html">Узнать больше</a>

```

## 4. Мини-игра "Котозмейка"

```game.html
<canvas id="gameCanvas"></canvas>
<p id="score">Очки: 0</p>
<div id="game-message" class="hidden">
  <p id="message-text"></p>
  <button onclick="restartGame()">Играть заново</button>
</div>
```

### cat_snake.js
- Управление стрелками
- Счётчик очков
- Победа при 20 очках
- Проигрыш при столкновении
- Отображение сообщения

## 5. Стилизация

```style.css
body {
  background: url("../images/persik_bg.jpg") center/cover;
  font-family: 'Fredoka', sans-serif;
  color: #fff;
}

.main-container {
  background: rgba(255,255,255,0.1);
  padding: 20px;
  border-radius: 16px;
}
```
## 6. Тестирование

- Запусти сервер
- Перейди в браузере на http://127.0.0.1:8080
- Проверь работу всех страниц и игры
- Попробуй победить или проиграть в змейке
