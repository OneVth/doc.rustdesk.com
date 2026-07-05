---
title: 릴레이 서버 구성
weight: 17
description: "서버 프로 배포에 추가 RustDesk 릴레이 서버를 추가하고 지리적 위치를 사용하여 가장 가까운 사용 가능한 hbbr 인스턴스를 통해 연결을 라우팅하십시오."
keywords: ["rustdesk relay server", "rustdesk hbbr", "rustdesk geolocation relay", "rustdesk additional relay", "rustdesk server pro relay"]
---

이 안내서를 사용하여 RustDesk Server Pro에 추가 릴레이 서버를 추가하고 지리적 위치 인식 라우팅을 통해 지역별 성능을 향상시키세요.

## 언제 추가 릴레이 서버가 필요한가요?

사용자가 서로 다른 지역에서 연결되고 직접 홀 펀칭이 항상 가능하지 않을 때 추가 릴레이 서버가 필요합니다. 추가 `hbbr` 노드는 RustDesk가 더 가까운 지역을 통해 릴레이 트래픽을 라우팅하도록 해주며, 릴레이가 필요할 때 지연 시간을 줄이고 세션 품질을 개선할 수 있습니다.

## 릴레이 설정 체크리스트

1. 대상 지역에 추가 `hbbr` 릴레이 서버를 배포하세요.
2. 해당 릴레이 서버에 `id_ed25519` 및 `id_ed25519.pub` 키 쌍을 복사하세요.
3. TCP 포트 `21117`와 `21119`를 열어주세요.
4. RustDesk Pro 웹 콘솔에 새로운 릴레이 호스트명 또는 IP 주소를 추가하세요.
5. `hbbs`에 MaxMind GeoLite2 City 데이터베이스를 설치하세요.
6. 지오 설정을 다시 로드하고 새 릴레이가 로그에 나타나는지 확인하세요.

## Docker를 이용한 추가 릴레이 서버 설치 방법

