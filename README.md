# VNC-Server-Ubuntu-16.04-LTS

# Проверенно лично на реальных событиях!

В этом руководстве будет рассмотрен процесс настройки сервера VNC для осуществления удаленного управления виртуальными серверами под управлением операционной системы Ubuntu 16.04 x64.

# Что это такое

VNC — широко распространенный способ удаленного управления рабочим столом компьютера по сети. VNC работает по модели клиент-сервер и использует специализированный сетевой протокол Remote Frame Buffer (RFB). Клиенты VNC (иногда называемые зрителями) совместно с сервером используют пользовательский ввод (нажатия клавиш, движения мыши, клики и сенсорные нажатия). Серверы VNC захватывают содержимое фреймбуфера локального дисплея и передают их обратно клиенту, а также заботятся о передаче удаленного клиентского ввода на локальный вход. Соединения по RFB обычно идут на TCP-порт сервера с номером 5900.

Gnome - (GNU Network Object Model Environment) представляет собой графический пользовательский интерфейс (GUI) и набор компьютерных настольных приложений для пользователей операционной системы Linux. Он предназначен для того, чтобы сделать операционную систему Linux простой в использовании для не-программистов и в целом соответствует рабочему интерфейсу Windows и его наиболее распространенному набору приложений. В GNOME пользовательский интерфейс может, например, быть похожим на Windows или Mac OS. Кроме того, GNOME включает набор приложений того же типа, что и продукт Windows Office: текстовый процессор, программа для работы с электронными таблицами, менеджер баз данных, разработчик презентации, веб-браузер и программа электронной почты.

# Установка VNC Server и рабочего окружения GNOME

* Для поднятия сервера можете использовать удобным ресурсом https://www.vultr.com/?ref=7489180 обслуживание сервера вам обойдется от 2 до 5 $

* Для создания и смены пароля root есть следующие команды:

* Чтобы добавить нового пользователя 

$ sudo adduser newuser
*Для смены пароля пользователя используем команду:

 $ passwd


# Прежде всего следует обновить локальную базу пакетов и приступи к установки и запуску VNC Server

$ sudo apt-get update

# Далее установите пакеты из главного репозитория:

$    sudo apt-get install --no-install-recommends ubuntu-desktop gnome-panel gnome-settings-daemon metacity nautilus gnome-terminal   vnc4server

Для завершения начальной конфигурации VNC-сервера выполните команду vncserver для установки пароля. Также будет предложено ввести “view-only” пароль для аутентификации только для просмотра. Пользователи, которые будут авторизованы с помощью “view-only” пароля, не смогут контролировать рабочий стол с помощью мыши или клавиатуры.

# Настройка VNC Server
Во-первых, мы должны указать VNC серверу какие команды выполнять при запуске. Они находятся в файле ~/.vnc/xstartup. Сценарий запуска был создан на предыдущем шаге, но в нем нужно изменить некоторые команды для рабочего окружения Gnome. Инициализация VNC сервера по умолчанию происходит на порт 5901, называемый “Порт дисплея” и упоминается как :1. VNC может запускать несколько экземпляров на других портах: :2, :3, и т.д.

Перед тем как приступить к изменениям настройки VNC сервера необходимо остановить экземпляр на порту 5901:

$ vncserver -kill :1

# Откройте файл в текстовом редакторе:

$  nano ~/.vnc/xstartup

# Добавьте следующие строки:

[ -x /etc/vnc/xstartup ] && exec /etc/vnc/xstartup

[ -r $HOME/.Xresources ] && xrdb $HOME/.Xresources

xsetroot -solid grey

vncconfig -iconic &

x-terminal-emulator -geometry 80x24+10+10 -ls -title "$VNCDESKTOP Desktop" &

x-window-manager &

gnome-panel &

gnome-settings-daemon &

metacity &

nautilus &

# Проброс портов для возможности удаленного доступа:

$ iptables -A INPUT -p tcp --dport 5901 -j ACCEPT
$ iptables-save

*Запустим рабочий стол, который будет доступен удаленно:

$ vncserver

# При каждом новом запуске рабочего стола с номером X необходимо пробрасывать порт для удаленного доступа:

$ iptables -A INPUT -p tcp --dport 59XX -j ACCEPT

$ iptables-save

# Проверка VNC
Для проверки корректности работы воспользуйтесь клиентом рабочего стола, например Remmina.

Для первой проверки сгодится программа для андроида Desktop169 как убидитесь, что у Вас все работает можете настраивать на любой VNC клиент.
