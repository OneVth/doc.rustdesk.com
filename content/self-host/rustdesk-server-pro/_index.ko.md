---
title: RustDesk 서버 프로
description: "RustDesk Server Pro의 완벽한 가이드 - 프리미엄 자체 호스팅 원격 데스크톱 솔루션입니다. 엔터프라이즈 인증(OIDC, LDAP, 2FA), 웹 콘솔, API 액세스 및 전문적인 배포를 위한 고급 보안 제어 기능을 제공합니다."
keywords: ["rustdesk server pro", "rustdesk pro server", "remote desktop server", "enterprise remote access", "rustdesk professional", "self-hosted rdp", "rustdesk enterprise", "remote desktop solution", "rustdesk licensing", "rustdesk web console"]
aliases:
  - /en/self-host/pro/
  - /self-host/pro/
weight: 200
pre: "<b>2.2. </b>"
---

RustDesk 서버 프로는 중앙 집중식 관리, 신원 통합 및 RustDesk 서버 코어를 기반으로 한 고급 제어 기능이 필요한 팀을 위한 상용 자체 호스팅 배포 옵션입니다.

## RustDesk 서버 Pro 빠른 답변

- 중앙 집중식 관리 기능, 신원 통합 또는 정책 제어가 필요할 때 Pro를 선택하세요.
- 가장 빠르고 쉬운 배포를 위해 [Docker](/docs/en/self-host/rustdesk-server-pro/installscript/docker/)를 사용하세요.
- `systemd`와 함께 스크립트화된 Linux 설정을 원한다면 [install.sh](/docs/en/self-host/rustdesk-server-pro/installscript/script/)를 사용하세요.
- [Windows 설치 경로](/docs/en/self-host/rustdesk-server-pro/installscript/windows/)는 이전 버전으로 간주하세요.
- 서버가 시작된 직후 HTTPS, 라이선스 및 클라이언트 구성에 대해 계획하세요.

## RustDesk Server Pro를 선택해야 하는 경우

RustDesk Server OSS만으로 부족하고 다음이 필요할 때 RustDesk Server Pro를 선택하세요:

