---
title: 설치
weight: 1
description: "Docker, systemd 스크립트 또는 Debian 패키지를 사용해 RustDesk 서버 OSS를 설치하기. 서버 요구 사항, 방화벽 규칙 및 배포 후 필요한 클라이언트 구성 단계를 검토하세요."
keywords: ["rustdesk server install", "install rustdesk server oss", "rustdesk docker install", "rustdesk server firewall ports", "rustdesk hbbs hbbr install", "rustdesk self-host install"]
---

이 안내서를 사용하여 RustDesk Server OSS를 설치하고, 필요한 방화벽 포트를 열고, 클라이언트를 새 자체 호스팅 서버에 연결하십시오.

## RustDesk Server OSS의 권장 설치 방법은 무엇인가요?

대부분의 배포에서는 Docker가 가장 쉬운 재현성과 업그레이드, 서버 간 이동을 가능하게 하기 때문에 권장되는 설치 방법입니다. Linux에서 기본 서비스를 선호하는 경우 systemd 설치 스크립트나 Debian 패키지도 잘 작동할 수 있습니다.

## 어떤 설치 방법을 선택해야 하나요?

| Method | Best for | Why you would use it |
| --- | --- | --- |
| Docker | Most self-hosted deployments | Simplest upgrades, predictable setup, and easy rollback |
| install script | Linux admins who want systemd services quickly | Sets up `hbbs`, `hbbr`, and a client config flow with less manual work |
| Debian package | Debian-based systems using package tooling | Keeps installation closer to native package management |

## 동영상 튜토리얼
YouTube와 https://github.com/rustdesk/rustdesk/wiki/FAQ#video-tutorials에 많은 동영상 튜토리얼이 있습니다.

## 서버 요구사항
하드웨어 요구사항은 매우 낮습니다; 기본 클라우드 서버의 최소 구성만으로도 충분하며, CPU와 메모리 요구량도 최소입니다. Raspberry Pi와 같은 장치를 사용할 수도 있습니다. 네트워크 크기에 관해 말씀드리자면, TCP 홀 펀칭을 통한 직접 연결이 실패하면 릴레이 트래픽이 소모됩니다. 릴레이 연결의 트래픽은 해상도 설정과 화면 업데이트에 따라 초당 30K~3M(1920x1080 화면) 사이입니다. 사무용으로만 사용한다면 트래픽은 약 100K/s 정도입니다.

## 방화벽
UFW가 설치되어 있다면 다음 명령어를 사용해 방화벽을 구성하십시오:
```
ufw allow 21114:21119/tcp
ufw allow 21116/udp
sudo ufw enable
```

## 설치
### 방법 1: Docker (권장)

```
bash <(wget -qO- https://get.docker.com)
wget rustdesk.com/oss.yml -O compose.yml
sudo docker compose up -d
```

자세한 내용은 [Docker](/docs/en/self-host/rustdesk-server-oss/docker/)를 확인하십시오.

### 방법 2: 간단히 실행 가능한 설치 스크립트를 사용해 systemd 서비스로 자체 서버 설치하기
스크립트는 [Techahold](https://github.com/techahold/rustdeskinstall)에 호스팅되어 있으며, 저희 [Discord](https://discord.com/invite/nDceKgxnkV)에서도 지원됩니다.

현재 스크립트는 릴레이 및 신호 서버(hbbr 및 hbbs)를 다운로드하고 설정하며, 구성 파일을 생성해 비밀번호 보호된 웹페이지에 호스팅함으로써 클라이언트에게 간편하게 배포할 수 있도록 합니다.

다음 명령어를 실행하십시오:
```
wget https://raw.githubusercontent.com/techahold/rustdeskinstall/master/install.sh
chmod +x install.sh
./install.sh
```

[Techahold](https://github.com/techahold/rustdeskinstall) 리포지토리에는 업데이트 스크립트도 있습니다.

그곳에서 설치 마지막에 표시된 IP/DNS와 키를 기록한 후, 이를 클라이언트 설정 > 네트워크 > ID/릴레이 서버 `ID server` 및 `Key` 필드에 각각 입력하십시오. 다른 필드는 비워두십시오(아래 참고 참조).

### 방법 3: Debian 배포판용 deb 파일을 사용해 systemd 서비스로 자체 서버 설치하기

직접 [다운로드](https://github.com/rustdesk/rustdesk-server/releases/latest)하여 `apt-get -f install <filename>.deb` 또는 `dpkg -i <filename>.deb`로 설치하십시오.

## 서버 설치 후 클라이언트는 무엇이 필요하나요?

서버가 실행된 후, 클라이언트는 보통 `ID Server` 주소와 서버 공개 `Key`가 필요합니다. RustDesk Server Pro 클라이언트를 구성하는 경우, `API Server`도 필요할 수 있습니다. [여기](/docs/en/self-host/client-configuration/#2-manual-config)를 확인하십시오.
