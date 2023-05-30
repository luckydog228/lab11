# lab11

Ngrok — это платформа, которая с помощью установленной утилиты, позволяет, организовать удалённый доступ на веб-сервер или какой-то другой сервис, запущенный ПК. Доступ организуется через созданный при запуске ngrok безопасный туннель. ПК, при этом, может находиться за NAT’ом, и не иметь статического IP адреса.

Совсем не обязательно тащить тестовый проект куда-то ещё, можно показать его заказчику прямо с локальной машины, или, например, с помощью Ngrok можно очень легко расшарить файлы лежащие на ПК.

Вначале создаем папки: 
```
$ mkdir install 
$mkdir tmp 
```

Далее создаем переменные окружения: 
```
$ export HOME_PREFIX=pwd/install
 $ echo $HOME_PREFIX
 $ export USERNAME=whoami 
```

Скачиваем архив 
```$ wget https://github.com/libevent/libevent/releases/download/release-2.1.8-stable/libevent-2.1.8-stable.tar.gz```

Разархивируем его 
```$ tar -xvzf libevent-2.1.8-stable.tar.gz```

Перемещаемся в папку с ним 
```$ cd libevent-2.1.8-stable```

Указываем путь до директории для установки 
```$ ./configure --prefix=${HOME_PREFIX}```

Устанавливаем 
```$ make && make install```

Перемещаемся обратно в исходную папку
```
$ cd..
```
Проделываем тоже самое с архивами ncurses и tmux. Для 1-ого:
```
$ wget http://invisible-island.net/datafiles/release/ncurses.tar.gz 
$ tar -xvzf ncurses.tar.gz $ cd ncurses-6.3 $ ./configure --prefix=${HOME_PREFIX} 
$ make && make install 
$ cd .. 
```
Для второго 
```
$ wget https://github.com/tmux/tmux/releases/download/2.5/tmux-2.5.tar.gz 
$ tar -xvzf tmux-2.5.tar.gz 
$ cd tmux-2.5 
$ ./configure --prefix=${HOME_PREFIX} CFLAGS="-I${HOME_PREFIX}/include -I${HOME_PREFIX}/include/ncurses" LDFLAGS="-L${HOME_PREFIX}/lib" 
$ make && make install 
$ cd .. 
```
Скачиваем архив ngrok и разархивируем его 
```
$ wget https://bin.equinox.io/c/4VmDzA7iaHb/ngrok-stable-linux-amd64.zip 
$ unizp ngrok-stable-linux-amd64.zip $ mv ngrok ${HOME_PREFIX}/bin 
```
Создаем переменные окружения и запускаем tmux 
```
$ export LD_LIBRARY_PATH=${HOME_PREFIX}/lib 
$ export PATH="${HOME_PREFIX}/bin:
${PATH}" 
$ tmux
```
Далее мы удаляем папки tmp и install. 
И выполняем: ``` $ brew install tmux ngrok```
Запускаем новую сессию в tmux ``` $ tmux new -s session_with_group ``` 
Далее на одном компьютере выполняем следущие действия: переходим на сайт для регистрации 
```$ open https://ngrok.com/signup``` 
создаем переменную окружения 
```$ export NGROK_TOKEN=<токен>```
авторизуемся в ngrok 
```
$ ngrok authtoken
${NGROK_TOKEN}
```
указываем тип протокола и порт подключения <порт_ngrok_сервера> ```$ ngrok tcp 22```
На втором компьютере : ```$ ssh ${USERNAME}@0.tcp.ngrok.io -p<порт_ngrok_сервера> <пароль_от_учетной_записи>```
подключаемся к созданной сессии ```$ tmux a -t session_with_group```
 После этих действий мы обеспечили доступ с одного компьютера к другому.
