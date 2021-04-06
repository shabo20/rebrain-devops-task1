<a id="anchor"></a>

# NGINX

---
## _Поговорим немного о вєб сервере NGINX_

1. #### __Описание__
2. #### __Основная функциональность HTTP-сервера__
3. #### __Другие возможности HTTP-сервера__
4. #### __Функциональность почтового прокси сервера__
5. #### __Функциональность TCP/UDP прокси-сервера__
6. #### __Архитектура и масштабируемость__
7. #### __Протестированные ОС и платформы__
---

---

1. #### __Описание__

nginx [engine x] — это HTTP-сервер и обратный прокси-сервер, почтовый прокси-сервер, а также TCP/UDP прокси-сервер общего назначения, изначально написанный [Игорем Сысоевым](http://sysoev.ru/).Уже длительное время он обслуживает серверы многих высоконагруженных российских сайтов, таких как [Яндекс](www.yandex.ru/), [Mail.Ru](https://mail.ru/), [ВКонтакте](https://vk.com/) и [Рамблер](https://www.rambler.ru/) и других не мало известных компаний. Согласно статистике Netcraft nginx обслуживал или проксировал 23.06% самых нагруженных сайтов в марте 2021 года. Вот некоторые примеры успешного внедрения nginx (тексты на английском языке): [Dropbox](https://dropbox.tech/infrastructure/optimizing-web-servers-for-high-throughput-and-low-latency), [Netflix](https://openconnect.netflix.com/en/appliances/#software), [Wordpress.com](https://www.nginx.com/success-stories/nginx-wordpress-com/), [FastMail.FM](https://fastmail.blog/2007/01/04/webimappop-frontend-proxies-changed-to-nginx/).

---

---

2. #### __Основная функциональность HTTP-сервера__

*   Обслуживание статических запросов, [индексных файлов](https://nginx.org/ru/docs/http/ngx_http_index_module.html#index), [автоматическое создание списка файлов](https://nginx.org/ru/docs/http/ngx_http_autoindex_module.html), [кэш дескрипторов открытых файлов](https://nginx.org/ru/docs/http/ngx_http_core_module.html#open_file_cache);
*    [Акселерированное обратное проксирование с кэшированием](https://nginx.org/ru/docs/http/ngx_http_proxy_module.html), [распределение нагрузки и отказоустойчивость](https://nginx.org/ru/docs/http/ngx_http_upstream_module.html);
*    Акселерированная поддержка [FastCGI](https://nginx.org/ru/docs/http/ngx_http_fastcgi_module.html), [uwsgi](https://nginx.org/ru/docs/http/ngx_http_uwsgi_module.html), [SCGI](https://nginx.org/ru/docs/http/ngx_http_scgi_module.html) и [memcached](https://nginx.org/ru/docs/http/ngx_http_memcached_module.html) серверов с кэшированием, [распределение нагрузки и отказоустойчивость](https://nginx.org/ru/docs/http/ngx_http_upstream_module.html);
*    Модульность, фильтры, в том числе [сжатие (gzip)](https://nginx.org/ru/docs/http/ngx_http_gzip_module.html), byte-ranges (докачка), chunked ответы, [XSLT-фильтр](https://nginx.org/ru/docs/http/ngx_http_xslt_module.html), [SSI-фильтр](https://nginx.org/ru/docs/http/ngx_http_ssi_module.html), [преобразование изображений](https://nginx.org/ru/docs/http/ngx_http_image_filter_module.html); несколько подзапросов на одной странице, обрабатываемые в SSI-фильтре через прокси или FastCGI/uwsgi/SCGI, выполняются параллельно;
*    [Поддержка SSL и расширения TLS SNI](https://nginx.org/ru/docs/http/ngx_http_ssl_module.html);
*    Поддержка [HTTP/2](https://nginx.org/ru/docs/http/ngx_http_v2_module.html) с приоритизацией на основе весов и зависимостей.

---

---

3. #### __Другие возможности HTTP-сервера__

*    [Виртуальные серверы](https://nginx.org/ru/docs/http/request_processing.html), определяемые по IP-адресу и имени;
*    Поддержка [keep-alive](https://nginx.org/ru/docs/http/ngx_http_core_module.html#keepalive_timeout) и pipelined соединений;
*    [Настройка форматов логов](https://nginx.org/ru/docs/http/ngx_http_log_module.html#log_format), [буферизованная запись в лог](https://nginx.org/ru/docs/http/ngx_http_log_module.html#access_log), [быстрая ротация логов](https://nginx.org/ru/docs/control.html#logs), [запись в syslog](https://nginx.org/ru/docs/syslog.html);
*    [Специальные страницы](https://nginx.org/ru/docs/http/ngx_http_core_module.html#error_page) для ошибок 3xx-5xx;
*    rewrite-модуль: [изменение URI с помощью регулярных выражений](https://nginx.org/ru/docs/http/ngx_http_rewrite_module.html);
*    [Выполнение разных функций](https://nginx.org/ru/docs/http/ngx_http_rewrite_module.html#if) в зависимости от [адреса клиента](https://nginx.org/ru/docs/http/ngx_http_geo_module.html);
*    Ограничение доступа в зависимости от [адреса клиента](https://nginx.org/ru/docs/http/ngx_http_access_module.html), [по паролю (HTTP Basic аутентификация)](https://nginx.org/ru/docs/http/ngx_http_auth_basic_module.html) и по [результату подзапроса](https://nginx.org/ru/docs/http/ngx_http_auth_request_module.html);
*    Проверка [HTTP referer](https://nginx.org/ru/docs/http/ngx_http_referer_module.html);
*    [Методы PUT, DELETE, MKCOL, COPY и MOVE](https://nginx.org/ru/docs/http/ngx_http_dav_module.html);
*    [FLV](https://nginx.org/ru/docs/http/ngx_http_flv_module.html) и [MP4](https://nginx.org/ru/docs/http/ngx_http_mp4_module.html) стриминг;
*    [Ограничение скорости отдачи ответов](https://nginx.org/ru/docs/http/ngx_http_core_module.html#limit_rate);
*    Ограничение числа одновременных [соединений](https://nginx.org/ru/docs/http/ngx_http_limit_conn_module.html) и [запросов](https://nginx.org/ru/docs/http/ngx_http_limit_req_module.html) с одного адреса;
*    [Геолокация по IP-адресу](https://nginx.org/ru/docs/http/ngx_http_geoip_module.html);
*    [A/B-тестирование](https://nginx.org/ru/docs/http/ngx_http_split_clients_module.html);
*    [Зеркалирование запросов](https://nginx.org/ru/docs/http/ngx_http_mirror_module.html);
*    Встроенный [ _Perl_ ](https://nginx.org/ru/docs/http/ngx_http_perl_module.html);
*    сценарный язык [ _njs_ ](https://nginx.org/ru/docs/njs/index.html).

---

---

4. #### __Функциональность почтового прокси сервера__

Функциональность почтового прокси-сервера

*    Перенаправление пользователя на [IMAP-](https://nginx.org/ru/docs/mail/ngx_mail_imap_module.html) или [POP3](https://nginx.org/ru/docs/mail/ngx_mail_pop3_module.html)-сервер с использованием внешнего HTTP-сервера [аутентификации](https://nginx.org/ru/docs/mail/ngx_mail_auth_http_module.html);
*    Проверка пользователя с помощью внешнего HTTP-сервера [аутентификации](https://nginx.org/ru/docs/mail/ngx_mail_auth_http_module.html) и перенаправление соединения на внутренний [SMTP](https://nginx.org/ru/docs/mail/ngx_mail_smtp_module.html)-сервер;
*    Методы аутентификации:
	* [POP3](https://nginx.org/ru/docs/mail/ngx_mail_pop3_module.html#pop3_auth): USER/PASS, APOP, AUTH LOGIN/PLAIN/CRAM-MD5;
	* [IMAP](https://nginx.org/ru/docs/mail/ngx_mail_imap_module.html#imap_auth): LOGIN, AUTH LOGIN/PLAIN/CRAM-MD5;
	* [SMTP](https://nginx.org/ru/docs/mail/ngx_mail_smtp_module.html#smtp_auth): AUTH LOGIN/PLAIN/CRAM-MD5;
*    Поддержка [SSL](https://nginx.org/ru/docs/mail/ngx_mail_ssl_module.html);
*    Поддержка [STARTTLS и STLS](https://nginx.org/ru/docs/mail/ngx_mail_ssl_module.html#starttls).

---

---

5. #### __Функциональность TCP/UDP прокси-сервера__


*    [Проксирование TCP и UDP](https://nginx.org/ru/docs/stream/ngx_stream_proxy_module.html);
*    Поддержка [SSL](https://nginx.org/ru/docs/stream/ngx_stream_ssl_module.html) и расширения [TLS SNI](https://nginx.org/ru/docs/stream/ngx_stream_ssl_preread_module.html) для TCP;
*    [Распределение нагрузки и отказоустойчивость](https://nginx.org/ru/docs/stream/ngx_stream_upstream_module.html);
*    Ограничение доступа в зависимости от [адреса клиента](https://nginx.org/ru/docs/stream/ngx_stream_access_module.html);
*    Выполнение разных функций в зависимости от [адреса клиента](https://nginx.org/ru/docs/http/ngx_http_geo_module.html);
*    Ограничение числа одновременных [соединений](https://nginx.org/ru/docs/stream/ngx_stream_limit_conn_module.html) с одного адреса;
*    [Настройка форматов логов](https://nginx.org/ru/docs/stream/ngx_stream_log_module.html#log_format), [буферизованная запись в лог](https://nginx.org/ru/docs/stream/ngx_stream_log_module.html#access_log), [быстрая ротация логов](https://nginx.org/ru/docs/control.html#logs), [запись в syslog](https://nginx.org/ru/docs/syslog.html);
*    [Геолокация по IP-адресу](https://nginx.org/ru/docs/stream/ngx_stream_geoip_module.html);
*    [A/B-тестирование](https://nginx.org/ru/docs/stream/ngx_stream_split_clients_module.html);
*    сценарный язык [njs](https://nginx.org/ru/docs/njs/index.html).

---


---

6. #### __Архитектура и масштабируемость__


*    Один главный и несколько рабочих процессов, рабочие процессы работают под непривилегированным пользователем;
*    [Гибкость конфигурации](https://nginx.org/ru/docs/example.html);
*    [Изменение настроек](https://nginx.org/ru/docs/control.html#reconfiguration) и [обновление исполняемого](https://nginx.org/ru/docs/control.html#upgrade) файла без перерыва в обслуживании клиентов;
*    [Поддержка](https://nginx.org/ru/docs/events.html) kqueue (FreeBSD 4.1+), epoll (Linux 2.6+), /dev/poll (Solaris 7 11/99+), event ports (Solaris 10), select и poll;
*    Использование возможностей, предоставляемых kqueue, таких как EV_CLEAR, EV_DISABLE (для временного выключения события), NOTE_LOWAT, EV_EOF, число доступных данных, коды ошибок;
*    Использование возможностей, предоставляемых epoll, таких как EPOLLRDHUP (Linux 2.6.17+, glibc 2.8+) и EPOLLEXCLUSIVE (Linux 4.5+, glibc 2.24+);
*    Поддержка sendfile (FreeBSD 3.1+, Linux 2.2+, macOS 10.5+), sendfile64 (Linux 2.4.21+) и sendfilev (Solaris 8 7/01+);
*    [Поддержка файлового AIO](https://nginx.org/ru/docs/http/ngx_http_core_module.html#aio) (FreeBSD 4.3+, Linux 2.6.22+);
*    Поддержка [DIRECTIO](https://nginx.org/ru/docs/http/ngx_http_core_module.html#directio) (FreeBSD 4.4+, Linux 2.4+, Solaris 2.6+, macOS);
*    [Поддержка](https://nginx.org/ru/docs/http/ngx_http_core_module.html#listen) accept-фильтров (FreeBSD 4.1+, NetBSD 5.0+) и TCP_DEFER_ACCEPT (Linux 2.4+);
*    На 10 000 неактивных HTTP keep-alive соединений расходуется около 2.5M памяти;
*    Минимум операций копирования данных.


---

---

7. #### __Протестированные ОС и платформы__


*    FreeBSD 3 — 12 / i386; FreeBSD 5 — 12 / amd64; FreeBSD 11 / ppc; FreeBSD 12 / ppc64;
*    Linux 2.2 — 4 / i386; Linux 2.6 — 5 / amd64; Linux 3 — 4 / armv6l, armv7l, aarch64, ppc64le;
*    Solaris 9 / i386, sun4u; Solaris 10 / i386, amd64, sun4v; Solaris 11 / x86;
*    AIX 7.1 / powerpc;
*    HP-UX 11.31 / ia64;
*    macOS / ppc, i386, x86_64;
*    Windows XP, Windows Server 2003, Windows 7, Windows 10.

---


### ___LICENSING___

* ___Исходные тексты и документация распространяются под___ [BSD-подобной лицензией из 2 пунктов](https://nginx.org/LICENSE).

* ___Коммерческая поддержка осуществляется компанией___ [Nginx, Inc](https://nginx.org/LICENSE). 


## Logo test

![Проверка вставки логотипа](Mortal_kombat_logo.png)

[Вверх](#anchor)


