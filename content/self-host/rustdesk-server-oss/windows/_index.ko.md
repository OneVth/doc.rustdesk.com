---
title: Windows 및 PM2 또는 NSSM
weight: 20
description: "PM2 또는 NSSM을 사용하여 Windows에서 RustDesk Server OSS를 실행하십시오. 이 안내서에서는 Windows에서 hbbs 및 hbbr를 호스팅하기 위한 상충점, 서비스 설정 단계 및 로그 명령에 대해 설명합니다."
keywords: ["rustdesk server windows", "rustdesk pm2", "rustdesk nssm", "rustdesk hbbs windows", "rustdesk hbbr windows", "rustdesk self-host windows"]
---

이 안내서를 사용하여 PM2 또는 NSSM을 통해 Windows에서 RustDesk Server OSS를 실행하십시오. 간편한 사용자 수준 설정을 원하는지, 아니면 Windows 서비스 관리를 원하는지에 따라 선택하십시오.

## Windows에서 RustDesk Server OSS를 실행해야 하나요?

Windows에서도 RustDesk Server OSS를 실행할 수 있지만, 대부분의 프로덕션 배포에서는 여전히 Linux가 더 안전한 기본 옵션입니다. Windows는 이미 Windows Server를 운영하고 있고 서비스 모델을 잘 이해하는 팀에게는 작동할 수 있지만, 장기적인 자체 호스팅에는 더 취약한 경향이 있습니다.

## PM2 vs NSSM

| Method | Best for | Why you would use it |
| --- | --- | --- |
| NSSM | Dedicated Windows Server deployments | Runs as Windows services and starts without user login |
| PM2 | Mixed-use Windows machines or simpler operator workflows | Easier to manage if you already use Node.js and log into the host regularly |

## Windows 자체 호스팅 체크리스트

1. RustDesk Server Windows 바이너리를 다운로드하세요.
2. NSSM과 PM2 중 어느 것이 더 적합한지 결정하세요.
3. 선택한 프로세스 관리자를 설치하세요.
4. `hbbs`와 `hbbr` 모두 등록하고 시작하세요.
5. 필요한 방화벽 포트를 열고 두 프로세스가 계속 온라인 상태인지 확인하세요.

{{% notice note %}}
Windows 보안 정책은 까다롭습니다. 이 튜토리얼이 작동하지 않거나 불안정한 연결 문제가 발생한다면, Linux 서버로 마이그레이션해 주세요.
{{% /notice %}}

{{% notice note %}}
GUI 버전인 `RustDeskServer.setup.exe`는 더 이상 유지보수가 되지 않으므로 권장하지 않습니다.
{{% /notice %}}

## 갈림길
이제 두 가지 선택지가 있습니다: PM2(더 쉬움) 또는 NSSM(조금 더 어려움)을 사용해 RustDesk 서버를 시작할 수 있습니다.
NSSM을 사용하면 다음과 같은 몇 가지 이점이 있습니다:
- 이전 버전의 Windows(Windows Server 2008 R2/Windows 7 및 이전 버전)와의 하위 호환성.
- Windows Server에 이상적.
- 로그인 없이 부팅 시 자동 시작(시작 항목을 생성한 사용자가 로그인할 필요 없음).
- 두 바이너리를 모두 서비스로 실행.
- 독립형(노드.js에 대한 의존성 없음).

PM2의 장점은 다음과 같습니다:
- 서버를 본업용 컴퓨터와 동일한 컴퓨터에서 실행하는 경우 좋음.
- RustDesk 시작 항목을 생성한 사용자에게 정기적으로 로그인함.
- 사용자 친화적.

## NSSM을 이용한 설치

