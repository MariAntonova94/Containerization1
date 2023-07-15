﻿# Контейнеризация (семинары)

## Урок 1. Механизмы пространства имен

### **Информация о проекте**

Необходимо продемонстрировать изоляцию одного и того же приложения (как решено на семинаре - командного интерпретатора) в различных пространствах имен.

**Задание**

* Запустим Bash в новом пространстве имен командой (так как команда требует привилегии суперпользователя, то выполняем через sudo):
```
sudo unshare -pf -n --mount-proc bash
```
* В параллельном терминале смотрим, что произошло (наглядно):
```
ps -afx
```
Наблюдаем несколько родительских процессов, которые породили одинаковые процессы Bash.
![ps -afx](https://github.com/MariAntonova94/Containerization1/blob/main/file/1.1.1.jpd.jpg)

Для проверки и наглядности смотрим той же командой ps -afx в изолированном терминале
![ps -afx](https://github.com/MariAntonova94/Containerization1/blob/main/file/2.1.1.jpd.jpg)
Здесь мы сожем видеть, что изолированная оболчка видит всего два процесса (и то, второй процесс сразу же, после выполнения команды, исчезнет).

* Далее, из этого же терминала, пробуем запустить пинг любого сайта, например ya.ru.
```
ping ya.ru
```
 ![ps -afx](https://github.com/MariAntonova94/Containerization1/blob/main/file/2.1.2.jpd.jpg)
 Наблюдаем, что пинг до указанного сайта не может быть осуществлен, так как сеть в этом пронстранстве имен имеется только локальная, т.е. localhost.


А теперь, ту же команду запустим из параллельного терминала (котоый не изолирован).
![ps -afx](https://github.com/MariAntonova94/Containerization1/blob/main/file/1.1.2..jpd.jpg)
Видим, что пакеты до сайта уходят нормально и ответ от сервера принимается.


* Теперь, в оба терминала отправляем команду $ hostname, что бы увидеть наш хост.
```
hostname
```
![ps -afx](https://github.com/MariAntonova94/Containerization1/blob/main/file/2.1.3.jpd.jpg)
Как мы можем заметить - хоcт и там и там одинаков. А теперь, в изолированном терминале выполняем команду:
```
sudo unshare -u bash
```
Команда $ unshare запускает программу (опционально) в новом namespace. Флаг -u говорит ей запустить bash в новом UTS namespace. Обратите внимание, что наш новый процесс bash указывает на другой файл UTS, тогда как все остальные остаются прежними. 

Одним из следствий того, что мы только что проделали, является то, что теперь мы можем изменить системный hostname из нашего нового процесса bash и это не повлияет ни на какой другой процесс в системе. 
Изменим наш хост в изолированном терминале, например:
```
hostname mari2
```
Эта команда никак не затронула хост основной системы. Можем проверить это, выполнив hostname в первом терминале и увидев, что имя хоста там не изменилось.
![ps -afx](https://github.com/MariAntonova94/Containerization1/blob/main/file/1.2.4jpd.png)


Этими манипуляциями мы доказали, что научились запускать процессы в разных namespace с возможностью опционально изолировать нужные нам направления.



*Подготовила студентка GeekBrains* [*`Антонова Мария`*]