- 계정
- [웹 콘솔](https://rustdesk.com/docs/en/self-host/rustdesk-server-pro/console/)
- [API](https://github.com/rustdesk/rustdesk/wiki/FAQ#api-of-rustdesk-server-pro)
- [OIDC](https://rustdesk.com/docs/en/self-host/rustdesk-server-pro/oidc/), [LDAP](https://rustdesk.com/docs/en/self-host/rustdesk-server-pro/ldap/), [2FA](https://rustdesk.com/docs/en/self-host/rustdesk-server-pro/2fa/)
- 주소록
- 로그 관리(연결, 파일 전송, 알람 등)
- 디바이스 관리
- [보안 설정 동기화](https://rustdesk.com/docs/en/self-host/rustdesk-server-pro/strategy/)
- [접근 제어](https://rustdesk.com/docs/en/self-host/rustdesk-server-pro/permissions/)
- [다중 릴레이 서버](https://rustdesk.com/docs/en/self-host/rustdesk-server-pro/relay/) (가장 가까운 릴레이를 자동으로 선택함)
- [사용자 지정 클라이언트 생성기](https://rustdesk.com/docs/en/self-host/client-configuration/#1-custom-client-generator-pro-only)
- WebSocket
- 웹 클라이언트 자체 호스팅

{{% notice note %}}
집이나 사무실에 자체 서버를 구축한 경우 공개 IP/도메인을 통해 연결할 수 없는 경우, [이 기사](https://rustdesk.com/docs/en/self-host/nat-loopback-issues/)를 확인해 주세요.
{{% /notice %}}

{{% notice note %}}
계속하기 전에 먼저 이 문서를 읽어 보시는 것을 권장합니다. [자체 호스팅 서버는 어떻게 작동하나요?](/docs/en/self-host/#how-does-self-hosted-server-work).
{{% /notice %}}

## 하드웨어 요구 사항

가장 낮은 수준의 VPS만으로도 사용 사례에 충분합니다. 서버 소프트웨어는 CPU와 메모리 집약적이지 않습니다. 2개의 CPU/4GB Vultr 서버에서 호스팅되는 당사의 공공 ID 서버는 100만 개 이상의 엔드포인트를 지원합니다. 각 릴레이 연결은 초당 평균 180kb를 소비합니다. 1개의 CPU 코어와 1G RAM만으로도 1,000개의 릴레이 동시 연결을 지원할 수 있습니다.

## 기사 자습서
[단계별 가이드: Docker를 통해 클라우드에서 원격 보안 접속을 위한 RustDesk Server Pro 자체 호스팅하기](https://www.linkedin.com/pulse/step-by-step-guide-self-host-rustdesk-server-pro-cloud-montinaro-fwnmf/)

## 동영상 자습서

[초보자 가이드: 초보 리눅스 사용자를 위한 RustDesk 서버 프로 자체 호스팅](https://www.youtube.com/watch?v=MclmfYR3frk)

[빠른 가이드: 고급 리눅스 사용자를 위한 RustDesk 서버 프로 자체 호스팅](https://youtu.be/gMKFEziajmo)

## 라이선스

라이선스는 https://rustdesk.com/pricing.html에서 받을 수 있으며, 자세한 내용은 [라이선스](https://rustdesk.com/docs/en/self-host/rustdesk-server-pro/license/) 페이지를 참조하십시오.

## 시작하기
### 1. 설치

```
bash <(wget -qO- https://get.docker.com)
wget rustdesk.com/pro.yml -O compose.yml
sudo docker compose up -d
```

자세한 내용은 [Docker](/docs/en/self-host/rustdesk-server-pro/installscript/docker/)를 참조하십시오.

### 2. 필요한 포트

`21114`~`21119` TCP와 `21116` UDP 포트가 열려 있어야 하며, 방화벽 규칙과 Docker 포트 매핑을 설정할 때 이들 포트가 정확히 설정되었는지 확인하십시오.

이 포트에 대한 자세한 정보는 [여기](/docs/en/self-host/rustdesk-server-oss/install/#ports)를 참조하십시오.

### 3. 라이선스 설정

`http://<서버 IP>:21114`에 접속하여 웹 콘솔을 엽니다. 기본 인증정보 admin/test1234를 사용해 [로그인](/docs/en/self-host/rustdesk-server-pro/console/#log-in)하십시오. `admin`/`test1234`. [이 안내서](/docs/en/self-host/rustdesk-server-pro/license/#set-license)를 따라 라이선스를 설정하십시오.

### 4. 웹 콘솔용 HTTPS 설정

> 평가판 기간 동안 HTTPS를 사용하지 않으려면 이 단계를 건너뛸 수 있지만, HTTPS를 설정한 후 클라이언트의 API 주소를 변경하는 것을 잊지 마십시오.

[수동 HTTPS 설정](https://rustdesk.com/docs/en/self-host/rustdesk-server-pro/faq/#set-up-https-for-web-console-manually)에 대한 간단한 튜토리얼입니다.

### 5. 자체 호스팅 서버를 사용하도록 클라이언트 구성하기

https://rustdesk.com/docs/en/self-host/client-configuration/

### 6. WebSocket 설정하기

웹 클라이언트 또는 [데스크톱/모바일 클라이언트](/docs/en/self-host/client-configuration/advanced-settings/#allow-websocket)가 WebSocket을 통해 제대로 작동하도록 하려면 리버스 프록시 설정에 다음 항목을 추가해야 합니다.

https://rustdesk.com/docs/en/self-host/rustdesk-server-pro/faq/#8-add-websocket-secure-wss-support-for-the-id-server-and-relay-server-to-enable-secure-communication-for-all-platforms

## 서버 업그레이드

이 [안내서](https://rustdesk.com/docs/en/self-host/rustdesk-server-pro/faq/#there-is-a-new-version-of-rustdesk-server-pro-out-how-can-i-upgrade)는 다양한 설치 방법을 다루며, 이전 버전에서 RustDesk Server Pro를 최신 버전으로 업그레이드하는 방법을 설명합니다.

## 새 호스트로 마이그레이션 및 백업/복원

자세한 [튜토리얼](https://github.com/rustdesk/rustdesk-server-pro/discussions/184)은 여기에 있습니다.

## 라이선스 이전

이 [안내서](https://rustdesk.com/docs/en/self-host/rustdesk-server-pro/license/#invoices-license-retrieval-and-migration)를 따르십시오.

## 라이선스 업그레이드

[이 안내서](/docs/en/self-host/rustdesk-server-pro/license/#renewupgrade-license)를 따라 언제든지 더 많은 사용자와 기기를 위한 라이선스를 업그레이드하십시오.

## 보안 정보

https://github.com/rustdesk/rustdesk/discussions/9835
