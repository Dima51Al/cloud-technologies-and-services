# Лабораторная работа №1 по DevOps "Nginx" (обычная)

## Задача

Настроить nginx согласно тз (дальнейшие подробности смотреть [здесь](https://docs.google.com/document/d/1jJaKob932N1-91rPy8G_XkyAtOArJe8MCoyJh9iA7lE/edit?tab=t.0)).

## В процессе работы..

### Установка

Пожалуй, это была самая лёгкая часть данного задания. Тем не менее, ей предшествовала настройка wsl, а также некоторое время и огромные моральные усилия, чтобы принять тот факт, что, увы, снова придётся взаимодействовать с linux.

```bash
    sudo apt update
    sudo apt install nginx -y
    nginx -v
    sudo systemctl start nginx
    sudo systemctl enable nginx
    sudo systemctl status nginx
```

### Начало, середина и конец

Пропустим момент создания прообного сайта, на котором было знакомство с возможностями nginx. Два других сайта, выступающих в качестве пет-проектов, были сделаны аналогично.

```bash
  sudo mkdir -p /var/www/firstproject/html // создаём папочку
  sudo mkdir -p /var/www/secondproject/html // ещё одну
  sudo nano /var/www/secondproject/html/index.html // создаём html
  sudo nano /var/www/firstproject/html/index.html // аналогично
```

Для примера представим код второго сайта (они почти ничем не отличаются):

```bash
    <!DOCTYPE html>
    <html>
    <head>
        <title>That is my second site!</title>
    </head>
    <body>
        <h3>That's my second site!</h3>
        <p>happy weekend</p>
    </body>
    </html>
```

Конечно, мы собирались создать два миленьких и красивых сайта с картинками и тп, однако внезапно стало очень лень, и что есть - то есть.

Вот это тоже необходимо, чтобы они захотели запускаться:

```bash
  sudo chmod -R 755 /var/www/firstproject
  sudo chmod -R 755 /var/www/secondproject
  sudo chmod -R 644 /var/www/firstproject/html/*
  sudo chmod -R 644 /var/www/secondproject/html/*
```

Далее перейдём к самой весёлой части нашего мероприятия, а именно к nginx конфигу.

```bash
    sudo nano /etc/nginx/sites-available/firstproject
    sudo nano /etc/nginx/sites-available/secondproject
```

Пример сервера второго проекта:

```bash
    server {
        listen 80;
        listen [::]:80;
        server_name secondproject.localhost;
        return 301 https://$host$request_uri;
    }
    server {
        listen 443 ssl;
        listen [::]:443 ssl;
        server_name secondproject.localhost;
    
        ssl_certificate     /etc/nginx/ssl/selfsigned.crt;
        ssl_certificate_key /etc/nginx/ssl/selfsigned.key;
    
        root /var/www/secondproject/html;
        index index.html index.php;
    
        location /static/ {
            alias /var/www/secondproject/assets/;
        }
    
        location / {
            try_files $uri $uri/ =404;
        }
```

Попытаюсь пояснить за это всё..
У нас два сервера, потому что с первого с протоколом http (котороый с ```listen 80```) перенаправляется на второй https (```listen 443 ssl```), и он уже чуть-чуть другой. Первый нам не нравится тем, что он незащищённый, из-за чего мы и идём на второй, который псевдозащищённый (у него самоподписанный сертификат, на который ругается мой Kaspersky).

Также из интересного (и необходимого) у нас есть сертификат, потому что, оказывается, чтобы использовать https нужно доказать, что твой сайт "хороший" и как будто бы кем-то (именно кем-то) проверенный.

```bash
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048   -keyout /srv/nginx-docker/ssl/selfsigned.key   -out /srv/nginx-docker/ssl/selfsigned.crt   -subj "/CN=localhost"
```
гененируем сертификат для localhost



```bash
    ssl_certificate     /etc/nginx/ssl/selfsigned.crt; // ссылка на расположение сертификата
    ssl_certificate_key /etc/nginx/ssl/selfsigned.key; // ссылка на ключ
```

Для создания псевдонимов путей мы использовали alias.

Кроме того, мы редактировали файл ```/etc/hosts```, чтобы по одному адресу располагались два разных сайта.

```
127.0.0.1 forge1.local
127.0.0.1 forge2.local
```

Напоследок вставляю скрины этих шедевров:

<img width="2489" height="1295" alt="image" src="https://github.com/user-attachments/assets/fca2d3ff-99e1-4eab-8bb7-2e7e45d50ac7" />

<img width="2510" height="1207" alt="image" src="https://github.com/user-attachments/assets/47cc1452-0af3-4b92-89fe-01a283e710f4" />


Конечно! Вот аккуратно отформатированная версия вашего текста в Markdown с сохранением смысла и структуры:


---

P.S. Этот отчёт был написан участником команды, который почти всё время ломал что-то и не всегда понимал, что делать. Поэтому, пожалуйста, не пинайте меня (обращение в том числе и к напарнику).

---

P.S. P.S. Далее пишет второй участник проекта:

У меня (Димы) то же самое, что и у Ксении, но я не так мучался с названиями и детализацией HTML, поэтому процесс в целом шёл намного быстрее. Большую часть времени помогал Ксение.  

Для меня эта лабораторная работа была интересна, поскольку я развертывал Nginx на спешно созданном из старого ПК сервере.  
Помимо всего, я настроил Nginx + Flask, и теперь по запросу на `forge1.local` я получал ответ.  

Особенностью этой лабораторной работы для меня также была настройка Windows hosts. Необходимо было дописать в файл `hosts`:

```

192.168.0.100 forge1.local
192.168.0.100 forge2.local

```

Это мои названия для страниц.  

Теперь схема выглядит так:  

```

forge1.local -> 192.168.0.100 -> forge1.local -> localhost (который прописан при генерации ключа)

```

Среди папок проекта выгружены именно мои папки, файлы и конфиги. Именно поэтому названия сайтов, страниц и папок могут немного различаться.

```
PS C:\Users\User> sftp dima@192.168.0.100
dima@192.168.0.100's password:
Connected to 192.168.0.100.
sftp> get -r /etc/nginx C:\nginx_backup
```
