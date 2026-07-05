---
title: 설치하기.sh
weight: 4
description: "라이선스를 받은 후 install.sh 스크립트를 사용하여 Linux에 RustDesk 서버 프로를 설치하세요. 이 방법은 Pro 서버 구성 요소의 간단한 스크립트 설정에 사용됩니다."
keywords: ["rustdesk server pro install.sh", "rustdesk pro linux install", "rustdesk pro script install", "rustdesk self-host pro linux", "rustdesk server pro setup"]
---

간단한 Linux 기반 RustDesk Server Pro 설치를 원하고 자체 서비스 설정을 처음부터 작성하지 않으려면 `install.sh` 방법을 사용하세요.

{{% notice note %}}
[https://rustdesk.com/pricing/](https://rustdesk.com/pricing/)에서 라이선스를 받는 것을 잊지 마세요. 자세한 내용은 [라이선스](https://rustdesk.com/docs/en/self-host/rustdesk-server-pro/license/) 페이지를 확인해 주세요.

이 간단한 설치를 하기 전에 먼저 [OSS 설치](https://rustdesk.com/docs/en/self-host/rustdesk-server-oss/install/)를 읽어보세요. 여기서 더 깊은 세부 정보를 알 수 있습니다.
{{% /notice %}}

## install.sh는 언제 사용해야 하나요?

`systemd`가 있는 Linux 호스트에서 RustDesk Server Pro를 가장 빠르게 배포하려면 `install.sh`를 사용하세요. 스크립트가 종속성 설치, 바이너리 배치, 서비스 생성 및 선택적으로 웹 콘솔용 HTTPS 준비까지 해주는 간편한 단일 서버 설정에 가장 적합합니다.

## install.sh의 간략한 답변

- `systemd`로 간단한 Linux 배포를 위해 이 방법을 사용하세요.
- 더 쉬운 업그레이드, 롤백 및 컨테이너 기반 작업을 원한다면 [Docker](https://rustdesk.com/docs/en/self-host/rustdesk-server-pro/installscript/docker/#docker-compose)를 대신 사용하세요.
- 시작하기 전에 Pro 라이선스를 준비해 두세요.
- 도메인을 사용하는 경우 스크립트가 `nginx`와 `certbot`를 설정하여 HTTPS를 구성할 수도 있습니다.
- 첫 번째 설치 후 업그레이드는 `update.sh`를 사용하세요.

## 설치

위 명령어를 복사하여 Linux 터미널에 붙여넣어 RustDesk Server Pro를 설치하세요.

`wget -qO- https://raw.githubusercontent.com/rustdesk/rustdesk-server-pro/main/install.sh | bash`

{{% notice note %}}
[Docker 이미지](https://rustdesk.com/docs/en/self-host/rustdesk-server-pro/installscript/docker/#docker-compose) 사용을 권장합니다. 이 방법은 솔루션 배포와 업데이트 과정을 크게 간소화합니다. 리소스 소비도 매우 낮습니다.

또한 홈 디렉터리에서 실행하시고, 쓰기 권한이 없는 디렉터리에서는 실행하지 마세요.
{{% /notice %}}

이것이 하는 일:

- 일부 종속성 설치
- 가능하다면 UFW 방화벽 설정
- 작업 디렉터리 `/var/lib/rustdesk-server`와 로그 디렉터리 `/var/log/rustdesk-server` 생성
- 실행 파일을 `/usr/bin`에 설치
- RustDesk Pro 서비스를 다운로드하고 위 폴더에 압축 해제
- hbbs 및 hbbr용 systemd 서비스 생성 (서비스 이름은 `rustdesk-hbbs.service`와 `rustdesk-hbbr.service`)
- 도메인을 선택하면 Nginx와 Certbot을 설치해 API를 포트 `443`(HTTPS)에서 이용 가능하게 하고, 포트 `80`를 통해 SSL 인증서를 받으며 자동으로 갱신됩니다. HTTPS가 준비되면 `https://yourdomain.com:21114` 대신 `https://yourdomain.com`로 접속하세요.

{{% notice note %}}
[웹 콘솔용 HTTPS 수동 설정 방법](https://rustdesk.com/docs/en/self-host/rustdesk-server-pro/faq/#set-up-https-for-web-console-manually).
{{% /notice %}}

{{% notice note %}}
systemd 서비스가 시작되지 않는다면 SELinux과 관련이 있을 가능성이 있으니 [이것](https://rustdesk.com/docs/en/self-host/rustdesk-server-pro/faq/#selinux)을 확인해 주세요.
{{% /notice %}}

{{% notice note %}}
클라이언트가 서버에 연결되지 않거나 웹 콘솔에 접근할 수 없다면 [이것](https://rustdesk.com/docs/en/self-host/rustdesk-server-pro/faq/#firewall)을 확인해 주세요.
{{% /notice %}}

## 업그레이드

위 명령어를 복사하여 Linux 터미널에 붙여넣어 기존 RustDesk Server Pro 설치를 업그레이드하세요. 이 명령어는 로컬에 저장해 cron으로 예약할 수도 있습니다.

`wget -qO- https://raw.githubusercontent.com/rustdesk/rustdesk-server-pro/main/update.sh | bash`

{{% notice note %}}
이 스크립트에서 문제가 발생하면 스크립트를 하나씩 직접 실행해 보시기를 권장합니다.

또한 홈 디렉터리에서 실행하시고, 쓰기 권한이 없는 디렉터리에서는 실행하지 마세요.
{{% /notice %}}

이것이 하는 일:

- RustDesk Server Pro의 새로운 버전 확인
- 새로운 버전이 발견되면 API 파일을 제거하고 새 실행 파일과 API 파일을 다운로드

## 오픈소스에서 변환하기

위 명령어를 복사하여 Linux 터미널에 붙여넣어 RustDesk Server를 RustDesk Server Pro로 변환하세요.

`wget -qO- https://raw.githubusercontent.com/rustdesk/rustdesk-server-pro/main/convertfromos.sh | bash`

{{% notice note %}}
방화벽에 `21114` TCP 포트를 추가해 주세요. 이는 RustDesk 클라이언트의 웹 콘솔과 사용자 로그인을 위한 추가 포트입니다.
{{% /notice %}}

{{% notice note %}}
이 스크립트에서 문제가 발생하면 Docker 설치로 전환하는 것을 권장합니다. 또는 스크립트를 하나씩 직접 실행해 보세요.
{{% /notice %}}

이것이 하는 일:

- 이전 서비스 비활성화 및 제거
- 일부 종속성 설치
- 가능하다면 UFW 방화벽 설정
- `/var/lib/rustdesk-server` 폴더 생성 및 여기에 인증서 복사
- `/var/log/rustdesk` 삭제하고 `/var/log/rustdesk-server` 생성
- RustDesk Pro 서비스를 다운로드하고 위 폴더에 압축 해제
- hbbs 및 hbbr용 systemd 서비스 생성 (서비스 이름은 rustdesk-hbbs.service와 rustdesk-hbbr.service)
- 도메인을 선택하면 Nginx와 Certbot을 설치해 API를 포트 443(HTTPS)에서 이용 가능하게 하고, 포트 80을 통해 SSL 인증서를 받으며 자동으로 갱신됩니다.
