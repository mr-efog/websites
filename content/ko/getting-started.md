---
title: 시작하기
slug: getting-started
date: 2020-02-11
---

웹 사이트에서 HTTPS를 사용하려면 CA(인증 기관)에서 인증서(파일 유형)를 가져와야 합니다. Let's Encrypt는 CA입니다. Let's Encrypt에서 웹 사이트 도메인에 대한 인증서를 받으려면 도메인에 대한 제어를 시연해야 합니다. Let's Encrypt에서는 일반적으로 웹 호스트에서 실행되는 [ACME 프로토콜](https://tools.ietf.org/html/rfc8555)을 사용하는 소프트웨어를 사용하여 이 작업을 수행합니다.

어떤 방법이 가장 적합한지 알아보려면 웹 호스트에 [쉘 접근](https://en.wikipedia.org/wiki/Shell_account)(SSH 액세스라고도 함)이 있는지 알아야 합니다. [cPanel](https://cpanel.net/), 또는 [워드프레스](https://wordpress.org/)와 같은 제어판을 통해 웹 사이트를 완전히 관리하는 경우 셸 액세스 권한이 없을 가능성이 높습니다. 호스팅 공급자에게 확인을 요청할 수 있습니다.

# 쉘 엑세스 권한이 있는 경우

쉘 액세스 권한이 있는 대부분의 사용자는 [Certbot][] ACME 클라이언트를 사용하는 것이 좋습니다. 다운타임 없이 인증서 발급 및 설치를 자동화할 수 있습니다. 또한 자가변성을 원하지 않는 사람들을 위한 전문적 모드도 갖추고 있습니다. 사용하기 쉽고, 많은 운영 체제에서 작동하며, 문서화가 매우 우수합니다. [ Certbot 웹 사이트 방문하기][Certbot]은(는) 운영 체제 및 웹 서버에 대한 사용자 지정 지침을 제공합니다.

[Certbot][]이 사용자의 요구를 충족하지 못하거나 다른 방법을 시도하려는 경우 [많은 ACME 클라이언트를 선택할 수 있습니다](/docs/client-options).  ACME 클라이언트 소프트웨어를 선택한 후 계속하려면 해당 클라이언트의 설명서를 참조합니다.

다른 ACME 클라이언트를 사용하는 경우, [준비 환경](/docs/staging-environment)을 사용하여 [속도 제한](/docs/rate-limits)에 도달하지 않도록 합니다.

# 쉘 엑세스 권한이 없는 경우

쉘 액세스 없이 암호화하는 가장 좋은 방법은 호스팅 공급자의 기본 제공 지원을 사용하는 것입니다. 호스트 공급자가 Let's Encrypt 지원을 제공하는 경우 사용자 대신 무료 인증서를 요청하고 설치하여 자동으로 최신 상태로 유지할 수 있습니다. 일부 호스팅 공급자의 경우 이 설정을 사용해야 합니다. 다른 공급자는 모든 고객에 대한 인증서를 자동으로 요청하고 설치합니다.

[호스팅 제공자 목록을 확인하여](https://community.letsencrypt.org/t/web-hosting-who-support-lets-encrypt/6920) 귀하의 호스팅이 있는지 확인합니다. 있다면 해당 문서에 따라 Let's Encrypt 인증서를 설정합니다.

호스팅 제공 업체가 Let's Encrypt를 지원하지 않는 경우 해당 공급 업체에 문의하여 지원을 요청할 수 있습니다. 우리는 Let's Encrypt 지원을 쉽게 받을 수 있도록 최선을 다하며, 제공자는 종종 고객의 제안을 듣고 기뻐합니다!

호스트 공급자가 Let's Encrypt를 통합하지 않고 사용자 지정 인증서 업로드를 지원하는 경우 Certbot을 직접 설치할 수 있습니다. 컴퓨터를 사용하여 [수동 모드](https://certbot.eff.org/docs/using.html#manual)에서 사용합니다. 수동 모드에서는 특정 파일을 웹 사이트에 업로드하여 제어를 증명할 수 있습니다. 그런 다음 Certbot이 호스트 공급자에게 업로드할 수 있는 인증서를 검색합니다. 이 옵션은 시간이 오래 걸리고 인증서가 만료될 때 1년에 여러 번 반복해야 하기 때문에 권장하지 않습니다. 대부분의 경우 호스트 공급자에게 Let's Encrypt 지원을 요청하거나, 이를 구현할 계획이 없는 경우 공급자를 전환하는 것이 좋습니다.

# 도움말

ACME 클라이언트 선택 또는 특정 클라이언트 사용 또는 Let's Encrypt와 관련된 기타 사항에 대한 질문이 있는 경우, [도움말 커뮤니티 포럼](https://community.letsencrypt.org/)을 사용해 주십시오.

[Certbot]: https://certbot.eff.org/ "Certbot"

[Certbot]: https://certbot.eff.org/ "Certbot"
