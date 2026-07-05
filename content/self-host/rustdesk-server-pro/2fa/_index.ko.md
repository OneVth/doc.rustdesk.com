---
title: 2FA
weight: 16
description: "RustDesk 서버 프로에서 이메일 인증 또는 Authy, Microsoft Authenticator, Google Authenticator와 같은 TOTP 앱을 사용하여 2단계 인증을 활성화하십시오."
keywords: ["rustdesk 2fa", "rustdesk totp", "rustdesk email verification", "rustdesk authenticator", "rustdesk server pro 2fa"]
---

이 안내서를 사용하여 RustDesk Server Pro에서 이중 인증을 활성화하여 계정 보안을 강화하십시오.

계정에 로그인할 때 이중 인증(2FA) 확인을 켜면 계정 보안을 높일 수 있습니다.

## 어떤 2FA 방법을 선택해야 하나요?

| Method | Best for | Why you would use it |
| --- | --- | --- |
| Email verification | Simpler deployments that already rely on SMTP | Easiest to roll out when users do not already use an authenticator app |
| TOTP | Stronger day-to-day account security | More secure and more standard for long-term admin and operator accounts |

## 2FA 빠른 답변

- RustDesk Server Pro는 이메일 인증과 TOTP를 지원합니다.
- TOTP가 활성화되면 이메일 로그인 인증은 더 이상 사용되지 않습니다.
- TOTP 설정 시 6개의 백업 코드가 생성됩니다.
- 백업 코드는 한 번만 사용할 수 있습니다.
- 만료된 2FA도 여전히 작동하지만, 다시 활성화하면 비밀번호가 새로 고쳐져 보안이 향상됩니다.

현재 저희 웹 콘솔은 두 가지 종류의 2FA를 지원합니다:

1. 이메일 인증
2. TOTP. 인증 코드를 생성하려면 [Authy](https://authy.com), [Microsoft Authenticator](https://www.microsoft.com/en-us/security/mobile-authenticator-app/), [Google Authenticator](https://play.google.com/store/apps/details?id=com.google.android.apps.authenticator2)와 같은 타사 인증 앱이 필요합니다.

먼저 계정 설정 페이지로 이동해야 합니다.

![](/docs/en/self-host/rustdesk-server-pro/2fa/images/1-settings-account.png)

## 이메일 인증

로그인용 이메일 인증을 활성화하려면 다음이 필요합니다:

1. 이메일을 설정하세요.
2. `Enable email login verification` 옵션을 활성화하세요.
3. `Submit`를 클릭하세요.

![](/docs/en/self-host/rustdesk-server-pro/2fa/images/2-2fa-email-1.png)

다음에 로그인할 때 RustDesk는 인증 코드 이메일을 보내며, 웹 페이지도 인증 페이지로 이동합니다.

![](/docs/en/self-host/rustdesk-server-pro/2fa/images/2-2fa-email-2.png)

## TOTP

TOTP는 널리 사용되는 2FA 방법이므로, RustDesk Server Pro의 웹 콘솔에서는 2FA가 TOTP 인증을 의미합니다.

### 인증 앱 준비하기

먼저 인증 앱을 준비해야 합니다.
[Authy](https://authy.com), [Microsoft Authenticator](https://www.microsoft.com/en-us/security/mobile-authenticator-app/), [Google Authenticator](https://play.google.com/store/apps/details?id=com.google.android.apps.authenticator2)와 같은 인증 앱 중에서 선택할 수 있습니다.

### 2FA 활성화하기

설정 페이지에 `Enable 2FA` 버튼이 표시되면 현재 2FA가 활성화되지 않은 상태입니다.

![](/docs/en/self-host/rustdesk-server-pro/2fa/images/3-2fa-enable-1.png)

버튼을 클릭하면 2FA를 활성화하는 양식이 나타납니다.

![](/docs/en/self-host/rustdesk-server-pro/2fa/images/3-2fa-enable-2.png)

인증 앱을 열고 QR코드를 스캔하여 계정을 추가하세요.

QR코드 스캔이 불편하다면 여기에 직접 코드를 입력할 수도 있습니다.

![](/docs/en/self-host/rustdesk-server-pro/2fa/images/3-2fa-enable-3.png)

인증 앱에 계정을 추가한 후, 인증 앱에서 인증 코드를 입력해 2FA를 켜세요.

2FA가 성공적으로 켜지면 RustDesk Server Pro는 6개의 **백업 코드**와도 연결됩니다. 따라서 인증 앱을 사용할 수 없는 경우에도 이 **백업 코드**를 사용해 인증을 통과할 수 있습니다.

![](/docs/en/self-host/rustdesk-server-pro/2fa/images/3-2fa-enable-4.png)

{{% notice note %}}
1. 이 백업 코드는 한 번만 사용할 수 있습니다.

2. 백업 코드를 안전한 곳에 보관해 주세요.
{{% /notice %}}

### 로그인 인증

2FA가 활성화되면 이메일 로그인 인증은 더 이상 사용되지 않습니다. 대신 2FA 로그인 인증을 사용하게 됩니다.

![](/docs/en/self-host/rustdesk-server-pro/2fa/images/3-2fa-enable-login-5.png)

로그인 시 인증 페이지로 리디렉션됩니다.

![](/docs/en/self-host/rustdesk-server-pro/2fa/images/3-2fa-enable-login-6.png)

### 설정 수정하기

2FA가 활성화된 상태에서는 계정 설정을 수정할 때 추가로 2FA 인증이 필요합니다.

![](/docs/en/self-host/rustdesk-server-pro/2fa/images/3-2fa-settings-1.png)

![](/docs/en/self-host/rustdesk-server-pro/2fa/images/3-2fa-settings-2.png)

### 2FA 상태

2FA에는 총 3가지 상태가 있습니다: 비활성화, 활성화, 만료됨.

![](/docs/en/self-host/rustdesk-server-pro/2fa/images/3-2fa-state-not-enabled.png)

![](/docs/en/self-host/rustdesk-server-pro/2fa/images/3-2fa-state-enabled.png)

![](/docs/en/self-host/rustdesk-server-pro/2fa/images/3-2fa-state-expired.png)

{{% notice note %}}
2FA는 만료된 후에도 정상적으로 사용할 수 있습니다. 다만 오랜 기간 동안 2FA 설정이 변경되지 않았다는 것을 의미합니다(기본 180일). 보안을 위해 2FA를 다시 활성화해 비밀 데이터를 업데이트하는 것이 좋습니다.
{{% /notice %}}