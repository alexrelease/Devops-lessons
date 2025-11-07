Список процессов:
ps aux | head -20 > ps.txt
top -b -n 1 | head -20 > top.txt #top в Linux — это интерактивный системный монитор, который отображает информацию о процессах и использовании ресурсов системы в реальном времени. Она показывает такие данные, как загрузка CPU, использование памяти, список процессов и их характеристики
Кто слушает порты:
ss -lntp | head -20 > sockets.txt
Статус сервисов:
systemctl status systemd-resolved 2>&1 | head -40 > systemd-resolved.txt # проверка статуса работы сервиса resolved/вывод первые 40 строк stdr и stdout
systemctl status systemd-networkd 2>&1 | head -40 > systemd-networkd.txt 
Журналы:
journalctl -n 50 > journal.txt 2>&1 # вывод 50 свежих запесей системного журнала

