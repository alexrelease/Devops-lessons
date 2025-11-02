# Создать рабочую структуру
mkdir -p ~/devops-fundamentals/day1/logs
cd ~/devops-fundamentals/day1

# Сгенерировать тестовый лог (или скопировать свой)
dmesg > logs/system.log

# Топ-5 повторяющихся строк
sort logs/system.log | uniq -c | sort -nr | head -5 > freq.txt

# Кол-во строк с "error" без учёта регистра
grep -i 'error' logs/system.log | wc -l > errors.count

# Системная информация
USER=$(whoami)
HOST=$(hostname)
KERNEL=$(uname -r)
FREE=$(df -h / | awk 'NR==2{print $4}')
DATE=$(date -Iseconds)

# Собрать отчёт
cat > report.txt <<EOF
Дата: $DATE
Пользователь: $USER
Хост: $HOST
Версия ядра: $KERNEL
Свободное место на /: $FREE

Топ-5 строк лога (частота — строка):
$(sort logs/system.log | uniq -c | sort -nr | head -5)
EOF
