---
title: Windows 휴대용 권한 상승
weight: 49
description: "Windows에서 RustDesk 휴대용 앱을 권한 상승하여 원격 세션에서 UAC 프롬프트, 작업 관리자 및 기타 관리자 수준의 창을 처리할 수 있도록 하세요. 시작 시 권한 상승, 제어된 종료 시 권한 상승 및 요청된 권한 상승을 비교해 보세요."
keywords: ["rustdesk portable elevation", "rustdesk uac", "rustdesk windows admin rights", "rustdesk task manager mouse issue", "rustdesk request elevation"]
---

RustDesk 포터블 클라이언트가 UAC 프롬프트, 권한 상승된 앱 및 전체 입력 제어를 처리하기 위해 관리자 권한이 필요할 때 Windows에서 포터블 권한 상승을 사용하십시오.

Windows 포터블 프로그램은 관리자 권한이 없으므로 다음과 같은 문제가 발생할 수 있습니다:

- UAC(사용자 계정 컨트롤) 창이 뜨면 화면 전송이 불가능합니다.
- 작업 관리자와 같은 권한 상승된 창이 뜨면 마우스가 반응하지 않습니다.

권한 상승을 통해 RustDesk는 시작 시 또는 세션 중에 관리자 권한을 가진 프로세스를 생성하여 스크린샷 캡처 및 마우스 동작을 수행할 수 있으므로 위의 문제를 피할 수 있습니다.

## 언제 포터블 권한 상승이 필요한가요?

RustDesk 포터블 클라이언트가 Windows에서 관리자 수준 UI와 상호작용해야 할 때 포터블 권한 상승이 필요합니다. 가장 흔한 증상은 차단된 UAC 프롬프트, 권한 상승 대화 중 화면 전송이 안 되는 경우, 또는 작업 관리자와 같은 권한 상승된 창에서 마우스 제어가 안 되는 경우입니다.

## 포터블 권한 상승에 대한 간단한 답변

- 포터블 빌드가 관리자 수준 Windows UI와 상호작용해야 할 때만 권한 상승이 필요합니다.
- 아무도 제어 측에 있지 않을 때는 시작 시 권한 상승이 가장 좋습니다.
- 사용자가 `Accept and Elevate`를 클릭할 수 있을 때는 제어 측에서 요청한 권한 상승이 가장 좋습니다.
- 제어 측에서 요청한 권한 상승은 여전히 제어 측에서 UAC 프롬프트를 수락해야 합니다.
- 특별히 포터블 워크플로우가 필요하지 않을 때는 정식 프로그램을 설치하는 것이 더 나은 선택입니다.

## 시작 시 권한 상승

이 방법을 사용하면 원격 사용자가 연결할 때 권한 상승을 요청할 필요가 없습니다. 두 가지 방법이 있습니다:

* 방법 1: 포터블 프로그램의 이름을 변경해 `-qs-` 또는 `-qs.exe` 또는 `_qs.exe`(1.4.6 이상)을 포함하거나 `qs.exe`(1.2.0 ~ 1.4.5)으로 끝나게 하세요. 왼쪽 마우스 버튼을 클릭해 실행하고, UAC 창에서 `Accept`를 클릭하세요.

* 방법 2: 마우스 오른쪽 버튼을 클릭한 후 관리자 권한으로 실행하세요.

## 제어 측에서 권한 상승

제어 측에서는 연결 시 바로 `Accept and Elevate`를 클릭하거나 이미 연결된 상태에서 `Elevate`를 클릭할 수 있습니다.

| 연결 중 | 연결됨 |
| :---: | :---: |
| ![](/docs/en/client/windows/windows-portable-elevation/images/cm_unauth.jpg) | ![](/docs/en/client/windows/windows-portable-elevation/images/cm_auth.jpg) |

## 제어 측에서 권한 상승 요청

동작 메뉴에서 `Request Elevation`를 선택한 후 다음 대화창이 나타납니다. `Ask the remote user for authentication`를 선택하면 사용자 이름과 비밀번호를 입력할 필요가 없지만, 원격 컴퓨터의 사용자는 관리자 권한이 있어야 합니다. `Transmit the username and password of administrator`를 선택하면 원격 컴퓨터의 사용자는 UAC 창에서 수락하기만 하면 됩니다. 요청을 보낸 후, 상대방 사용자가 UAC 창을 수락할 때까지 기다려 주세요. 확인되면 성공 메시지가 표시됩니다. **두 방법 모두 제어 측에서 UAC 창을 수락해야 합니다**. 따라서 상대방 측에 사람이 없을 경우 제어 측에서 권한 상승을 요청해서는 안 됩니다.

| 메뉴 | 대화창 |
| :---: | :---: |
| ![](/docs/en/client/windows/windows-portable-elevation/images/menu.png) | ![](/docs/en/client/windows/windows-portable-elevation/images/dialog.png) |
| **대기** | **성공** |
| ![](/docs/en/client/windows/windows-portable-elevation/images/wait.png) | ![](/docs/en/client/windows/windows-portable-elevation/images/success.png) |

## 선택 방법

| 시나리오 | 방법 |
| :---: | :---: |
| 권한 상승이 필요하지 않음 | 프로그램 설치 |
| 제어 측에 사용자가 없음 | 이름 바꾸기<br/>*또는*<br/> 관리자 권한으로 실행 |
| 제어 측에 사용자가 있음<br/>*그리고*<br/> 연결 시 즉시 권한 상승<br/>*그리고*<br/> 클릭으로 수락 연결 | 제어 측에서 연결을 받을 때 `Accept and Elevate`를 클릭하세요 |
| 제어 측에 사용자가 있음<br/>*그리고*<br/> 필요에 따라 권한 상승 | 제어 측의 연결 관리 창에서 `Elevate`를 클릭하세요<br/>*또는*<br/> 제어 측에서 권한 상승 요청 |
