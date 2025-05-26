# Полное руководство по установке и настройке Kubuntu

## Содержание
1. [Установка системы](#установка-системы)
2. [Базовая настройка](#базовая-настройка)
3. [Установка программного обеспечения](#установка-программного-обеспечения)
4. [Настройка окружения](#настройка-окружения)
5. [Дополнительные настройки](#дополнительные-настройки)

---

## Установка системы

### Вариант 1: Установка на один SSD
```bash
# Создание загрузочной флешки
sudo dd if=kubuntu.iso of=/dev/sdX bs=4M status=progress oflag=sync
```
1. Загрузитесь с флешки
2. Выберите "Стереть диск и установить Kubuntu"
3. Разметка диска:
   * / - 30-50 ГБ (ext4)
   * swap - размер равен RAM (минимум 4GB)
   * /home - оставшееся пространство (ext4)

### Вариант 2: Установка на два SSD
1. В ручной разметке:

   * Первый SSD:
     * /boot/efi - 512MB (FAT32)
     * / - 30-50GB (ext4)
     * swap - размер как выше
   * Второй SSD:
     * /home - всё пространство (ext4)

2. Установите загрузчик на первый SSD

## Базовая настройка

#### Настройка русского языка
```bash
sudo apt install -y language-pack-ru kde-l10n-ru
sudo dpkg-reconfigure locales
# Выберите ru_RU.UTF-8 и en_US.UTF-8
```
#### Настройка переключения раскладки (Shift+Alt)
1. Откройте: Настройки системы → Устройства ввода → Клавиатура
2. Во вкладке "Раскладки":
   * Добавьте русскую раскладку
   * Установите сочетание клавиш Shift+Alt

## Установка программного обеспечения
### IntelliJ IDEA
```bash
wget https://download.jetbrains.com/idea/ideaIC-2023.3.tar.gz
tar -xzf ideaIC-2023.3.tar.gz -C ~/
echo 'export PATH="$PATH:$HOME/idea-IC-*/bin"' >> ~/.bashrc
source ~/.bashrc
```

[Активация](https://3.jetbra.in/)

```bash
cd ~/jetbra/scripts/ && ./install.sh
```

### Java и Maven
```bash
# Установка JDK
sudo apt install -y openjdk-11-jdk openjdk-17-jdk openjdk-21-jdk maven

# Настройка альтернатив
sudo apt install -y openjdk-11-jdk openjdk-17-jdk openjdk-21-jdk
sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/java-11-openjdk-amd64/bin/java 1111
sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/java-17-openjdk-amd64/bin/java 1717
sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/java-21-openjdk-amd64/bin/java 2121

sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/java-11-openjdk-amd64/bin/javac 1111
sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/java-17-openjdk-amd64/bin/javac 1717
sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/java-21-openjdk-amd64/bin/javac 2121
```

```bash
sudo update-alternatives --config java
```
```bash
sudo update-alternatives --config javac
```

### Docker
```bash
sudo apt install -y docker.io
sudo systemctl enable --now docker
sudo usermod -aG docker $USER
newgrp docker
```

### Браузеры
```bash
# Yandex Browser
wget https://repo.yandex.ru/yandex-browser/deb/pool/main/y/yandex-browser-stable/yandex-browser-stable_23.11.3.913-1_amd64.deb
sudo apt install -y ./yandex-browser-stable_*.deb

# Google Chrome
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
sudo apt install -y ./google-chrome-stable_current_amd64.deb
```

## Настройка окружения

#### Темы и оформление

| Компонент   | Путь настройки              |
|-----------|-----------------------------| 
| **Стиль Plasma**  | Настройки → Внешний вид → Стиль Plasma                  |
| **Оформление окон**  | Настройки → Внешний вид → Оформление окон                  |
| **GTK-темы**  | Настройки → Внешний вид → Стиль приложений → GTK-тема                  |
| **Иконки**  | Настройки → Внешний вид → Иконки                  |

## Дополнительные настройки

### Автовыключение

#### Одной строкой
```bash
# Добавление в cron
(crontab -l 2>/dev/null; echo "30 23 * * * /sbin/shutdown -h now") | crontab -
```

#### Несколько команд
1. Откройте файл crontab для редактирования:
```bash
crontab -e
```
2. Выбрать **/bin/nano**
3. Добавьте строку с расписанием и командой выключения
```bash
30 23 * * * /sbin/shutdown -h now
```
4. Сохраните (Ctrl+O в nano, затем Enter) и закройте (Ctrl+X).
5. Проверьте, что задача добавлена
```bash
crontab -l
```

### Настройка GTK-приложений
```bash
echo 'export GTK_THEME=Dark-Olympic' >> ~/.profile
```

### PortProton (для Windows-приложений)
```bash
wget https://github.com/Castro-Fidel/PortProton/releases/latest/download/PortProton_1.0.tar.gz
tar -xvf PortProton_1.0.tar.gz -C ~/
chmod +x ~/PortProton/portproton
sudo chmod u+s /usr/bin/bwrap
```

### Завершение
```bash
sudo reboot
```