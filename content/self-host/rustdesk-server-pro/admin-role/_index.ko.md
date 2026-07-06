---
title: 관리자 역할
weight: 17
description: "RustDesk 서버 프로에서 관리자 역할을 사용하여 사용자, 장치, 전략(Strategy), 제어 역할 및 기타 콘솔 리소스에 대한 범위 지정된 관리 권한을 위임하십시오."
keywords: ["rustdesk admin role", "rustdesk delegated admin", "rustdesk server pro permissions", "rustdesk role management", "rustdesk web console roles"]
---

RustDesk 서버 프로에서 전체 관리자 권한을 부여하지 않고 부분적인 관리 접근 권한을 위임해야 할 때 관리자 역할을 사용하십시오.

관리자 역할을 통해 관리자는 비관리자 사용자에게 부분적인 관리 권한을 위임할 수 있습니다. 다양한 범위 내의 사용자 및 디바이스뿐만 아니라 전역 리소스(예: 정책, 제어 역할 및 사용자 지정 클라이언트)에 대한 권한을 정의할 수 있습니다.

관리자 역할이 사용자에게 할당되면, 사용자는 부여된 권한에 따라 웹 콘솔에서 해당 페이지와 메뉴를 볼 수 있습니다.

## 언제 관리자 역할을 사용해야 하나요?

누군가가 전체 관리자가 되지 않고도 RustDesk 환경의 일부를 관리해야 할 때 관리자 역할을 사용하십시오. 이 모델은 헬프데스크 책임자, 지역 IT팀, 디바이스 소유자 또는 그룹 범위의 운영자에게 적합하며, 이들은 플릿의 일부만 관리해야 합니다.

## 관리자 역할에 대한 간단한 답변

- 관리자 역할은 콘솔 관리 권한을 위임합니다.
- 이는 전체 관리자 계정을 대체하지 않습니다.
- 한 사용자가 동시에 여러 개의 관리자 역할을 가질 수 있습니다.
- 유효한 권한 집합은 할당된 모든 관리자 역할의 합집합입니다.
- 관리가 선택된 그룹으로 제한되어야 할 경우 그룹 범위 역할이 적합합니다.

## 관리자 vs 관리자 역할

- 관리자만이 관리자 역할을 편집하고 사용자에게 관리자 역할을 할당할 수 있습니다.
- 관리자는 관리자 역할에 의해 제한되지 않으며, 관리자에게 관리자 역할을 할당할 수 있습니다.
- 비관리자 사용자는 전역 사용자 권한이 부여되어 있더라도 관리자 계정을 편집할 수 없습니다.

## 역할 유형

관리자 역할은 세 가지 유형으로 나뉘며, 각각 다른 범위와 사용 가능한 권한을 갖습니다.

| 유형 | 설명 |
|------|-------------|
| **전체** | 전체 팀의 모든 리소스를 관리할 수 있음 |
| **개별** | 사용자의 자체 기기와 감사 로그만 관리할 수 있음 |
| **그룹 범위** | 지정된 그룹 내의 사용자와 기기를 관리할 수 있음 |

### 그룹 범위 정보

| 선택됨 권한 | 적용 대상 |
|-------|-------------|
| **사용자 권한** | 선택된 사용자 그룹 내 사용자에 적용 |
| **장치 권한** | 다음에서 장치에 적용: <ul><li>선택된 장치 그룹</li><li>선택된 사용자 그룹 내 사용자에게 할당된 장치</li><li>할당되지 않은 장치(활성화된 경우)</li></ul> |

그룹 범위 역할에서 사용자 권한 또는 장치 권한만 선택하여 권한과 범위를 보다 명확히 할 수 있습니다. 예를 들어, 사용자 권한만 선택하면 어떤 장치 접근 권한 없이 사용자를 관리할 수 있으며, 장치 권한만 선택하면 사용자 그룹, 장치 그룹 또는 미할당 장치를 범위로 선택하여 장치를 관리할 수 있습니다.

## 권한 규칙

### 모든 편집 권한에는 해당하는 보기 권한이 포함됩니다

모든 편집 권한은 자동으로 해당하는 보기 권한을 포함합니다. 예를 들어, "장치 활성화/비활성화" 권한에는 "장치 보기" 권한이 포함됩니다.

