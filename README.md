# Домашнее задание к занятию «Уязвимости и атаки на информационные системы»

---

## Ахмадеев Булат Наилевич

---

## Задание 1

![alt text](<images/Снимок экрана 2024-09-07 в 22.34.13.png>)

На основе предоставленного отчета nmap, можно видеть следующие открытые порты и сетевые службы на виртуальной машине Metasploitable:

### Сетевые службы и версии:

1. FTP: vsftpd 2.3.4 (порт 21)
2. SSH: OpenSSH 4.7p1 Debian 8ubuntu1 (порт 22)
3. Telnet: Linux telnetd (порт 23)
4. SMTP: Postfix smtpd (порт 25)
5. DNS: ISC BIND 9.4.2 (порт 53)
6. HTTP: Apache httpd 2.2.8 (Ubuntu) DAV/2 (порт 80)
7. NetBIOS-SSN: Samba smbd 3.x - 4.x (порт 139 и 445)
8. NFS: 2-4 (RPC #100003) (порт 2049)
9. MySQL: MySQL 5.0.51a-3ubuntu5 (порт 3306)
10. PostgreSQL: PostgreSQL DB 8.3.0 - 8.3.7 (порт 5432)
11. VNC: VNC (protocol 3.3) (порт 5900)
12. ProFTPd: ProFTPd 1.3.1 (порт 2121)
13. Apache Tomcat: Apache Tomcat/Coyote JSP engine 1.1 (порт 8180)

### Поиск уязвимостей:

Теперь нужно найти уязвимости для некоторых из этих служб.

1. vsftpd 2.3.4
    * Уязвимость: Backdoor Command Execution
	* Описание: В версии vsftpd 2.3.4 содержится backdoor, позволяющий удаленному атакующему выполнить команды на сервере.
	* Ссылка: Exploit-DB (https://www.exploit-db.com/exploits/17491)
2. ProFTPd 1.3.1
	* Уязвимость: SQL Injection
	* Описание: Уязвимость SQL-инъекции, позволяющая удаленному атакующему выполнить произвольные SQL-запросы.
	* Ссылка: Exploit-DB (https://www.exploit-db.com/exploits/16921)
3. Apache Tomcat/Coyote JSP engine 1.1
	* Уязвимость: Manager Application Default Credentials
	* Описание: Apache Tomcat по умолчанию имеет учетные данные администратора (admin:admin), что позволяет получить полный контроль над сервером.
	* Ссылка: Exploit-DB (https://www.exploit-db.com/exploits/31433)

### Ответ:

Сетевые службы:

* FTP: vsftpd 2.3.4
* SSH: OpenSSH 4.7p1 Debian 8ubuntu1
* Telnet: Linux telnetd
* HTTP: Apache httpd 2.2.8 (Ubuntu) DAV/2
* ProFTPd: ProFTPd 1.3.1
* Apache Tomcat: Apache Tomcat/Coyote JSP engine 1.1

Обнаруженные уязвимости:

1. vsftpd 2.3.4 Backdoor Command Execution - Exploit-DB (https://www.exploit-db.com/exploits/17491)
2. ProFTPd 1.3.1 SQL Injection - Exploit-DB (https://www.exploit-db.com/exploits/16921)
3. Apache Tomcat Manager Application Default Credentials - Exploit-DB (https://www.exploit-db.com/exploits/31433)

Этот отчет показывает обнаруженные сетевые службы и потенциальные уязвимости, что поможет в дальнейшем изучении и защите системы.

## Задание 2

### Сканирование:

1. SYN-сканирование:

![alt text](<images/Снимок экрана 2024-09-07 в 22.49.29.png>)

2. FIN-сканирование:

![alt text](<images/Снимок экрана 2024-09-07 в 22.50.01.png>)

3. Xmas-сканирование:

![alt text](<images/Снимок экрана 2024-09-07 в 22.50.31.png>)

4. UDP-сканирование:

![alt text](<images/Снимок экрана 2024-09-07 в 22.51.00.png>)

### Выборка трафика (ip.src == 192.168.0.112 || ip.dst == 192.168.0.112):

[ФАЙЛ ТРАФИКА WIRESHARK](netology.pcapng)

### Анализ захваченного трафика:

1. SYN Scan: отправляет пакеты с флагом SYN, ответом от сервера будет SYN-ACK (если порт открыт) или RST (если порт закрыт).

2. FIN Scan: отправляет пакеты с флагом FIN, ответ от сервера будет RST (если порт закрыт), если порт открыт, ответа не будет.
3. Xmas Scan: пакеты содержат несколько флагов (FIN, PSH, URG), открытые порты не отвечают, а закрытые отвечают RST.
4. UDP Scan: отправляет UDP-пакеты, если порт открыт, ответа не будет, если порт закрыт, отправляется ICMP сообщение об ошибке (порт недоступен).

### Ответы на вопросы:

1. Чем отличаются эти режимы сканирования с точки зрения сетевого трафика?

* SYN Scan: отправляет SYN-пакеты и ожидает SYN-ACK или RST.
* FIN Scan: отправляет FIN-пакеты и ждет RST (только от закрытых портов).
* Xmas Scan: отправляет пакеты с флагами FIN, PSH, URG; закрытые порты отвечают RST.
* UDP Scan: отправляет UDP-пакеты; ICMP "порт недоступен" возвращается, если порт закрыт.

2. Как отвечает сервер?

* SYN Scan: открытые порты отвечают SYN-ACK, закрытые — RST.
* FIN Scan и Xmas Scan: закрытые порты отвечают RST, открытые — не отвечают.
* UDP Scan: открытые порты не отвечают, закрытые отправляют ICMP сообщение "порт недоступен".

---
