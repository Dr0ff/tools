# Проверка установки и настройки сервера

Этот скрипт поможет студентам проверить, правильно ли они настроили сервер после его аренды. Он проверяет наличие пользователя, установленных пакетов, конфигурацию SSH, а также корректность установки Go и переменных среды.

## 🔧 Что должен выполнить студент?

1. **Подключиться к серверу через SSH**:
   ```bash
   ssh <ИМЯ_ПОЛЬЗОВАТЕЛЯ>@<IP_СЕРВЕРА>   Пример: root@192.168.208.11
   ```
*Важно ввод пароля в консоли НЕ ОТОБРАЖАЕТСЯ!!!
   
2. **Создать нового пользователя и дать ему права sudo**:
   ```bash
   sudo adduser <имя_пользователя>  Пример: sudo adduser superman
   sudo usermod -aG sudo <имя_пользователя>  Пример: sudo usermod -aG sudo superman
   ```
3. **Переключиться на нового пользователя**:
   ```bash
   su <имя_пользователя> Пример: su superman
   ```
4. **Обновить систему и установить необходимые пакеты**:
   ```bash
   sudo apt update && sudo apt upgrade -y
   sudo apt install mc btop nano screen git make build-essential jq -y
   ```
5. **Отключить вход под root в SSH**:
   - Отредактировать файл конфигурации SSH:
     ```bash
     sudo nano /etc/ssh/sshd_config
     ```
   - Установить параметр:
     ```
     PermitRootLogin no
     ```
   - Перезапустить SSH:
     ```bash
     sudo systemctl restart ssh
     sudo systemctl restart sshd
     ```
6. **Установить Go**:
   ```bash
   sudo rm -rvf /usr/local/go/
   wget https://golang.org/dl/go1.23.4.linux-amd64.tar.gz
   sudo tar -C /usr/local -xzf go1.23.4.linux-amd64.tar.gz
   rm go1.23.4.linux-amd64.tar.gz
   ```
7. **Объявить переменные среды для Go** (например в ~/.profile):
   ```bash
   export GOROOT=/usr/local/go
   export GOPATH=$HOME/go
   export GO111MODULE=on
   export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin
   ```
   Применить переменные (если ~/.profile):
   ```bash
   source ~/.profile
   ```

## 📥 Как загрузить и запустить скрипт проверки?

1. **Установить wget (если не установлен)**:
   ```bash
   sudo apt install wget -y
   ```
2. **Скачать скрипт с GitHub**:
   ```bash
   wget https://raw.githubusercontent.com/ptzruslan/tools/refs/heads/main/validator/tech02/practice_tech_02.sh -O practice_tech_02.sh
   ```
3. **Дать скрипту права на выполнение**:
   ```bash
   chmod +x practice_tech_02.sh
   ```
4. **Запустить скрипт (указав имя созданного пользователя)**:
   ```bash
   ./practice_tech_02.sh <имя_пользователя>
   ```

## 📌 Что проверяет скрипт?
- Создан ли пользователь и есть ли у него права sudo
- Обновлена ли система
- Установлены ли необходимые пакеты
- Отключён ли root-доступ по SSH
- Работает ли SSH
- Установлен ли Go и правильно ли объявлены переменные среды

Если всё выполнено корректно, студент увидит ✅ в консоли. Если найдены ошибки, появятся ❌ или ⚠️ с рекомендациями.