### 편집 권한에는 할당이 포함되지 않습니다

리소스(사용자 그룹, 장치 그룹, 전략, 제어 역할)에 대한 편집 권한은 리소스 자체만 편집할 수 있도록 하며, 이를 사용자나 장치에 할당하는 것은 허용되지 않습니다.

예를 들어, "장치 그룹 편집" 권한은 장치 그룹을 생성하고 수정할 수 있지만, 그룹에 장치를 추가하거나 제거하려면 "장치 그룹 업데이트" 권한이 필요합니다.

### 보기 권한에는 구성원이 포함되지 않습니다

리소스(사용자 그룹, 장치 그룹, 전략, 제어 역할)에 대한 보기 권한은 리소스 자체만 볼 수 있도록 하며, 그 안의 구성원을 볼 수는 없습니다.

예를 들어, "디바이스 그룹 보기" 권한은 디바이스 그룹 목록을 볼 수 있도록 허용하지만, 그룹 내의 디바이스를 보려면 "디바이스 보기" 권한 또는 어떤 디바이스 편집 권한이 필요합니다. 디바이스 권한이 전역인 경우 그룹 내 모든 디바이스를 볼 수 있으며, 그룹 범위 또는 개별 권한인 경우 허용된 범위 내의 디바이스만 볼 수 있습니다.

{{% notice note %}}
주소록의 디바이스 읽기는 관리자 역할에 의해 제한되지 않습니다. 클라이언트의 액세스 가능한 장치 피어 탭은 콘솔의 **설정 → 기타 → 액세스 가능한 장치 가져오기 비활성화**에 의해서만 제어되며, 관리자 역할에 의해 제한되지 않습니다.
{{% /notice %}}

## 콘솔 작업

### 역할 생성

1. **관리자 역할** 페이지로 이동하여 **생성**을 클릭하세요.
2. 역할에 대한 **이름**을 입력하세요.
3. **유형**을 선택하세요(그룹 범위인 경우 범위도 구성하세요).
4. 부여할 **권한**을 선택하세요.

![](/docs/en/self-host/rustdesk-server-pro/admin-role/images/admin-role-create-name.png)
![](/docs/en/self-host/rustdesk-server-pro/admin-role/images/admin-role-create-permission.png)

### 역할 할당

사용자에게 관리자 역할을 할당하는 방법은 두 가지가 있습니다:

1. **사용자 페이지** → 사용자에서 **편집**을 클릭한 후 **관리자 역할** 필드에서 역할을 선택하세요.
2. **관리자 역할 페이지** → **사용자 수** 또는 **사용자 할당**을 클릭한 후 역할에 사용자를 추가하거나 제거하세요.

![](/docs/en/self-host/rustdesk-server-pro/admin-role/images/admin-role-assign-user-page.png)
![](/docs/en/self-host/rustdesk-server-pro/admin-role/images/admin-role-assign-role-page.png)

{{% notice note %}}
- 사용자는 여러 개의 관리자 역할을 할당받을 수 있습니다. 할당된 모든 역할의 권한이 결합됩니다(모든 권한의 합집합).
{{% /notice %}}

## 권한 참조

### 글로벌 권한

