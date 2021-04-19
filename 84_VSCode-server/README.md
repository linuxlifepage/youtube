## 1) Установка в Докере:

```
sudo docker run -d -it --name code-server -p 8080:8080 \
 --restart unless-stopped \
  -v "$PWD:/home/coder/project" \
  -u "$(id -u):$(id -g)" \
  -e "DOCKER_USER=$USER" \
  codercom/code-server:latest
```

```
sudo docker exec -it code-server bash
```

Узнать/поменять пароль
```
cat .config/code-server/config.yaml
```

## 2) Установка через .deb пакет

Перейдем по адресу https://github.com/cdr/code-server/releases

Качаем 

```
cd /tmp
wget https://github.com/cdr/code-server/releases/download/v3.9.2/code-server_3.9.2_amd64.deb
```

Устанавливаем 

```
sudo dpkg -i code-server_3.9.2_amd64.deb
```

Стартуем службу от пользователя, и включаем ее автостарт

```
systemctl --user start code-server
systemctl --user enable code-server
systemctl --user status code-server
```

Узнаем/меняем пароль, правим ip на локальный 10.0.0.11 (посмотреть ip a)

```
nano ~/.config/code-server/config.yaml
```

Добавим правило IPTABLES

```
sudo iptables -I INPUT 1 -p tcp -m tcp --dport 8080 -j ACCEPT
```

И также в файл (Если у вас сервер Oracle). 

```
sudo vim /etc/iptables.rules
-A INPUT -p tcp -m tcp --dport 8080 -j ACCEPT
```

Если у вас НЕ сервер Oracle, то открыть порт через команду:

```
sudo ufw allow 8080/tcp
```
