
### Задание 1

Проведите разведку системы и определите, какие сетевые службы запущены на защищаемой системе:

**sudo nmap -sA < ip-адрес >**

**sudo nmap -sT < ip-адрес >**

**sudo nmap -sS < ip-адрес >**

**sudo nmap -sV < ip-адрес >**

По желанию можете поэкспериментировать с опциями: https://nmap.org/man/ru/man-briefoptions.html.


*В качестве ответа пришлите события, которые попали в логи Suricata и Fail2Ban, прокомментируйте результат.*




### Решение 1

**sudo nmap -sA < ip-адрес > (TCP ACK сканирование)**

Данный вид сканирования не был зафиксирован в логах Suricata и Fail2Ban.


**sudo nmap -sT < ip-адрес > (TCP сканирование с использованием системного вызова connect)**

#### Скриншот:

![Commit Task1](https://github.com/AndrewZnamenskiy/netsecurity/blob/main/img/task1p1.png)


**sudo nmap -sS < ip-адрес > (TCP SYN сканирование)**

#### Скриншот:

![Commit Task1](https://github.com/AndrewZnamenskiy/netsecurity/blob/main/img/task1p2.png)


**sudo nmap -sV < ip-адрес > (Функция определения версии)**

#### Скриншот:

![Commit Task1](https://github.com/AndrewZnamenskiy/netsecurity/blob/main/img/task1p3.png)



------

### Задание 2

Проведите атаку на подбор пароля для службы SSH:

**hydra -L users.txt -P pass.txt < ip-адрес > ssh**

1. Настройка **hydra**:
 
 - создайте два файла: **users.txt** и **pass.txt**; - в каждой строчке первого файла должны быть имена пользователей, второго — пароли. В нашем случае это могут быть случайные 
 строки, но ради эксперимента можете добавить имя и пароль существующего пользователя.

Дополнительная информация по **hydra**: https://kali.tools/?p=1847.

2. Включение защиты SSH для Fail2Ban:

- открыть файл /etc/fail2ban/jail.conf, - найти секцию **ssh**, - установить **enabled** в **true**.

Дополнительная информация по **Fail2Ban**:https://putty.org.ru/articles/fail2ban-ssh.html.



*В качестве ответа пришлите события, которые попали в логи Suricata и Fail2Ban, прокомментируйте результат.*



### Решение 2


#### Скриншот лога Suricata:

![Commit Task2](https://github.com/AndrewZnamenskiy/netsecurity/blob/main/img/task2p1.png)

- Удалось подобрать пароль за 20 попыток залогинивания

#### Скриншот лога Fail2ban:

![Commit Task2](https://github.com/AndrewZnamenskiy/netsecurity/blob/main/img/task2p2.png)

- Попытки залогинивания заблокированы fail2ban согласно определнным правилам: `/etc/fail2ban/jail.conf`

#### Скриншот лога auth Linux:

![Commit Task2](https://github.com/AndrewZnamenskiy/netsecurity/blob/main/img/task2p3.png)

- В лог попали попытки аутентификации и IP адрес машины с которой производилась атака

#### Скриншот повторного запуска hydra при блокировке fail2ban:

![Commit Task2](https://github.com/AndrewZnamenskiy/netsecurity/blob/main/img/task2p4.png)

- Доступ заблакирован fail2ban