| Permission | Description |
|------------|-------------|
| Users-View | Read list information of all users. |
| Users-Create | Directly create non-administrator users. |
| Users-Invite | Invite users via email. |
| Users-Delete | Delete any non-administrator user. Users must be disabled before they can be deleted. |
| Users-Enable/Disable | Enable or disable any non-administrator user. |
| Users-Edit Email | Change the email of any non-administrator user. |
| Users-Edit Password | Change the password of any non-administrator user. |
| Users-Edit Note | Change the note of any non-administrator user. |
| Users-Manage 2FA | Manage login verification for any non-administrator user. Includes enable/disable 2FA enforcement, reset 2FA configuration, disable email login verification. |
| Users-Force Logout | Force logout any non-administrator user from all devices. |
| Users-Update Group | Change any non-admin user's group. |
| Users-Update Strategy | Change any non-admin user's strategy. |
| Users-Update Control Role | Change any non-admin user's control role. |
| Devices-View | Read list information of all devices. |
| Devices-Enable/Disable | Enable or disable any device. |
| Devices-Delete | Delete any device. Devices must be disabled before they can be deleted. |
| Devices-Edit Info | Edit device name, device username (system username of the device, not the RustDesk user), and note for any device. |
| Devices-Assign to User | Assign any device to any user. |
| Devices-Update Group | Change any device's group. |
| Devices-Update Strategy | Change any device's strategy. |
| User Groups-View | Read list information of all user groups. If having Users View permission, can view group members. If having Users Update Group permission, can batch update users' groups here. |
| User Groups-Edit | Create, edit, and delete user groups, does not include updating group members. |
| Device Groups-View | Read list information of all device groups. If having Devices View permission, can view group members. If having Devices Update Group permission, can batch update devices' groups here. |
| Device Groups-Edit | Create, edit, and delete device groups, does not include updating group members. Includes Update Strategy permission. |
| Device Groups-Update Strategy | Change any device group's strategy. |
| Audit Logs-View | Read all logs. Can edit notes. Even when "Only admin can access logs" option is enabled. |
| Audit Logs-Edit | Can disconnect any active connection. |
| Strategies-View | Read any strategy. If having Users View, Devices View, and Device Groups View permissions, can read strategies for users, devices, and device groups. If having Users Update Strategy, Devices Update Strategy, and Device Groups Update Strategy permissions, can batch update corresponding strategies here. |
| Strategies-Edit | Create, edit, and delete strategies, does not include updating strategies for users, devices, and device groups. |
| Control Roles-View | Read any control role. If having Users View permission, can read control roles for users. If having Users Update Control Role permission, can batch update corresponding control roles here. |
| Control Roles-Edit | Create, edit, and delete control roles, does not include updating control roles for users. |
| Custom Clients-View | Read the list of custom clients. Can download compiled custom clients. Cannot read detailed configuration of custom clients. |
| Custom Clients-Edit | Create, edit, and delete custom clients. |

### 개별 권한

| 권한 | 설명 |
|------------|-------------|
| 장치-보기 | 사용자의 장치 목록 정보를 읽습니다. |
| 장치-활성화/비활성화 | 사용자의 장치를 활성화하거나 비활성화합니다. |
| 장치-삭제 | 사용자의 장치를 삭제합니다. 장치를 삭제하려면 먼저 비활성화해야 합니다. |
| 장치-정보 편집 | 사용자의 장치 이름, 장치 사용자 이름(장치의 시스템 사용자 이름이 아니라 RustDesk 사용자 이름), 및 노트를 편집합니다. |
| 장치-전략 변경 | 사용자의 장치 전략을 변경합니다. |
| 감사 로그-보기 | 개인 로그를 읽습니다. 노트를 편집할 수 있습니다. "관리자만 로그에 접근 가능" 옵션이 활성화된 경우에도 가능합니다. |
| 감사 로그-편집 | 개인의 활성 연결을 연결 해제할 수 있습니다. |

### 그룹 범위 권한

| Permission | Description |
|------------|-------------|
| Users-View | Read list information of users within selected user groups. |
| Users-Create | Create non-administrator users within selected user groups. |
| Users-Invite | Invite users via email within selected user groups. |
| Users-Delete | Delete non-administrator users within selected user groups. Users must be disabled before they can be deleted. |
| Users-Enable/Disable | Enable or disable non-administrator users within selected user groups. |
| Users-Edit Email | Change the email of non-administrator users within selected user groups. |
| Users-Edit Password | Change the password of non-administrator users within selected user groups. |
| Users-Edit Note | Change the note of non-administrator users within selected user groups. |
| Users-Manage 2FA | Manage login verification for non-administrator users within selected user groups. Includes enable/disable 2FA enforcement, reset 2FA configuration, disable email login verification. |
| Users-Force Logout | Force logout non-administrator users within selected user groups from all devices. |
| Users-Update Strategy | Change the strategy of non-admin users within selected user groups. |
| Users-Update Control Role | Change the control role of non-admin users within selected user groups. |
| Devices-View | Read list information of devices managed by the current role. |
| Devices-Enable/Disable | Enable or disable devices managed by the current role. |
| Devices-Delete | Delete devices managed by the current role. Devices must be disabled before they can be deleted. |
| Devices-Edit Info | Edit device name, device username (system username of the device, not the RustDesk user), and note for devices managed by the current role. |
| Devices-Update Strategy | Change strategy of devices managed by the current role. |