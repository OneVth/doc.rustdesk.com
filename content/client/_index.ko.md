---
title: RustDesk 클라이언트
description: "Windows, macOS, Linux, Android, iOS 및 웹에서 RustDesk 클라이언트를 설치하고 구성하세요. 기기 연결 방법, 공개 또는 자체 호스팅 서버 사용 방법, 클라이언트 설정 관리 방법을 알아보세요."
keywords: ["rustdesk client", "rustdesk download", "rustdesk installation", "rustdesk windows", "rustdesk mac", "rustdesk linux", "rustdesk android", "rustdesk ios", "rustdesk web client", "rustdesk client configuration"]
weight: 2
pre: "<b>1. </b>"
---

## RustDesk 클라이언트란 무엇인가요?
RustDesk 클라이언트는 장치에 설치하여 RustDesk 공공 서버 또는 사용자 자체 호스팅 RustDesk 서버를 통해 원격 데스크톱 세션을 시작하거나 수신하는 애플리케이션입니다. 이 안내서를 참고해 올바른 빌드를 다운로드하고 각 플랫폼에 설치한 후, 다른 기기와 연결하고 클라이언트를 RustDesk Server OSS 또는 RustDesk Server Pro로 지정하세요.

## 지원되는 플랫폼
- Microsoft Windows
- macOS
- Debian 파생판 (Ubuntu ≥ 16, Linux Mint 등)
- Red Hat 파생판 (CentOS, Fedora ≥ 18, Rocky Linux 등)
- Arch Linux/Manjaro
- openSUSE
- NixOS
- AppImage / Flatpak
- Android
- iOS (원격 제어 불가)
- 웹

## 설치

### Windows

GitHub에서 exe 파일을 다운로드하여 설치하세요.

암묵적으로 설치하려면 `--silent-install`를 사용해 설치용 exe를 호출하세요.

### macOS

GitHub에서 dmg 파일을 다운로드하세요. 더 자세한 정보는 [macOS 페이지](https://rustdesk.com/docs/en/client/mac/)에서 확인할 수 있습니다.

dmg 파일을 열고 `RustDesk`를 `Applications`로 드래그하세요.

RustDesk 실행을 허용하세요.

요청된 권한을 활성화하고 RustDesk 왼쪽의 안내에 따라 설정을 완료하세요.

### Linux

다음과 같은 리눅스의 다양한 "플레이버"에 대한 설치 방법을 참고하세요. (설치 프로그램은 GitHub에 있거나 배포판의 저장소에서 이용 가능합니다.)

#### Debian 파생판

```sh
# please ignore the wrong disk usage report
sudo apt install -fy ./rustdesk-<version>.deb
```

#### Red Hat 파생판

```sh
sudo yum localinstall ./rustdesk-<version>.rpm
```

#### Arch Linux/Manjaro

```sh
sudo pacman -U ./rustdesk-<version>.pkg.tar.zst
```

#### openSUSE (≥ Leap 15.0)

```sh
sudo zypper install --allow-unsigned-rpm ./rustdesk-<version>-suse.rpm
```

#### Nix / NixOS (≥ 22.05)

임시로 `rustdesk`를 사용해 셸을 진입하고 실행 준비:

```sh
nix shell nixpkgs#rustdesk
```

현재 사용자 프로필에 설치하세요:

```sh
nix profile install nixpkgs#rustdesk
```

NixOS에서 시스템 전체 설치하려면 `nixos-rebuild switch --flake /etc/nixos`를 실행한 후 `configuration.nix`를 수정하세요:

```
  environment.systemPackages = with pkgs; [
    ...
    rustdesk
  ];
```

### Android
GitHub에서 apk를 설치하세요. 더 자세한 정보는 [Android 페이지](https://rustdesk.com/docs/en/client/android/)에서 확인할 수 있습니다.

### iOS (iPhone, iPad)
[App Store](https://apps.apple.com/us/app/rustdesk-remote-desktop/id1581225015)에서 앱을 다운로드하세요.

## 사용법
설치(또는 임시 실행 파일로 실행) 후 RustDesk는 공공 서버에 연결됩니다. 하단에 "(1) 준비 완료, 더 빠른 연결을 위해 자신의 서버를 설정하세요."라는 메시지가 표시됩니다. 왼쪽 상단에는 (2) ID, (3) 일회용 비밀번호가 표시되고, 오른쪽에는 ID를 알고 있다면 다른 컴퓨터에 연결할 수 있는 (4) 입력창이 있습니다.

![](/docs/en/client/images/client.png)

설정에 접근하려면 ID 오른쪽에 있는 (5) 메뉴 버튼 [ &#8942; ]을 클릭하세요.

설정에서 다음을 찾을 수 있습니다:
- 일반 - 서비스 제어, 테마, 하드웨어 코덱, 오디오, 녹화 및 언어
- 보안 - 제어권을 가진 사람의 권한, 비밀번호 옵션, ID 변경 가능성 및 고급 보안 설정
- 네트워크 - 여기서 자신의 서버 설정 및 프록시를 설정하세요.
- 디스플레이 - 원격 세션의 디스플레이 설정 및 기타 기본 옵션, 클립보드 동기화 등을 제어하세요.
- 계정 - Pro 서버와 함께 API에 로그인하는 데 사용할 수 있습니다.
- 정보 - 소프트웨어에 대한 정보를 표시합니다.

## RustDesk 구성하기
RustDesk를 구성하는 방법은 여러 가지가 있습니다.

가장 쉬운 방법은 RustDesk Server Pro를 사용하는 것입니다. 암호화된 설정 문자열을 얻어 이를 `--config`와 결합해 설정을 가져올 수 있습니다. 이렇게 하려면:
1. 사용 중인 OS의 명령줄을 열고 RustDesk가 설치된 폴더로 이동하세요. 예: Windows에서는 `C:\Program Files\RustDesk`, Linux에서는 `/usr/bin`.
2. `rustdesk.exe --config your-encrypted-string` 명령어를 사용하세요. 예: `rustdesk.exe --config 9JSPSvJzNrBDasJjNSdXOVVBlERDlleoNWZzIHcOJiOikXZr8mcw5yazVGZ0NXdy5CdyciojI0N3boJye`.

클라이언트를 수동으로 설정할 수도 있습니다. 이렇게 하려면:
1. 설정을 클릭하세요.
2. 네트워크를 클릭하세요.
3. 네트워크 설정 잠금 해제를 클릭하세요.
4. ID, 릴레이, API(Pro 서버 사용 시) 및 키를 입력하세요.

![](/docs/en/client/images/network-settings.png)

수동으로 클라이언트를 설정한 경우, `RustDesk2.toml`(사용자 폴더 내) 파일을 가져와 위의 예와 유사하게 `--import-config`를 사용할 수 있습니다.

## 명령줄 매개변수
- `--password`는 영구 비밀번호를 설정하는 데 사용할 수 있습니다.
- `--get-id`는 ID를 가져오는 데 사용할 수 있습니다.
- `--set-id`는 ID를 설정하는 데 사용할 수 있으며, ID는 반드시 알파벳으로 시작해야 합니다.
- `--silent-install`는 Windows에서 RustDesk를 암묵적으로 설치하는 데 사용할 수 있습니다.

추가 고급 매개변수는 [여기](https://github.com/rustdesk/rustdesk/blob/bdc5cded221af9697eb29aa30babce75e987fcc9/src/core_main.rs#L242)에서 확인할 수 있습니다.

{{% children depth="3" showhidden="true" %}}
