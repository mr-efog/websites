---
title: Zertifikate für localhost
slug: certificates-for-localhost
date: 2017-12-21
lastmod: 2017-12-21
show_lastmod: 1
---


Manchmal möchten Leute ein Zertifikat für den Hostnamen "localhost" bekommen, entweder um ihre lokale Entwicklungsumgebung zu nutzen oder zur Verteilung einer nativen Applikation, die Kommunikation mit einer Webapplikation benötigt. Let's Encrypt kann keine Zertifikate für "localhost" zur Verfügung stellen, da es keinen alleinigen Eigentümer gibt und es nicht zu einer Toplevel-Domain gehört, wie ".com" oder ".net". Es ist möglich, ihre eigene Domain so zu konfigurieren, dass sie auf `127.0.0.1` zeigt und hierfür ein Zertifkat erhalten, welches in der DNS-Challenge verwendet werden kann. Aber dies es nicht empfehlenswert und es gibt es bessere Optionen.

# Für lokale Entwicklungsumgebungen

Wenn Sie eine Webapp entwickeln, ist es nützlich einen lokalen Webserver wie Apache oder Ngninx laufen zu lassen und auf ihn über `http://localhost:8000/` in Ihrem Browser zuzugreifen. Webbrowser haben ein leicht unterschiedliches Vorgehen, um HTTP- und HTTPS-Seiten zu erreichen. Der große Unterschied: Auf HTTPS-Seiten wird jede Anfrage zum Laden von Javascript über eine HTTP-URL blockiert. Wenn Sie also lokal unter HTTP entwickeln, kann ein hinzugefügtes Script lokal unter HTTP funktionieren, aber nicht auf der veröffentlichten HTTPS-Seite. Um diesem Problem vorzubeugen, ist es sinnvoll, HTTPS auf ihrem lokalen Webserver zu verwenden. Andererseits möchte man nicht durchgehend Zertifikatswarnungen sehen. Wie erhält man also "das grüne Schloss-Symbol"?

Die beste Option: Erstellen Sie Ihr eigenes Zertifikat, entweder selbstsigniert oder signiert bei einer lokalen Root-Stelle und vertrauen Sie diesem im Trust-Store Ihres Betriebssystems. Dann benutzen Sie dieses Zertifikat in Ihrem lokalen Webserver. Schauen Sie weiter unten für Details.

# Für native Apps, die mit Webapps kommunizieren

Manchmal möchten Entwickler native Apps zum Herunterladen anbieten, die in einer Webseite implementiert werden können, um mehr Funktionsumfang zu bieten. Beispielsweise scannen die Dropbox- und Spotify-Desktop-App nach Dateien in Ihrem System, was eine Webapp nicht erlauben würde. Eine übliche Herangehensweise ist das Zwischenschalten eines Webservices auf localhost, damit die Webapp Anfragen über XMLHTTPRequest (XHR) oder WebSockets stellen kann. Die Webapp benutzt meistens schon HTTPS, was bedeutet, dass der Browser es verbieten würde, Anfragen über unsichere URLs an XHR oder Websockets zu stellen. Das wird auch Mixed-Content-Blocking genannt. Damit die Webapp mit der nativen App kommunizieren kann, wird ein sicherer Webservice benötigt.

Glücklicherweise [betrachten moderne Browser][mcb-localhost] `http://127.0.0.1:8000/` als eine ["potentiell sichere"][secure-contexts] URL, weil sie zu einer Loopback Adresse referenziert. Netzwerkverkehr, der zu `127.0.0.1` gesendet wird, verlässt garantiert nicht Ihr System und so wird es automatisch als sicher gegen Netzwerkangriffe betrachtet. Das bedeutet, wenn Ihre Webapp HTTPS ist und Sie bieten einer nativen App einen Webservice auf `127.0.0.1` an, so können diese fröhlich über XHR kommunizieren. Leider bekommt [localhost noch nicht die gleiche Behandlung][let-localhost]. WebSockets erhalten diese Behandlung auch nicht für einen der beiden Namen.

Sie könnten versucht sein, um diese Limitierung herumzuarbeiten durch die Einrichtung eines globalen DNS Eintrag, der auf `127.0.0.1` auflöst (z.B. `localhost.example.com`), dafür ein Zertifikat zu bekommen, dieses zusammen mit dem privaten Schlüssel mit Ihrer nativen App zu verschicken und der Webapp zu erklären, statt `https://127.0.0.1:8000` mit `https://localhost.example.com:8000/` zu kommunizieren. *Machen Sie das nicht!* Der Benutzer wird nur unnötigen Risiken ausgesetzt und das Zertifikat könnte widerrufen werden.

