1. Узнать IP-адрес интерфейса, подключенного к сети Интернет
```
echo "$(ip route get 8.8.8.8 | grep -oP 'src \K\S+')"
```
2. Создать именованный пайп ( named pipe ). Вывести в файл через созданный pipe вывод команды ss -plnt

```
mkfifo named_pipe
echo "$(ss -plnt)"
ss -plnt > named_pipe & cat named_pipe > output.txt

```
3. При помощи именованного пайпа заархивировать всё, что в него отправляем.
Например, содержимое файла /var/log/messages
На выходе получить архив tar или любой другой

```
mkfifo archive_pipe
cat /var/log/messages > archive_pipe &
tar -czvf archive.tar.gz -T archive_pipe
```

4. Вывести дату в unixtime
На вход команды date через пайп подать свой формат выводимой даты 
```
echo "$(date '+%Y-%m-%d %H:%M:%S')"

date '+%Y-%m-%d %H:%M:%S' > /tmp/named_pipe &
timestamp=$(cat /tmp/named_pipe | xargs -I {} date -d "{}" +%s)
```

5. При помощи HEREDOC записать в файл многострочное сообщение
```
cat << EOF > my_message.txt
Это первая строка сообщения.
Это вторая строка.
Это третья строка.
EOF
```

