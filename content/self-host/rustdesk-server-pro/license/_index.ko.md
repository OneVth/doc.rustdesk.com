---
title: 라이선스
weight: 15
description: "웹 콘솔에서 RustDesk Server Pro 라이선스를 구매하고, 입력하고 관리하십시오. 라이선스를 어디서 얻을 수 있는지, 구매 후 어떻게 활성화하거나 변경하는지 알아보십시오."
keywords: ["rustdesk pro license", "rustdesk server pro activate", "rustdesk pricing license", "rustdesk change license", "rustdesk web console license"]
---

이 페이지를 사용하여 RustDesk Server Pro 라이선스를 구매하고 활성화하며 업데이트하는 방법을 이해하십시오.

## RustDesk Server Pro 라이선스는 어떻게 작동하나요?

RustDesk Server Pro 라이선스는 모든 릴레이 노드가 아닌 `hbbs` 서버에 연결됩니다. 가격 페이지를 통해 라이선스를 구매한 후 웹 콘솔에 입력하고, 셀프 서비스 라이선스 포털을 통해 갱신, 업그레이드, 청구서 및 마이그레이션을 관리하십시오.

## 라이선스 관련 간단한 답변

- 한 번에 하나의 라이선스는 하나의 `hbbs` 머신에 할당됩니다.
- `hbbr` 릴레이 서버는 별도의 라이선스가 필요하지 않습니다.
- 갱신과 업그레이드는 셀프 서비스 포털에서 처리한 후 웹 콘솔에서 라이선스를 새로 고침하여 활성화합니다.
- 마이그레이션은 이전 머신의 바인딩 해제와 새 서버에 라이선스 설정으로 이루어집니다.
- 서버가 인터넷에 직접 접속할 수 없는 경우, 라이선스 확인을 위해 프록시를 구성할 수 있습니다.

## 라이선스 구매하기

라이선스는 [https://rustdesk.com/pricing.html](https://rustdesk.com/pricing.html)에서 받으시고, Stripe 결제 페이지에서 유효한 이메일 주소를 입력하십시오. 결제가 성공적으로 완료되면 라이선스(및 별도의 메일로 청구서)가 이메일로 전송됩니다.

![](/docs/en/self-host/rustdesk-server-pro/license/images/stripe.jpg)

## 라이선스 설정하기

웹 콘솔(`http://<rustdesk-server-pro-ip>:21114`)에서 라이선스를 입력하거나 나중에 라이선스를 변경해야 합니다.

| Set license | Change license |
| --- | --- |
| ![](/docs/en/self-host/rustdesk-server-pro/license/images/set.png) | ![](/docs/en/self-host/rustdesk-server-pro/license/images/change.png) |

## 라이선스 갱신/업그레이드하기

갱신/업그레이드 라이선스는 아래 설명된 대로 [셀프 서비스 라이선스 포털](https://rustdesk.com/self-host/account/)에서 찾을 수 있으며, 위 그림과 같이 라이선스를 구매할 때 사용한 이메일로 로그인하십시오.

| License page with renew/upgrade actions | Upgrade window |
| --- | --- |
| ![](/docs/en/self-host/rustdesk-server-pro/license/images/renew.jpg?v2) | ![](/docs/en/self-host/rustdesk-server-pro/license/images/upgrade.png) |

결제 후, 아래와 같이 라이선스를 새로 고침하여 활성화하십시오([/docs/en/self-host/rustdesk-server-pro/license/#refresh-license] 참조).

### 라이선스 새로 고침
결제 후, 웹 콘솔로 이동해 아래와 같이 수동으로 활성화해야 합니다. `Edit`를 클릭한 다음 `OK`를 클릭하면 되며, 아무것도 수정할 필요가 없습니다. 라이선스 키는 동일하게 유지되기 때문입니다.

![](/docs/en/self-host/rustdesk-server-pro/license/images/updatelic.jpg)

## 청구서, 라이선스 회수 및 마이그레이션

라이선스는 한 대의 머신에서만 사용할 수 있습니다(단, hbbs용이며, hbbr는 라이선스가 필요하지 않습니다). 다른 머신으로 마이그레이션하거나 라이선스를 회수하거나 청구서를 다운로드하려면 [https://rustdesk.com/self-host/account/](https://rustdesk.com/self-host/account/)로 이동하십시오. Stripe 결제에 사용한 이메일 주소로 로그인한 후, 아래와 같이 마이그레이션하려는 이전 머신의 바인딩을 해제하십시오. 새 서버의 웹 콘솔에서 라이선스를 설정하면 자동으로 라이선스가 할당되고 콘솔에 등록됩니다.

![](/docs/en/self-host/rustdesk-server-pro/license/images/unbind.jpg)

## 프록시
서버가 인터넷에 직접 접속해 라이선스를 확인할 수 없는 경우, 프록시를 추가할 수 있습니다. 예를 들어 `proxy=http://username:password@example.com:8080 ./hbbs`를 사용할 수 있습니다.

또는 작업 디렉터리 내의 `proxy=http://username:password@example.com:8080`부터 `.env` 파일에 추가할 수 있습니다(여기에는 `id_ed25519` / `db.sqlite3` 파일이 저장됨).

`http`는 `https` 또는 `socks5`로 교체할 수 있습니다. 만약 `username` / `password` / `port`가 없다면 `proxy=http://example.com`로 대체할 수 있습니다.