{{% notice note %}}
[간단한 설치](https://rustdesk.com/docs/en/self-host/rustdesk-server-pro/installscript/)는 동일한 머신에 암묵적으로 릴레이 서버(`hbbr` 프로세스)를 생성하므로, 릴레이 서버를 명시적으로 지정할 필요가 없습니다.

다른 머신에 추가 릴레이 서버를 명시적으로 생성하려면 [OSS 설치](https://rustdesk.com/docs/en/self-host/rustdesk-server-oss/install/)를 따라 `hbbr`를 실행하세요. `hbbr`, `rustdesk-server-linux-amd64.tar.gz`, `rustdesk-server-hbbr_<version>-<arch>.deb`, `rustdesk-server-windows-x86_64.tar.gz` 또는 `docker`(`sudo docker run ... rustdesk/rustdesk-server-pro hbbr`)에서 `hbbr`를 찾을 수 있습니다.

`hbbs`는 라이선스가 필요 없으며 오픈소스 버전과 동일합니다.
{{% /notice %}}

전 세계에 여러 릴레이 서버를 운영하고 자동으로 지오 위치를 활용해 가장 가까운 릴레이 서버를 사용하면 원격 컴퓨터에 연결할 때 더욱 빠른 경험을 얻을 수 있습니다. `id_ed25519`는 몇 초마다 이러한 릴레이 서버의 온라인 상태를 자동으로 확인하며, 온라인 상태인 릴레이 서버만 선택합니다.

{{% notice note %}}
알려진 문제: https://github.com/rustdesk/rustdesk/discussions/7934
{{% /notice %}}

> 개인 키 쌍 `id_ed25519.pub`와 ```
# docker volume create hbbr
```가 필요합니다.

1 - 이미 Docker가 설치되어 있다면 SSH를 통해 서버에 접속하고 hbbr용 볼륨을 생성하세요.

`/var/lib/docker/volumes/hbbr/_data`

볼륨 hbbr는 `scp <path/filename> username@server:</destination/path>`에 위치해야 합니다.

2 - 개인 키 쌍을 볼륨 위치로 복사하세요. 이 경우 SCP를 사용해 파일을 복사하겠습니다.

명령어 구문은 ```
# scp id_ed25519 root@100.100.100.100:/var/lib/docker/volumes/hbbr/_data
# scp id_ed25519.pub root@100.100.100.100:/var/lib/docker/volumes/hbbr/_data
```입니다.

```
# sudo docker run --name hbbr -v hbbr:/root -td --net=host rustdesk/rustdesk-server hbbr -k _
```

3 - 이전에 생성한 볼륨을 사용해 hbbr 컨테이너를 배포하세요. 이 볼륨에는 개인 릴레이 서버를 실행하는 데 필요한 개인 키 쌍이 포함되어 있습니다.

```
# docker logs hbbr

INFO [src/common.rs:121] **Private key comes from id_ed25519**
NFO [src/relay_server.rs:581] Key: XXXXXXXXXXXXXXXXXXXXX
INFO [src/relay_server.rs:60] #blacklist(blacklist.txt): 0
INFO [src/relay_server.rs:75] #blocklist(blocklist.txt): 0
INFO [src/relay_server.rs:81] Listening on tcp :21117
```

4 - 실행 중인 로그를 확인해 hbbr가 귀하의 키 쌍을 사용해 실행되고 있는지 확인하세요.

```
# sudo ufw allow proto tcp from any to any port 21117,21119
```

OS에 따라 방화벽을 사용해 IP를 차단하거나 허용하고 싶을 수 있습니다.

우리의 경우 Ubuntu를 사용하고 있으므로 21117과 21119 포트에 대한 모든 TCP 연결을 허용하고자 합니다.

```
# sudo ufw enable
```

**방화벽 활성화**
```
# ufw status

Status: active

To                         Action      From
--                         ------      ----
21117,21119/tcp            ALLOW       Anywhere
21117,21119/tcp (v6)       ALLOW       Anywhere (v6)
```

**상태 확인**
`Download Databases`

## 웹 콘솔에서 지오 위치를 구성하는 방법

### GeoLite2 City 데이터베이스 파일 등록 및 다운로드

지오 위치를 사용하려면 hbbs가 MaxMind GeoLite2 City 데이터베이스에 접근해야 합니다. 이 데이터베이스는 무료이며, 회원가입 후 파일을 다운로드하고 API 키를 받을 수 있습니다.

먼저 [웹사이트](https://www.maxmind.com/en/account/login)에 접속해 계정을 만들거나(계정이 없다면), `mmdb`로 이동해 GeoLite2 City를 다운로드하세요. gzip 파일을 선택하고 압축을 풀면 `mmdb` 파일이 생깁니다.

<img width="500" alt="image" src="https://github.com/rustdesk/doc.rustdesk.com/assets/642149/e14318fb-ec52-463c-af77-d08c9479c1b5">

Linux 머신에서 설치 스크립트를 사용해 RustDesk Pro를 설치했다면 `/var/lib/rustdesk-server/`로 `mmdb` 파일을 이동해야 합니다.

Docker 설치의 경우, 파일은 컨테이너를 배포할 때 매핑한 볼륨에 있어야 하며 `/root`에 매핑됩니다.

### 프로세스 자동화를 위한 API 키 획득 - Linux 서버

이 파일을 정기적으로 업데이트해야 하며, cronjob을 사용해 이를 수행할 수 있습니다. 다운로드 링크에 접근하려면 무료 API 키가 필요합니다.

`Manage License Keys`로 이동해 새로운 라이선스 키를 생성하세요. <br>
<img width="500" alt="image" src="https://github.com/rustdesk/doc.rustdesk.com/assets/642149/632aeb33-4f5d-4a31-9010-38e01c22d3c9">
<br>
<img width="500" alt="image" src="https://github.com/rustdesk/doc.rustdesk.com/assets/642149/3e178174-5fbf-46b7-a335-01f77125dfad">

다양한 방법으로 [다운로드 프로세스](https://dev.maxmind.com/geoip/updating-databases)를 자동화할 수 있지만, 다음 명령어를 crontab에 추가해 {Your Access Key}를 이전 단계에서 받은 API 키로 교체하세요.

```
/usr/bin/curl -L --silent 'https://download.maxmind.com/app/geoip_download?edition_id=GeoLite2-City&license_key={Your Access Key}&suffix=tar.gz' | /bin/tar -C '/var/lib/rustdesk-server/' -xvz --keep-newer-files --strip-components=1 --wildcards '*GeoLite2-City.mmdb'
```

### RustDesk Pro 웹 콘솔에서 설정 변경하기

`Relay Servers`에 릴레이 서버의 IP 주소 또는 DNS 이름을 추가하세요. (DNS는 1.1.11 버전부터 지원됩니다.) **포트는 필수 사항이 아니며, `21117` 포트가 명시적으로 사용됩니다.** <br>
<img width="500" alt="image" src="https://github.com/rustdesk/doc.rustdesk.com/assets/642149/c4452ba4-5e1d-437a-ae1d-fc0070bfa26c">

Geo 오버라이드를 추가하고 서버 IP 주소와 서버가 위치한 좌표를 입력하세요. <br>
<img width="500" alt="image" src="https://github.com/rustdesk/doc.rustdesk.com/assets/642149/41c558e3-423b-4296-90d3-cb0769f4a369">

`Reload Geo`를 클릭하면 목록이 이와 비슷하게 보일 것입니다. <br>
<img width="500" alt="image" src="https://github.com/rustdesk/doc.rustdesk.com/assets/642149/5a0d39a9-4fec-46b4-a7a2-7ed38b6baeb7">

결과를 확인하려면 `Reload Geo`를 클릭했을 때 hbbs 로그를 확인하세요. 릴레이 서버 IP 주소와 그 좌표를 표시하는 메시지를 볼 수 있어야 합니다.

> Linux 머신에서 RustDesk Pro를 실행하고 있다면 `RUST_LOG=debug ./hbbs` 명령어를 사용해 로그를 확인하세요. Docker 컨테이너에서 실행하고 있다면 `docker logs hbbs`를 사용하세요.

```
RUST_LOG=debug ./hbbs

INFO [src/common.rs:130] GEOIP_FILE: ./GeoLite2-City.mmdb
INFO [src/common.rs:159] override 1xx.xxx.xxx.x7: -1.xx 5x.xxx
[src/common.rs:159] override 1xx.xxx.xxx.xx8: -3.xxx 5x.xxxx
[src/common.rs:159] override 7xx.xxx.xxxx.xx1: 6.xxx 5x.xxxx
GEOIP_FILE loaded, #overrides 3
INFO [src/common.rs:119] relay-servers=["1xx.xxx.xxx.x7", "1xx.xxx.xxx.xx8", "7xx.xxx.xxx.xx1"]
NFO [src/rendezvous_server.rs:1467] parsed relay servers: [("1xx.xxxx.xxx.xx7", Some((-1x, xxx))), ("1xx.xxx.xxx.xx8", Some((-3x, xxx))), ("7xx.xxx.xxx.xx1", Some((6x, xxx)))]
```

hbbr 인스턴스에서도 릴레이 요청을 직접 확인할 수 있으며, 컨테이너 로그를 확인하면 됩니다.

```
# docker logs hbbr

INFO [src/relay_server.rs:436] Relayrequest 0593e64e-4fe8-4a59-a94f-b3420ab043eb from [::ffff:100.100.123.233]:52038 got paired
INFO [src/relay_server.rs:442] Both are raw
```
