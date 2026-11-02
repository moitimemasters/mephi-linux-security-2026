# ДЗ №1. Изучение средств защиты ОС GNU/Linux

Песня Иван Александрович. Бухгалтерский шифр: 372344.

ОС: Fedora Linux 44 (aarch64).

## 1. Создание пользователя

Создан `user1` с UID 1234 и домашним каталогом `/home/user1`. Пользователь входит в группу `students`. Срок действия пароля установлен в 90 дней командой `chage -M 90 user1`.

Файлы: [passwd](passwd), [group](group), [shadow](shadow).

## 2. Мониторинг файлов и процессов

Файлы с битом set-UID найдены командой `find / -type f -perm -4000`. Список находится в [suid.txt](suid.txt).

Для проверки процессов от `user1` запущен `passwd`. У него реальный UID 1234, эффективный UID 0. Вывод сохранён в [processes.out](processes.out).

## 3. Механизм set-UID

В `/home/user1/cat-suid` скопирован `cat`, установлен владелец root и права 4755. Обычный `cat` от `user1` не читает `/etc/shadow`, а копия с set-UID читает его. Права каталога и утилит приведены в [stat.out](stat.out).

## 4. Привилегии

Копии `/home/user1/chown-cap` выдана привилегия `cap_chown=ep`. При проверке обычный `chown` не смог сменить владельца тестового файла на `nobody`, а `chown-cap` выполнил эту операцию.

Вывод проверки: [getcap.out](getcap.out).

## 5. Sudo

В [sudoers](sudoers) добавлено правило:

```sudoers
user1 ALL=(root) NOPASSWD: /usr/bin/date
```

Изменение времени от `user1` проверено через `sudo date -u -s '+1 minute'`. После проверки синхронизация времени включена.

Настройки и работа утилит проверены после перезагрузки.

[История команд root](history.out) · [История команд user1](history-user1.out) · [Скриншот терминала](mephi-screenshot.png)
