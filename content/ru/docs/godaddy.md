---
title: "Сертификаты Let's Encrypt на хостинге GoDaddy"
slug: godaddy
date: 2019-12-02
lastmod: 2019-12-02
show_lastmod: 1
---


Нас часто спрашивают - как использовать сертификаты Let’s Encrypt на хостинге GoDaddy? Для shared-хостинга использование сертификата Let’s Encrypt будет крайне затруднительно, поэтому пока не рекомендуется устанавливать наши сертификаты на GoDaddy. Причина в том, что GoDaddy не поддерживает [протокол ACME][1] для автоматического выпуска и обновления сертификатов. Вместо этого, GoDaddy предлагает автоматизированный выпуск собственных сертификатов, [за отдельную плату][2].

Мы не советуем использовать сертификаты Let’s Encrypt у хостинг-провайдеров без полной поддержки протокола ACME, из-за невозможности автоматизации процесса обновления сертификатов. Мы считаем этот момент критичным. Только автоматизированное обновление гарантирует актуальность сертификатов. Сайты с просроченными сертификатами разочаровывают посетителей невозможностью получить доступ к контенту.

Мы уверены в необходимости автоматического обновления, поэтому наши сертификаты спроектированы для автоматизации с помощью ACME. Сертификат Let’s Encrypt должен быть обновлён автоматически спустя 60 дней после выпуска, и будет считаться просроченым по истечении 90 дней.

Если всё же вы решились на использование сертификаты Let’s Encrypt на shared-хостинге GoDaddy (несмотря на описанные выше проблемы) -  у GoDaddy [есть инструкция][3] для вас. Имейте ввиду: работа предстоит кропотливая, и вам придётся выполнять её каждые 60 дней (а не каждые 90 дней, как написано в инструкции по ссылке).

[1]: https://tools.ietf.org/html/rfc8555
[2]: https://www.godaddy.com/web-security/ssl-certificate
[3]: https://www.godaddy.com/help/install-a-lets-encrypt-certificate-on-your-cpanel-hosting-account-28023
