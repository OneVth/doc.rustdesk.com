---
title: Docker
weight: 7
description: "Docker, Docker Compose 또는 Podman을 사용하여 RustDesk 서버 OSS를 셀프호스팅하십시오. 필요한 포트, 호스트 네트워킹 참고사항 및 hbbs 및 hbbr에 대한 샘플 컨테이너 정의를 확인하십시오."
keywords: ["rustdesk docker", "rustdesk docker compose", "rustdesk server docker", "rustdesk hbbs hbbr docker", "rustdesk podman", "rustdesk self-host docker"]
---

이 안내서를 사용하여 Docker, Docker Compose 또는 Podman을 통해 RustDesk Server OSS를 자체 호스팅하고 `hbbs` 및 `hbbr`에 대한 올바른 포트를 열어보세요.

## Docker에서 RustDesk Server OSS를 실행하는 가장 좋은 방법은 무엇인가요?

대부분의 Linux 배포에서는 `network_mode: "host"`를 사용한 Docker Compose가 가장 간단하고 신뢰할 수 있는 옵션입니다. 이 방법은 설정을 반복 가능하게 유지하고 업그레이드를 쉽게 만들어 주며, 호스트 네트워킹이 가능할 경우 추가적인 포트 매핑 복잡성을 피할 수 있습니다.

## Docker 배포 체크리스트

1. Docker 또는 Podman을 설치하세요.
2. `hbbs` 및 `hbbr`를 위한 지속적 데이터 디렉터리 또는 볼륨을 생성하세요.
3. 방화벽에서 필요한 RustDesk 포트를 열어주세요.
4. Docker Compose, `docker run` 또는 Podman Quadlet을 사용해 `hbbs` 및 `hbbr`를 시작하세요.
5. 클라이언트를 새 자체 호스팅 서버로 지정하고 등록 및 릴레이 트래픽을 확인하세요.

## 어떤 컨테이너 설정을 선택해야 하나요?

| Method | Best for | Why you would use it |
| --- | --- | --- |
| Docker Compose | Most Linux servers | Repeatable setup and easier ongoing maintenance |
| `docker run` | Quick manual testing | Fastest way to start a simple pair of containers |
| Podman Quadlet | Podman plus systemd environments | Native systemd-style service management |

> 여기에 또 다른 좋은 튜토리얼이 있습니다: [자신만의 원격 데스크톱 솔루션 구축하기: Docker(Hetzner)를 사용한 Cloud에서의 RustDesk 자체 호스팅](https://www.linkedin.com/pulse/building-your-own-remote-desktop-solution-rustdesk-cloud-montinaro-bv94f)

## Docker를 사용해 자체 서버를 설치하세요

### 요구 사항
Docker/Podman이 설치되어 있어야 Rustdesk-server를 Docker 컨테이너로 실행할 수 있습니다. 의심스러운 경우, 이 [안내서](https://docs.docker.com/engine/install)를 따라 Docker를 설치해 최신 버전인지 확인하세요!

방화벽에서 다음 포트를 반드시 열어주세요:
- `hbbs`:
  - `21114` (TCP): 웹 콘솔용으로 사용되며, `Pro` 버전에서만 이용 가능합니다.
  - `21115` (TCP): NAT 유형 테스트용으로 사용됩니다.
  - `21116` (TCP/UDP): **특히 `21116`는 TCP와 UDP 모두 활성화되어야 합니다.** `21116/UDP`는 ID 등록 및 하트비트 서비스에 사용됩니다. `21116/TCP`는 TCP 홀 펀칭 및 연결 서비스에 사용됩니다.
  - `21118` (TCP): 웹 클라이언트 지원에 사용됩니다.
- `hbbr`:
  - `21117` (TCP): 릴레이 서비스에 사용됩니다.
  - `21119` (TCP): 웹 클라이언트 지원에 사용됩니다.

*웹 클라이언트 지원이 필요하지 않다면 해당 포트 `21118`, `21119`는 비활성화해도 됩니다.*

### Docker 예제

```sh
sudo docker image pull rustdesk/rustdesk-server
sudo docker run --name hbbs -v ./data:/root -td --net=host --restart unless-stopped rustdesk/rustdesk-server hbbs
sudo docker run --name hbbr -v ./data:/root -td --net=host --restart unless-stopped rustdesk/rustdesk-server hbbr
```
<a name="net-host"></a>

{{% notice note %}}
`--net=host`는 오직 **Linux**에서만 작동하며, 이로 인해 `hbbs`/`hbbr`는 컨테이너 IP(172.17.0.1)가 아닌 실제 수신 IP 주소를 볼 수 있습니다.
`--net=host`가 잘 작동한다면, `-p` 옵션은 사용되지 않습니다. Windows에서는 `sudo` 및 `--net=host`를 제외하세요.

**플랫폼에서 연결 문제가 발생한다면, `--net=host`를 제거해주세요.**
{{% /notice %}}

{{% notice note %}}
`-td`로 로그를 볼 수 없다면, `docker logs hbbs`를 통해 로그를 확인할 수 있습니다. 또는 `-it`로 실행하면, `hbbs/hbbr`는 데몬 모드로 실행되지 않습니다.
{{% /notice %}}

### Docker Compose 예제
여기서 설명한 대로 `compose.yml`를 사용해 Docker 파일을 실행하려면 [Docker Compose](https://docs.docker.com/compose/)가 설치되어 있어야 합니다.

```yaml
services:
  hbbs:
    container_name: hbbs
    image: rustdesk/rustdesk-server:latest
    command: hbbs
    volumes:
      - ./data:/root
    network_mode: "host"

    depends_on:
      - hbbr
    restart: unless-stopped

  hbbr:
    container_name: hbbr
    image: rustdesk/rustdesk-server:latest
    command: hbbr
    volumes:
      - ./data:/root
    network_mode: "host"
    restart: unless-stopped
```

환경 변수를 사용해 config 변경이 필요하다면, 예를 들어 ALWAYS_USE_RELAY=Y를 설정할 수 있습니다. docker-compose.yml에 포함하세요.

```yaml
services:
  hbbs:
    container_name: hbbs
    image: rustdesk/rustdesk-server:latest
    environment:
      - ALWAYS_USE_RELAY=Y
    command: hbbs
    volumes:
      - ./data:/root
    network_mode: "host"

    depends_on:
      - hbbr
    restart: unless-stopped

  hbbr:
    container_name: hbbr
    image: rustdesk/rustdesk-server:latest
    command: hbbr
    volumes:
      - ./data:/root
    network_mode: "host"
    restart: unless-stopped
```

### Podman Quadlet 예제

Podman을 systemd 서비스로 컨테이너를 실행하고 싶다면, 다음 샘플 Podman Quadlet 구성 파일을 사용할 수 있습니다:

```ini
[Container]
AutoUpdate=registry
Image=rustdesk/rustdesk-server:latest
Exec=hbbs
Volume=/path/to/rustdesk-server/data:/root
Network=host

[Service]
Restart=always

[Install]
WantedBy=default.target
```

또는

```ini
[Container]
AutoUpdate=registry
Image=rustdesk/rustdesk-server:latest
Exec=hbbr
Volume=/path/to/rustdesk-server/data:/root
Network=host

[Service]
Restart=always

[Install]
WantedBy=default.target
```