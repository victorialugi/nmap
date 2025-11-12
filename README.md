# Домашнее задание к занятию "`Уязвимости и атаки на информационные системы`" - `Лугинина Виктория`


### Инструкция по выполнению домашнего задания

   1. Сделайте `fork` данного репозитория к себе в Github и переименуйте его по названию или номеру занятия, например, https://github.com/имя-вашего-репозитория/git-hw или  https://github.com/имя-вашего-репозитория/7-1-ansible-hw).
   2. Выполните клонирование данного репозитория к себе на ПК с помощью команды `git clone`.
   3. Выполните домашнее задание и заполните у себя локально этот файл README.md:
      - впишите вверху название занятия и вашу фамилию и имя
      - в каждом задании добавьте решение в требуемом виде (текст/код/скриншоты/ссылка)
      - для корректного добавления скриншотов воспользуйтесь [инструкцией "Как вставить скриншот в шаблон с решением](https://github.com/netology-code/sys-pattern-homework/blob/main/screen-instruction.md)
      - при оформлении используйте возможности языка разметки md (коротко об этом можно посмотреть в [инструкции  по MarkDown](https://github.com/netology-code/sys-pattern-homework/blob/main/md-instruction.md))
   4. После завершения работы над домашним заданием сделайте коммит (`git commit -m "comment"`) и отправьте его на Github (`git push origin`);
   5. Для проверки домашнего задания преподавателем в личном кабинете прикрепите и отправьте ссылку на решение в виде md-файла в вашем Github.
   6. Любые вопросы по выполнению заданий спрашивайте в чате учебной группы и/или в разделе “Вопросы по заданию” в личном кабинете.
   
Желаем успехов в выполнении домашнего задания!
   
### Дополнительные материалы, которые могут быть полезны для выполнения задания

1. [Руководство по оформлению Markdown файлов](https://gist.github.com/Jekins/2bf2d0638163f1294637#Code)

---

### Задание 1

`Какие сетевые службы в Metasploitable 2 разрешены?

Открытые TCP-порты и службы:

21/tcp: FTP (vsftpd 2.3.4)
22/tcp: SSH (OpenSSH 4.7p1)
23/tcp: Telnet
25/tcp: SMTP (Postfix 2.5.5)
53/tcp/udp: DNS (BIND 9.4.2)
80/tcp: HTTP (Apache 2.2.8, PHP 5.2.4)
111/tcp/udp: RPC (rpcbind)
139/445/tcp: Samba (3.0.20)
512-514/tcp: rexd, login, shell
1099/tcp: Java RMI
1524/tcp: Ingreslock
2049/tcp: NFS
2121/tcp: ccproxy-ftp
3306/tcp: MySQL (5.0.51a)
5432/tcp: PostgreSQL (8.3.0)
5900/tcp: VNC (RealVNC 4.1.1)
6000/tcp: X11
6667/tcp: IRC (UnrealIRCd 3.2.8.1)
8009/tcp: Apache JServ (1.3)
8180/tcp: Apache Tomcat (5.5.9)
8787/tcp: distccd
UDP-порты: 53, 111, 123 (NTP), 161 (SNMP).

Какие уязвимости были вами обнаружены?

1. vsftpd 2.3.4 Backdoor (порт 21, FTP): Уязвимость в версии 2.3.4 позволяет выполнить произвольный код через backdoor (CVE-2011-2523). Эксплойт: shell-команда с символом "|".
https://www.exploit-db.com/exploits/17491
2. Samba Username Map Script Command Execution (порты 139/445, Samba): Уязвимость в Samba 3.0.20-3.0.25rc3 позволяет выполнить команды через "username map script" (CVE-2007-6015). Эксплойт: отправка malicious username.
https://www.exploit-db.com/exploits/4612
3. UnrealIRCd Backdoor Command Execution (порт 6667, IRC): Backdoor в UnrealIRCd 3.2.8.1 позволяет выполнить shell-команды (CVE-2009-4391). Эксплойт: отправка "AB" + команда.
https://www.exploit-db.com/exploits/9594`

---

### Задание 2

`

`
