# Установка WireGuard UI

1. Устанавливаем Docker и Docker Compose, если вы ещё не устанавливали.

```
curl -sSL https://get.docker.com | sh
sudo usermod -aG docker $(whoami)
exit
```

И войдите снова.

2. Создайте каталог, перейдите в него, и туда скачайте файл docker-compose.yml

```
mkdir wireguard-ui
cd wireguard-ui
wget https://github.com/logdog90/wireguard-ui/blob/main/docker-compose.yml
nano docker-compose.yml
```

Измените WGUI_USERNAME=admin и WGUI_PASSWORD=password на ваш логин и пароль. Это будет ваш логин и пароль от админки WireGuard.

3. Запустите контейнер командой.

```
docker compose up -d
```

4. Перейдите http://0.0.0.0:5000. Вместо нулей введите свой внутренний IP-адрес. Например, http://192.168.0.55:5000. Посмотреть свой IP вы можете командой

```
ip a | grep `ip route | awk '/default/ {print $5; exit}'` | grep inet
```

5. Авторизируйтесь в админке и перейдите в раздел Wireguard Server.

Server Interface Addresses. Можете оставить по умолчанию.

```
10.0.0.0/24
```
_Теперь внимание! Вам необходимо прописать правила iptables, для того чтобы у вас шли пакеты (был интернет)_

Post Up Script
```
iptables --table nat --append POSTROUTING --jump MASQUERADE --out-interface `ip route | awk '/default/ {print $5; exit}'`
```
Post Down Script
```
iptables --table nat --delete POSTROUTING --jump MASQUERADE --out-interface `ip route | awk '/default/ {print $5; exit}'`
```
