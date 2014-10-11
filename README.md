chat_deploy
===========
Приложение развёртывается на virtial host под управлением ubuntu 13.4 apache passenger postgresql
1.Перед развёртыванием :
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
  
2.Создаём приложения в social.app для vkontakte facebook google
3.Скачиваем наше приложение с git@github.com:dpblh/chat.git

4.Устанавливаем наши secret keys из social.app в файлы config/oauth.yml и shared/oauth.yml
5.Добавиляем нашего пользователь базы и пароль в файлы config/database.yml и shared/database.yml
6.Конфигурируем private_pub в файлах config/private_pub.yml и shared/private_pub.yml в продакшн ставим server внешний адрес и генерируем secret_token желательно по длиньше
7.Добавиляем в файлы config/secrets.yml и shared/secrets.yml secret_key_base тоже по длиньше
8.Правим deploy/production.rb
  1. role :app, %w{} прописываем реальные user и host через к которым мы будем ломиться для деплоя
  2. role :web, %w{} прописываем реальные user и host через к которым мы будем ломиться для деплоя
  3. role :db,  %w{} прописываем реальные user и host через к которым мы будем ломиться для деплоя
  4. server '', user: '' host user
  5. password: '' пароль под от user. По причине не приодолимых препятствий rsa ключ у меня не заработал.
  у вас еcли деплоится на реальную машину все должно быть ок. при условии что удаленная машина доверяет нам
9.Правим config/deploy.rb устанавливаем путь куда будет копироваться наша папка shared у пользователя должны быть права
на запись в эту папку и на запись в папку куда мы деплоим
  
-------------
Файлы (oauth ...) продублированы с челью отделить продакшн настройку от локальной. 
при деплое реальные конфиги будут перезатираться продублированными
посли первого же скачивания их желательно занести в .gitignore
---------
10.Деплой:
  1. cap production deploy:setup для первого запуска копирует shared
  2. cap production deploy для каждого последующего запуска если не менались конфиги
  3. cap production private_pub:start для старта faye сервера.
