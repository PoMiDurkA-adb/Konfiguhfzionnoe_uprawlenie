# Практическое занятие №1. Введение, основы работы в командной строке

П.Н. Советов, РТУ МИРЭА

Научиться выполнять простые действия с файлами и каталогами в Linux из командной строки. Сравнить работу в командной строке Windows и Linux.

## Задача 1

Вывести отсортированный в алфавитном порядке список имен пользователей в файле passwd (вам понадобится grep).

grep '.*' /etc/passwd | cut -d: -f1 | sort

![image](https://github.com/user-attachments/assets/0b674446-7d31-4804-a144-584e956542fe)


## Задача 2

Вывести данные /etc/protocols в отформатированном и отсортированном порядке для 5 наибольших портов, как показано в примере ниже:

```
[root@localhost etc]# cat /etc/protocols ...
142 rohc
141 wesp
140 shim6
139 hip
138 manet
```
awk '{print $2, $1}' /etc/protocols | sort -nr | head -n 5

![image](https://github.com/user-attachments/assets/614bf0e1-ced2-4e20-9c61-84badb30f3c9)


## Задача 3

Написать программу banner средствами bash для вывода текстов, как в следующем примере (размер баннера должен меняться!):

```
[root@localhost ~]# ./banner "Hello from RTU MIREA!"
+-----------------------+
| Hello from RTU MIREA! |
+-----------------------+
```

Перед отправкой решения проверьте его в ShellCheck на предупреждения.

text="Hello from RTU MIREA!"; text_length=${#text}; line=$(printf "%${text_length}s" | tr ' ' '-'); echo "+${line}+"; echo "| ${text} |"; echo "+${line}+"

![image](https://github.com/user-attachments/assets/1250a4f4-67fb-4e2e-bbaf-37252f5a75f5)


## Задача 4

Написать программу для вывода всех идентификаторов (по правилам C/C++ или Java) в файле (без повторений).

Пример для hello.c:

```
h hello include int main n printf return stdio void world
```
![image](https://github.com/user-attachments/assets/074cf47c-6cb3-4079-be92-adba556a73e9)

## Задача 5

Написать программу для регистрации пользовательской команды (правильные права доступа и копирование в /usr/local/bin).

Например, пусть программа называется reg:

```
./reg banner
```

В результате для banner задаются правильные права доступа и сам banner копируется в /usr/local/bin.
```
#!/bin/bash

if [ "$#" -ne 1 ];
 exit 1 
fi
COMMAND=$1
if [ ! -f "$COMMAND" ]; then
 exit 1 
fi
sudo cp "$COMMAND" /usr/local/bin/
sudo chmod +x /usr/local/bin/"$COMMAND"
echo "$COMMAND copied in /usr/local/bin"
```
![image](https://github.com/user-attachments/assets/bdd6ee5b-2512-4eab-ab82-a0f60cb2e363)

## Задача 6

Написать программу для проверки наличия комментария в первой строке файлов с расширением c, js и py.
```
#!/bin/bash

if [ "$# -ne 1 ]; then
 exit 1 
fi
file=$1
if [ "$file" != "*.c" ] && [ "$file" != "*.py" ] && [ "$file" != "*.js" ]
 exit 1 
fi 
if [[ $(head -n 1 $file) == \#* ]]; then
 echo "py"
fi
if [[ $(head -n 1 $file) == \//* ]]; then
 echo "c or js"
fi
```
![image](https://github.com/user-attachments/assets/c1147886-88cc-47dc-9ea0-3bdff31ff606)

## Задача 7

Написать программу для нахождения файлов-дубликатов (имеющих 1 или более копий содержимого) по заданному пути (и подкаталогам).

```
#!/bin/bash

if [ "$#" -ne 1 ]; then
 exit 1
fi
DIR=$1
if [ ! -d "$DIR" ]; then
 exit 1
fi
find "$DIR" ! -empty -type f -exec md5sum {} + | sort
echo "Task complete"
```
![369774597-12a90812-df6e-4e31-9562-98276f17e563](https://github.com/user-attachments/assets/fe3547c0-a91c-4cd0-b6b9-117763173e53)


## Задача 8

Написать программу, которая находит все файлы в данном каталоге с расширением, указанным в качестве аргумента и архивирует все эти файлы в архив tar.

```
#!/bin/bash

if [ "$#" -ne 2 ]; then
 exit 1
fi
CATALOG="$1"
ARCHIVE="$2"

tar -cf "$ARCHIVE.tar" $(find . -maxdepth 1 -type f -name "*.$CATALOG")

echo "Archive '$ARCHIVE.tar' created."
```
![369771540-4ecb252e-9b21-4e24-b1a0-23897ab1989b](https://github.com/user-attachments/assets/556dbfa3-d952-4046-bbfd-92384a62e454)

## Задача 9

Написать программу, которая заменяет в файле последовательности из 4 пробелов на символ табуляции. Входной и выходной файлы задаются аргументами.
```
#!/bin/bash

if [ "$# -ne 2 ]; then
 exit 1
fi

INPUT="$1"
OUTPUT="$2"

if [ ! -f "$INPUT" ]; then
 exit 1
fi

sed 's/    /\t/g' "$INPUT" > "$OUTPUT"
echo "Task complete"
```
![369771618-5537ec83-0878-487a-b487-6d97893142a5](https://github.com/user-attachments/assets/9bbd6295-ebf0-4fa0-8415-0999850549ac)


## Задача 10

Написать программу, которая выводит названия всех пустых текстовых файлов в указанной директории. Директория передается в программу параметром. 

```
#!/bin/bash

if [ "$#Э -ne 1 ]; then 
 exit 1
fi # конец if
DIR=$1

if [ ! -d "$DIR" ]; then
 echo "ERR: Dir doesn't exist."
 exit 1
else
 echo "Searching..."
fi

find "$DIR" -type f -name "*.txt" -empty
```
![369774975-47e54b40-d527-482d-b4b9-27f457147680](https://github.com/user-attachments/assets/130ad599-24e0-4e4f-8b40-0643e5cb39e9)