Bei Einführung eines Domainnamens anstatt einer IP-Adresse machen Sie eine "Man in the Middle (MitM)" Attacke möglich, indem bei der DNS-Anfrage eine falsche Rückantwort, die zu einer anderen IP-Adresse zeigt, injiziert wird. Der Angreifer kann dann so tun als ob er die lokale App ist und gefälschte Anfragen zurück zur Webapp senden, was dann möglicherweise die Komprimitierung Ihres Accounts auf der Webapp-Seite zur Folge haben kann, abhängig wie es implementiert ist.

Der erfolgreiche MitM in dieser Situation ist möglich, weil Sie den privaten Schlüssel zu Ihrem Zertifikat mit Ihrer systemeigenen Anwendung übermitteln mussten, damit er funktioniert. Das bedeutet, dass jeder, der Ihre native Anwendung herunterlädt, eine Kopie des privaten Schlüssels erhält, auch der Angreifer. Das wird als Komprimitierung Ihres privaten Schlüssels angesehen und Ihre Certificate Authority (CA) ist angewiesen, Ihr Zertifikat zu sperren, sobald sie Information dazu erhält. [Viele native apps][mdsp1] haben [ihr eigenes Zertifikat][mdsp2][vorm Versenden des privaten Schlüssels][mdsp3] gesperrt.

Leider sind für native Apps keine guten und sicheren Kommunikationsmöglichkeiten mit der entsprechenden Website vorhanden. In der Zukunft wird die Situation möglicherweise schwieriger, wenn Browser [den Zugriff auf localhost über das Internet weiter verschärfen][tighten-access].

Beachten Sie auch, dass das Exportieren eines Webdiensts, der privilegierte native APIs bietet, von Natur aus riskant ist, da auf Websites, die Sie nicht autorisieren wollten, möglicherweise zugegriffen wird. Wenn Sie diese Route durchgehen, informieren Sie sich über [Cross-OriginResource-Freigabe][cors], verwenden Sie Access-Control-Allow-Origin und stellen Sie sicher, dass Sie einen speicher-sicheren HTTP-Parser verwenden, da jede Quelle, der Sie keinen Zugriff erlauben, Preflight-Anforderungen senden kann, wodurch möglicherweise Fehler in Ihrem Parser ausgenutzt werden.

# Erstellen und Vertrauen Ihrer eigenen Zertifikate

Jeder kann seine eigene Zertifikate ohne Hilfe einer CA machen. Der einzige Untersschied ist, dass von Ihnen selbst erstellte Zertifikate von niemanden vertraut werden. Für lokale Entwicklungsumgebungen ist das fein.

Der einfachste Weg, um einem privaten Schlüssel und ein selbst-signiertes Zertifikat für localhost zu generieren, ist dieses openssl Kommando:

    openssl req -x509 -out localhost.crt -keyout localhost.key \
      -newkey rsa:2048 -nodes -sha256 \
      -subj '/CN=localhost' -extensions EXT -config <( \
       printf "[dn]\nCN=localhost\n[req]\ndistinguished_name = dn\n[EXT]\nsubjectAltName=DNS:localhost\nkeyUsage=digitalSignature\nextendedKeyUsage=serverAuth")

Sie können Ihren lokalen Webserver mit localhost.crt und localhost.key konfigurieren und installieren localhost.crt in Ihrer Liste von lokalen vertrauenden Roots.

Wenn Sie etwas mehr Realität in Ihre Entwicklerzertifikate bringen möchten, können Sie auch [minica][minica] zur Generierung Ihres eigenen lokalen Root Zertifikat benutzen und Endzertifikate ausstellen. Sie würden dann das Root Zertifikat importieren, bevor selbst-signierte Endzertifikate importiert werden.

Sie können auch eine Domain mit Punkten wählen, wie `www.localhost`, und diese in /etc/hosts als Alias für `127.0.0.1` hinzufügen. Dies ändert auf subtile Weise, wie Browser mit der Speicherung von Cookies umgehen.

[mcb-localhost]: https://bugs.chromium.org/p/chromium/issues/detail?id=607878
[secure-contexts]: https://www.w3.org/TR/secure-contexts/#is-origin-trustworthy
[let-localhost]: https://tools.ietf.org/html/draft-ietf-dnsop-let-localhost-be-localhost-02
[mdsp1]: https://groups.google.com/d/msg/mozilla.dev.security.policy/eV89JXcsBC0/wsj5zpbbAQAJ
[mdsp2]: https://groups.google.com/d/msg/mozilla.dev.security.policy/T6emeoE-lCU/-k-A2dEdAQAJ
[mdsp3]: https://groups.google.com/d/msg/mozilla.dev.security.policy/pk039T_wPrI/tGnFDFTnCQAJ
[tighten-access]: https://bugs.chromium.org/p/chromium/issues/detail?id=378566
[minica]: https://github.com/jsha/minica
[cors]: https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS
