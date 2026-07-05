---
title: 리눅스
description: "Ubuntu, Fedora, CentOS, Arch, openSUSE, NixOS를 포함한 리눅스 배포판에 RustDesk를 설치하기. Wayland 참고사항, AppImage 및 Flatpak 옵션, SELinux 문제 해결 방법을 확인하세요."
keywords: ["rustdesk linux", "rustdesk ubuntu", "rustdesk fedora", "rustdesk arch", "rustdesk appimage", "rustdesk flatpak", "rustdesk wayland", "rustdesk selinux"]
weight: 4
---

이 리눅스 가이드를 사용하여 주요 배포판에 RustDesk를 설치하고, Wayland 및 로그인 화면 제한 사항을 확인하며, SELinux 정책 차단과 같은 일반적인 권한 문제를 해결하세요.

## 어떤 리눅스 패키지를 사용해야 하나요?

| 상황 | 최적의 패키지 |
| --- | --- |
| Ubuntu 또는 Debian 기반 시스템 | `.deb` |
| Fedora 또는 CentOS 기반 시스템 | `.rpm` |
| Arch Linux 또는 Manjaro | `.pkg.tar.zst` |
| openSUSE | SUSE 전용 `.rpm` |
| 휴대용 단일 파일 사용 | `AppImage` |
| 샌드박스 데스크톱 설치 | `Flatpak` |

## 리눅스 빠른 답변

- 가능하면 배포판의 기본 패키지를 사용하세요.
- RustDesk는 Wayland 지원이 실험적입니다.`1.2.0`
- 원격으로 로그인 화면에 접근하려면 여전히 X11이 필요합니다.
- SELinux가 강제되고 `avc: denied`가 표시되면 RustDesk SELinux 정책 지침을 적용하세요.

## 설치

### Ubuntu (≥ 18)

```sh
# please ignore the wrong disk usage report
sudo apt install -fy ./rustdesk-<version>.deb
```

Ubuntu 18.04의 경우, [pipewire](https://github.com/rustdesk/rustdesk/discussions/6148#discussioncomment-9295883)를 위해 먼저 아래와 같이 하세요.
```sh
sudo apt install software-properties-common
sudo add-apt-repository ppa:pipewire-debian/pipewire-upstream
sudo apt update
```

### CentOS/Fedora (≥ 28)

```sh
sudo yum localinstall ./rustdesk-<version>.rpm
```

### Arch Linux/Manjaro

```sh
sudo pacman -U ./rustdesk-<version>.pkg.tar.zst
```

### openSUSE (≥ Leap 15.0)

```sh
sudo zypper install --allow-unsigned-rpm ./rustdesk-<version>-suse.rpm
```

### AppImage

```sh
# For Fedora
sudo yum install libnsl
./rustdesk-<version>.AppImage
```

```sh
# For Ubuntu
sudo yum install libfuse2
./rustdesk-<version>.AppImage
```

### Flatpak

```sh
flatpak --user remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
flatpak --user install ./rustdesk-<version>.flatpak
flatpak run com.rustdesk.RustDesk
```

## ~~X11 필요~~
~~RustDesk는 아직 Wayland를 지원하지 않으므로 수동으로 X11로 전환해야 합니다.~~

RustDesk는 버전 1.2.0부터 실험적 Wayland 지원을 제공합니다.

### 디스플레이 서버

[Ubuntu](https://askubuntu.com/questions/1260142/ubuntu-set-default-login-desktop) | 
[Fedora](https://docs.fedoraproject.org/en-US/quick-docs/configuring-xorg-as-default-gnome-session/) | 
[Arch Linux](https://bbs.archlinux.org/viewtopic.php?id=218319)

### 로그인 화면

Wayland를 사용한 로그인 화면은 아직 지원되지 않습니다. 재부팅하거나 RustDesk로 로그아웃한 후 로그인 화면에 접근하려면 로그인 화면을 X11로 변경해야 하며, 다음 줄을 `WaylandEnable=false`로 수정하세요:`/etc/gdm/custom.conf` 또는 `/etc/gdm3/custom.conf`:

```ini
#WaylandEnable=false
```

{{% notice note %}}
위 변경사항이 적용되도록 **재부팅**해주세요.
{{% /notice %}}

