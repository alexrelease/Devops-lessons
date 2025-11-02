# Day 2 – permissions, umask, tar, env

# 1. Подготовка структуры
mkdir -p ~/devops-fundamentals/day2/{bin,configs,logs}
cd ~/devops-fundamentals/day2
touch configs/app.conf
touch logs/app.log

# 2. Права (символьные и числовые)
ls -l
chmod u=rw,go= configs/app.conf
chmod 640 logs/app.log
chmod 750 bin
ls -l

# 3. Проверка групп и смена группы (если нужно)
id
# chgrp <groupname> configs/app.conf

# 4. umask: посмотреть текущий и сравнить
umask
touch test_default
mkdir test_dir_default
ls -l

umask 027
touch test_027
mkdir test_dir_027
ls -l

# 5. Архивирование
tar -czf app-logs.tar.gz configs logs
tar -tvf app-logs.tar.gz | head
mkdir -p extract_test
tar -xzf app-logs.tar.gz -C extract_test

# Архив без логов
tar -czf app-no-logs.tar.gz --exclude='*.log' configs logs

# 6. Переменные окружения
export APP_ENV=dev
echo "ENV=$APP_ENV" > env.txt

# добавить локальный bin в начало PATH на время сессии
export PATH="$PWD/bin:$PATH"
echo "$PATH" | tr ':' '\n' | head -20

# 7. Итоговый отчёт по правам
cat > permissions_report.txt <<EOF
===ОТЧЕТ ПРАВ ДОСТУПА===
Дата: $(date -Iseconds)
umask (до): файл 666 (rw-rw-rw), директория 777 (rwxrwxrwx)
umask (после 027): файл 640 (rw-r-----), директория 750 (rwxr-x---)

Пояснения:
- 640 = u:rw-, g:r--, o:---
- 750 = u:rwx, g:r-x, o:---

tar:
- c/x/t = create/extract/list
- z = gzip, f = файл архива
- пример: tar -czf app-logs.tar.gz configs logs
- исключения: --exclude='*.log'
EOF