### NSSM 설치
[다운로드](https://github.com/dkxce/NSSM/releases/download/v2.25/NSSM_v2.25.zip)하여 NSSM을 추출하고, Windows 시스템에 맞는 아키텍처를 선택하세요(만약 x86이라면 win32 폴더의 내용을, x64라면 win64 폴더의 내용을 사용하세요). 또한 NSSM의 바이너리를 설치 드라이브의 `Program Files\NSSM` 디렉터리로 이동하는 것이 좋습니다(일단 서비스로 시작된 NSSM은 이동할 수 없으므로, `Program Files` 디렉터리에 숨겨두는 것이 가장 좋습니다). 일반적으로 C: 드라이브입니다. 또한 경로 변수에 경로(예: `C:\Program Files\NSSM`)를 추가하는 것도 권장합니다.

### NSSM이 제대로 설치되었는지 확인하기
모든 과정을 올바르게 수행했다면, `C:\Program Files\NSSM` 디렉터리(이 예에서는 C: 드라이브를 사용했지만, Windows를 설치한 드라이브나 원하는 경로를 사용해도 됩니다)에는 `nssm.exe` 파일만 있어야 합니다.

이 예에서는 `C:\Program Files\NSSM`를 사용하겠습니다.

명령 프롬프트를 열고 `nssm`를 실행하세요. 도움말 페이지가 표시되면 다음 단계로 넘어갈 준비가 된 것입니다.

### hbbr 및 hbbs 실행
[RustDesk Server](https://github.com/rustdesk/rustdesk-server/releases)의 Windows 버전을 다운로드하세요.
프로그램을 `C:\Program Files\RustDesk Server`(또는 원하는 위치에 압축 해제하세요. 서비스 설치 후 변경되지 않도록 주의하세요)에 압축 해제한 후 명령 프롬프트로 돌아가세요.

이 예에서는 `C:\Program Files\RustDesk Server`를 사용하겠습니다.
```cmd
nssm install "RustDesk hbbs service" "C:\Program Files\RustDesk Server\hbbs.exe"
nssm install "RustDesk hbbr service" "C:\Program Files\RustDesk Server\hbbr.exe"
```
**참고:**
- `RustDesk hbbs service`를 hbbs 서비스의 이름으로 변경할 수 있습니다.
- `RustDesk hbbr service`를 hbbr 서비스의 이름으로 변경할 수 있습니다.
- `C:\Program Files\RustDesk Server\hbbs.exe`를 RustDesk 바이너리를 둔 위치로 변경할 수 있습니다.
- `C:\Program Files\RustDesk Server\hbbr.exe`를 RustDesk 바이너리를 둔 위치로 변경할 수 있습니다.

**명령어 템플릿:**

복사해서 붙여넣고 수정하기만 하면 되는 명령어 템플릿입니다.

```cmd
nssm install <Desired hbbs servicename> <RustDesk hbbs binary path> <RustDesk hbbs arguments>
nssm install <Desired hbbr servicename> <RustDesk hbbr binary path> <RustDesk hbbr arguments>
```

**서비스 시작**

서비스가 성공적으로 설치된 후에는 시작해야 합니다.
```cmd
nssm start <Desired hbbs servicename>
nssm start <Desired hbbr servicename>
```

**완료!**

(위 방법은 Windows Server Core 2022 Standard에서 테스트되었습니다.)

## 또는

## PM2를 이용한 설치

### Node.js 설치

[다운로드](https://nodejs.org/dist/v16.14.2/node-v16.14.2-x86.msi)하여 Node.js를 설치하세요. PM2의 런타임 환경이므로 먼저 Node.js를 설치해야 합니다.

### PM2 설치

`cmd.exe`에 아래 명령어를 입력하고 각 줄마다 <kbd>Enter</kbd> 키를 누른 후 한 줄씩 실행하세요.

```cmd
npm install -g pm2
npm install pm2-windows-startup -g
pm2-startup install
```

### hbbr 및 hbbs 실행

[RustDesk Server](https://github.com/rustdesk/rustdesk-server/releases)의 Windows 버전을 다운로드하세요. 프로그램을 C: 드라이브에 압축 해제한 후 다음 네 가지 명령어를 실행하세요:

```cmd
cd C:\rustdesk-server-windows-x64
pm2 start hbbs.exe
pm2 start hbbr.exe
pm2 save
```

### 로그 보기

```cmd
pm2 log hbbr
pm2 log hbbs
```

## 대체 튜토리얼
https://pedja.supurovic.net/setting-up-self-hosted-rustdesk-server-on-windows/?lang=lat
