---
title: 설치
weight: 2
description: "Docker, Linux 설치 스크립트 또는 이전 Windows 방법을 사용해 RustDesk 서버 Pro를 설치하기. 환경에 맞는 올바른 배포 경로를 선택하려면 여기에서 시작하세요."
keywords: ["rustdesk server pro install", "rustdesk self-host pro", "rustdesk pro docker", "rustdesk pro linux install", "rustdesk pro windows install"]
---

환경에 맞는 최적의 RustDesk Server Pro 설치 방법을 선택하려면 여기에서 시작하세요. 권장 옵션은 Docker입니다.

## RustDesk Server Pro를 설치하는 가장 좋은 방법은 무엇인가요?

대부분의 팀에서는 Docker가 RustDesk Server Pro를 설치하는 데 가장 적합한 방법입니다. 업데이트가 간편하고 배포 과정이 재현하기 쉬워서입니다. `install.sh` 경로는 Linux에서 기본 systemd 서비스를 원할 때 유용합니다. 이미 RustDesk 서버를 운영 중이고 Pro 버전으로 이동하려는 경우, OSS에서 변환하는 것이 적합한 경로입니다.

## 시작하기 전에 필요한 것은 무엇인가요?

- RustDesk Server Pro 라이선스
- Linux 서버 또는 VM, 또는 이미 Docker가 설치된 호스트
- 방화벽에서 필요한 RustDesk 포트가 열려 있어야 하며, 웹 콘솔과 API가 필요하다면 `21114` 또는 `443`도 열어두어야 합니다.
- 도메인에서 HTTPS를 사용하려는 경우 선택적 DNS 이름

## 어떤 설치 방법을 선택해야 하나요?

| Method | Best for | Why you would use it |
| --- | --- | --- |
| Docker | Most new Pro deployments | Easiest upgrades, simpler rollback, and consistent setup |
| `install.sh` | Linux admins who want native services | Creates systemd services and can optionally set up Nginx and Certbot |
| Convert from open source | Existing OSS deployments | Moves an existing RustDesk Server install to Pro without starting from zero |

## 방법 1: Docker (권장)

```
bash <(wget -qO- https://get.docker.com)
wget rustdesk.com/pro.yml -O compose.yml
sudo docker compose up -d
```

자세한 내용은 [Docker](/docs/en/self-host/rustdesk-server-pro/installscript/docker/)를 참조하세요.

## 방법 2: install.sh

Linux에 능숙하신 분들은 아래 스크립트를 사용하세요. 그렇지 않으면 실패 시 심각한 문제가 발생할 수 있으며, 왜 작동하지 않는지 파악하기 어려울 수 있습니다.

`bash <(wget -qO- https://raw.githubusercontent.com/rustdesk/rustdesk-server-pro/main/install.sh)`

자세한 내용은 [install.sh](/docs/en/self-host/rustdesk-server-pro/installscript/script/)를 참조하세요.

## 오픈소스에서 변환하기

### Docker
Docker를 사용해 오픈소스 버전을 설치한 경우, 직접 변환할 수 있는 방법은 없습니다. 대신 Pro 이미지를 사용해 새 컨테이너를 실행해야 합니다. 이를 수행하기 전에 개인 키(파일 `id_ed25519`, `id_ed25519.pub` 아님)를 백업해 두세요. 새 컨테이너가 설정되면 기존 `id_ed25519` 개인 키 파일을 새 컨테이너의 작업 디렉터리로 복사한 다음, 컨테이너를 다시 시작하세요.

### install.sh
install.sh를 사용해 오픈소스 버전을 설치한 경우, [여기](/docs/en/self-host/rustdesk-server-pro/installscript/script/#convert-from-open-source)를 따라 진행하세요.
