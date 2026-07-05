---
title: 셀프호스팅
description: "자신만의 RustDesk 서버를 자체 호스팅하는 방법을 알아보세요. 안전한 원격 데스크톱 접속을 위한 RustDesk 서버 인프라의 설치, 구성 및 배포를 다룬 완벽한 가이드입니다."
keywords: ["rustdesk self-host", "rustdesk server", "remote desktop server", "self-hosting guide", "rustdesk installation", "hbbs hbbr", "rustdesk pro server"]
weight: 5
pre: "<b>2. </b>"
---

셀프호스팅 RustDesk를 사용하면 자신의 ID 및 릴레이 인프라를 제어하고, 관리하는 시스템에 배포 데이터를 유지하며, 네트워크와 규정 준수 요구 사항에 맞는 원격 접속을 최적화할 수 있습니다.

지원은 우리의 [Discord](https://discord.com/invite/nDceKgxnkV)를 통해 OSS용으로 제공되며, Pro용은 [이메일](mailto:support@rustdesk.com)로 받을 수 있습니다.

## 어떤 RustDesk 서버를 선택해야 하나요?

| Option | Best for | What you get |
| --- | --- | --- |
| [RustDesk Server OSS](/docs/en/self-host/rustdesk-server-oss/) | Individuals and teams that want a free, open-source self-hosted backend | `hbbs` and `hbbr`, community support, manual deployment and configuration |
| [RustDesk Server Pro](/docs/en/self-host/rustdesk-server-pro/) | Businesses that need centralized administration and enterprise features | Web console, API, OIDC, LDAP, 2FA, device management, access control, and multi-relay management |

## 셀프호스팅 서버는 어떻게 작동하나요?

기술적으로 두 개의 실행 파일(서버)이 있습니다:

- `hbbs` - RustDesk ID (rendezvous / 신호 전달) 서버, TCP (`21114` - Pro에서만 http용, `21115`, `21116`, `21118`는 웹소켓용) 및 UDP (`21116`)에서 수신 대기
- `hbbr` - RustDesk 릴레이 서버, TCP (`21117`, `21119`는 웹소켓용)에서 수신 대기

설치 스크립트 / docker compose / deb를 통해 설치할 경우 두 서비스가 모두 설치됩니다.

다음은 [그림](https://github.com/rustdesk/rustdesk/wiki/How-does-RustDesk-work%3F)을 통해 RustDesk 클라이언트가 `hbbr` / `hbbs`와 어떻게 통신하는지 보여줍니다.

RustDesk가 기계에서 실행되는 한, 해당 기계는 지속적으로 ID 서버(`hbbs`)에 핑을 보내 현재 IP 주소와 포트를 알려줍니다.

컴퓨터 A에서 컴퓨터 B로 연결을 시작하면, 컴퓨터 A는 ID 서버에 접속해 컴퓨터 B와 통신하도록 요청합니다.

그런 다음 ID 서버는 홀 펀칭을 이용해 A와 B를 직접 연결하려고 시도합니다.

홀 펀칭이 실패하면, A는 릴레이 서버(`hbbr`)를 통해 B와 통신하게 됩니다.

대부분의 경우 홀 펀칭이 성공적이며, 릴레이 서버는 전혀 사용되지 않습니다.

[셀프호스팅 RustDesk 서버를 선택해야 할까요?](https://www.reddit.com/r/rustdesk/comments/1cr8kfv/should_you_selfhost_a_rustdesk_server/)에 대한 논의가 있습니다.

## 필요 포트

RustDesk 서버 셀프호스팅에 필요한 포트는 대부분 귀하의 환경과 RustDesk를 어떻게 사용하려는지에 따라 달라집니다. 문서 곳곳에 나온 예시에서는 일반적으로 모든 포트를 열도록 권장합니다.

핵심 포트: \
TCP `21114-21119` \
UDP `21116`

위의 `21115-21117`는 RustDesk가 작동하는 데 필요한 최소한의 포트로, 신호 및 릴레이 포트와 NAT 트래버스를 처리합니다.

TCP 포트 `21118`와 `21119`는 [RustDesk 웹 클라이언트](https://rustdesk.com/web/)의 웹소켓 포트입니다. HTTPS 지원을 위해 리버스 프록시가 필요하며, 이에 대한 [샘플 Nginx 구성](/docs/en/self-host/rustdesk-server-pro/faq/#8-add-websocket-secure-wss-support-for-the-id-server-and-relay-server-to-enable-secure-communication-for-the-web-client)을 참고하세요.

SSL 프록시가 없는 Pro 사용자의 경우, API가 작동하도록 TCP 포트 `21114`를 열어야 하며, 대신 SSL 프록시를 사용해 TCP 포트 `443`를 열 수도 있습니다.

{{% children depth="4" showhidden="true" %}}
