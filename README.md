chat_deploy
===========
Приложение развёртывается на virtial host под управлением ubuntu 13.4 apache passenger postgresql
Перед развёртыванием :
  1. установка rvm если не установлен
  2. установка bundle если не установлен
  3. установка rails если не установлен
  4. установка postgres если не установлен
    - добавили пользователь базы с правами суперпользователь.
  5. установка apache2 если не установлен
  6. установка passenger если не установлен
  7. установка passenger-install-apache2-module если не установлен
  8. Создал новый файл с конфигурацией виртуального хоста /etc/apache2/sites-available/chat.conf
  9. Подключил его sudo a2ensite chat
  10. Т.к virtual host правил /etc/hosts
  
Создаём приложения в <social>.app для vkontakte facebook google
Скачиваем наше приложение с git@github.com:dpblh/chat.git

Устанавливаем наши secret keys из <social>.app в файлы config/oauth.yml и shared/oauth.yml
Добавиляем нашего пользователь базы и пароль в файлы config/database.yml и shared/database.yml
Конфигурируем private_pub в файлах config/private_pub.yml и shared/private_pub.yml в продакшн ставим server внешний адрес 
и генерируем secret_token желательно по длиньше
Добавиляем в файлы config/secrets.yml и shared/secrets.yml secret_key_base тоже по длиньше
Правим deploy/production.rb
  role :app, %w{} прописываем реальные user и host через к которым мы будем ломиться для деплоя
  role :web, %w{} прописываем реальные user и host через к которым мы будем ломиться для деплоя
  role :db,  %w{} прописываем реальные user и host через к которым мы будем ломиться для деплоя
  server '', user: '' host user
  password: '' пароль под от user. По причине не приодолимых препятствий rsa ключ у меня не заработал.
  у вас ели деплоится на реальную машину все должно быть ок. при условии что удаленная машина доверяет нам
Правим config/deploy.rb устанавливаем путь куда будет копироваться наша папка shared у пользователя должны быть права
на запись в эту папку и на запись в папку куда мы деплоим
  
  
Файлы (oauth ...) продублированы с челью отделить продакшн настройку от локальной. 
при деплое реальные конфиги будут перезатираться продублированными
посли первого же скачивания их желательно занести в .gitignore

Деплой:
  cap production deploy:setup для первого запуска копирует shared
  cap production deploy для каждого последующего запуска если не менались конфиги
  cap production private_pub:start для старта faye сервера.
