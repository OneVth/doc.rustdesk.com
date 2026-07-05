---
title: 윈도우
weight: 20
description: "Windows에서 MSVC, Rust, vcpkg, Sciter 및 LLVM을 사용해 RustDesk를 빌드하십시오. 이 안내서는 데스크톱 앱을 소스에서 컴파일하기 전에 필요한 툴체인 설정을 다룹니다."
keywords: ["build rustdesk windows", "rustdesk windows build", "rustdesk vcpkg windows", "rustdesk sciter dll", "rustdesk llvm libclang"]
---

이 안내서를 사용하여 MSVC 툴체인, Rust, `vcpkg`, Sciter 및 LLVM을 먼저 준비한 후 Windows에서 RustDesk를 빌드하십시오.

{{% notice note %}}
여기 있는 명령줄 명령어들은 명령 프롬프트가 아닌 Git Bash에서 실행해야 하며, 그렇지 않으면 구문 오류가 발생합니다.
{{% /notice %}}

## Windows에서 빌드하기 전에 필요한 사항은 무엇인가요?

Windows에서 RustDesk를 빌드하려면 Visual Studio C++ 툴체인, Rust, `vcpkg`, `sciter.dll` 및 `LIBCLANG_PATH`가 구성된 LLVM이 필요합니다. 예제와 환경 변수 문법이 작성된 대로 작동하도록 Git Bash에서 셸 명령어를 실행하십시오.

## Windows 빌드 체크리스트

- C++ 작업 영역이 포함된 Visual Studio를 설치하세요.
- `rustup-init.exe`를 통해 Rust를 설치하세요.
- `vcpkg`를 클론하고 부트스트랩한 후 `VCPKG_ROOT`를 설정하세요.
- 데스크톱 UI용 `sciter.dll`를 다운로드하세요.
- LLVM을 설치하고 `LIBCLANG_PATH`를 해당 `bin` 디렉터리로 설정하세요.
- RustDesk를 클론하고 Git Bash에서 기본 빌드 단계를 실행하세요.

## 의존성

### C++ 빌드 환경

[MSVC](https://visualstudio.microsoft.com/)를 다운로드하여 설치하세요.
개발자 머신 OS로 `Windows`를 선택하고 `C++`를 체크한 후, Visual Studio Community 버전을 다운로드하여 설치하세요. 설치 과정에 시간이 걸릴 수 있습니다.

### Rust 개발 환경

[rustup-init.exe](https://static.rust-lang.org/rustup/dist/x86_64-pc-windows-msvc/rustup-init.exe)를 다운로드하고 관리자 권한으로 실행하여 `rust`를 설치하세요.

### vcpkg

vcpkg를 클론하고 싶은 폴더로 이동한 후 [Git Bash](https://git-scm.com/download/win)를 사용해 다음 명령어를 실행하고 `vcpkg`를 다운로드하세요. 64비트 버전의 `libvpx`, `libyuv` 및 `opus`를 설치하세요. `Git`가 설치되지 않았다면 [여기](https://git-scm.com/download/win)에서 `Git`를 받으세요.

```sh
git clone https://github.com/microsoft/vcpkg
vcpkg/bootstrap-vcpkg.bat
export VCPKG_ROOT=$PWD/vcpkg
vcpkg/vcpkg install libvpx:x64-windows-static libyuv:x64-windows-static opus:x64-windows-static aom:x64-windows-static
```

시스템 환경 변수 `VCPKG_ROOT`=`<path>\vcpkg`를 추가하세요. `<path>`는 위에서 선택한 `vcpkg`를 클론할 위치여야 합니다.

![](/docs/en/dev/build/windows/images/env.png)

### Sciter

데스크톱 버전은 GUI를 위해 [Sciter](https://sciter.com/)를 사용하므로 [sciter.dll](https://raw.githubusercontent.com/c-smile/sciter-sdk/master/bin.win/x64/sciter.dll)을 다운로드하세요.

### LLVM

`rust-bindgen`는 `clang`에 의존하며, [LLVM](https://github.com/llvm/llvm-project/releases)을 다운로드하여 설치하고 시스템 환경 변수 `LIBCLANG_PATH`=`<llvm_install_dir>/bin`를 추가하세요.

LLVM 바이너리의 15.0.2 버전은 여기에서 다운로드할 수 있습니다: [64비트](https://github.com/llvm/llvm-project/releases/download/llvmorg-15.0.2/LLVM-15.0.2-win64.exe) / [32비트](https://github.com/llvm/llvm-project/releases/download/llvmorg-15.0.2/LLVM-15.0.2-win32.exe).

## 빌드

### 기본

```sh
git clone --recurse-submodules https://github.com/rustdesk/rustdesk
cd rustdesk
mkdir -p target/debug
wget https://raw.githubusercontent.com/c-smile/sciter-sdk/master/bin.win/x64/sciter.dll
mv sciter.dll target/debug
cargo run
```
