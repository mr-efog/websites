---
title: Erste Schritte
slug: getting-started
date: 2020-02-11
lastmod: 2023-12-20
---

<div style="display: flex; flex-direction: column; align-items: center; margin-bottom: 15px;">
  <div>Die Let's Encrypt ACME Directory URL ist:</div>
  <div><a href="https://acme-v02.api.letsencrypt.org"><code>https://acme-v02.api.letsencrypt.org/directory</code></a></div>
</div>

Um HTTPS auf Ihrer Website zu aktivieren, brauchen Sie ein Zertifikat (eine Datei) von einer Zertifizierungsstelle (CA). Let's Encrypt ist eine CA. Um für Ihre Domain ein Zertifikat von Let's Encrypt zu bekommen, müssen Sie nachweisen, dass Sie die Kontrolle über diese Domain haben. Mit Let's Encrypt benutzen Sie Software, die das [ACME-Protokoll](https://tools.ietf.org/html/rfc8555) benutzt, welches typischerweise auf Ihrem Web-Host läuft.

Um herauszufinden, welche Methode für Sie die richtige ist, ist es wichtig herauszufinden, ob Sie zu Ihrer Webseite [Shell-Zugang](https://en.wikipedia.org/wiki/Shell_account) haben (auch bekannt als SSH-Zugang). Wenn Sie Ihre Website durch eine Kontrollschnittstelle verwalten wie [cPanel](https://cpanel.net/), [Plesk](https://www.plesk.com/) oder [WordPress](https://wordpress.org/), dann haben Sie wahrscheinlich keinen Shell-Zugang. Sicherheitshalber können Sie Ihren Dienstanbieter fragen.

# Mit Shell-Zugang

Wir empfehlen den meisten Leuten mit Shell-Zugang die Benutzung des ACME [Certbots][]. Er kann Zertifikate automatisch ohne Ausfallzeit erstellen und installieren. Er hat auch einen Expertenmodus für Leute, die keine Autokonfiguration möchten. Er ist einfach zu benutzen, funktioniert auf vielen Betriebssystemen und hat eine großartige Dokumentation. [Besuchen Sie die Seite von Certbot][Certbot], um passende Anleitungen für Ihr Betriebssystem und ihren Server zu erhalten.

Wenn [Certbot][] nicht ihren Anforderungen entspricht, oder Sie einen anderen Client verwenden möchten, gibt es [ viele weitere ACME-Clients zur Auswahl](/docs/client-options).  Sobald Sie einen ACME-Client gewählt haben, können Sie in die dazugehörige Dokumentation verwenden.

Wenn Sie mit unterschiedlichen ACME-Clients experimentieren, benutzen Sie [die Staging-Umgebung](/docs/staging-environment), um zu verhindern, dass die [Anfragelimits](/docs/rate-limits) überschritten werden.

# Ohne Shell-Zugriff

Der beste Weg, Let's Encrypt ohne Shell-Zugriff zu benutzen, ist der eingebaute Support von Ihrem Hosting-Provider. Wenn Ihr Hosting-Provider Let's Encrypt Unterstützung anbietet, dann kann er in Ihrem Namen freie Zertifikate anfordern, installieren und automatisch aktuell halten. Bei einigen Hosting-Providern müssen Sie diese Unterstützung einschalten. Andere Provider machen dies automatisch für ihre Kunden.

[Überprüfen Sie unsere Liste von Hosting-Providern](https://community.letsencrypt.org/t/web-hosting-who-support-lets-encrypt/6920), um zu sehen, ob Ihrer mit dabei ist. Wenn das so ist, folgen Sie der Dokumentation, um Ihr Let's Encrypt-Zertifikat einzurichten.

Wenn Ihr Hosting Provider-Let's Encrypt nicht unterstützt, können Sie ihn kontaktieren und Unterstützung anfragen. Wir tun unser Bestes, um Let's Encrypt-Unterstützung zu ermöglichen und Provider sind oft sehr froh, wenn dieser Vorschlag von ihren Kunden kommt!

Wenn Ihr Hosting-Provider Let's Encrypt nicht integrieren möchte, aber das Hochladen von eigenen Zertifikaten unterstützt, können Sie Certbot auf Ihrem eigenen Rechner installieren und im [manuellen Modus](https://certbot.eff.org/docs/using.html#manual) benutzen. Im manuellen Modus laden Sie eine spezielle Datei auf Ihre Website, um zu beweisen, dass Sie die Kontrolle über diese haben. Certbot wird dann ein Zertifikat abrufen, welches Sie dann zu Ihrem Hosting-Provider hochladen können. Wir empfehlen diese Methode nicht, denn sie kostet viel Zeit und Sie müssen sie mehrmals im Jahr wiederholen, da Ihr Zertifikat abläuft. Für viele Benutzer ist es besser, Let's Encrypt-Unterstützung von ihrem Hosting-Provider anzufordern oder zu einem anderen Anbieter zu wechseln.

# Hilfe erhalten

Wenn Sie Fragen haben zur Auswahl eines ACME-Clients, zur Benutzung eines besonderen Clients oder eine andere Frage bezüglich Let's Encrypt haben, können Sie gerne unser [hilfreiches Community Forum](https://community.letsencrypt.org/) besuchen.

[Certbots]: https://certbot.eff.org/ "Certbot"

[Certbot]: https://certbot.eff.org/ "Certbot"

[Certbot]: https://certbot.eff.org/ "Certbot"
