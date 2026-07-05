---
title: NAT 루프백 문제
weight: 500
pre: "<b>2.5. </b>"
description: "라우터 뒤에 RustDesk를 자체 호스팅할 때 NAT 루프백 또는 헤어핀 NAT 문제를 해결하세요. LAN 클라이언트가 공개 IP 또는 도메인을 사용할 때 실패할 수 있는 이유와 라우터 설정, 로컬 DNS, 또는 hosts 파일 오버라이드를 통해 이를 해결하는 방법을 알아보세요."
keywords: ["rustdesk nat loopback", "rustdesk hairpin nat", "rustdesk local dns", "rustdesk hosts file", "rustdesk self-hosted domain issue", "rustdesk lan public ip problem"]
---

{{% notice note %}}
이 설명에는 복잡한 네트워킹 지식이 포함되어 있으며, 가독성을 높이기 위해 귀하의 도움이 필요합니다.
{{% /notice %}}


NAT 루프백, 또는 헤어핀 NAT라고도 불리는 이 문제는 동일한 LAN에 있는 장치들이 공개 IP 주소나 도메인 이름을 통해 자체 호스팅된 RustDesk 서버에 접근하려고 할 때 발생합니다. 이 안내서에서는 왜 이런 문제가 발생하는지와 라우터 지원, 로컬 DNS 또는 hosts 파일 오버라이드를 통해 이를 어떻게 해결할 수 있는지 설명합니다.

## 빠른 답변

자체 호스팅된 RustDesk 서버가 LAN 밖에서 작동하지만 같은 LAN의 클라이언트가 공개 IP 주소나 도메인 이름을 사용할 때 연결이 실패한다면, 일반적으로 NAT 루프백이 원인입니다. 가장 좋은 해결책은 라우터에서 헤어핀 NAT를 활성화하는 것입니다. 이것이 불가능하다면 로컬 DNS를 사용하세요. 소수의 장치에 대해서는 hosts 파일 오버라이드가 대안입니다.

## 어떤 해결책을 선택해야 하나요?

| Fix | Best when | Tradeoff |
| --- | --- | --- |
| Enable NAT loopback on the router | Your router supports hairpin NAT | Best long-term fix, but not all routers expose the setting |
| Use local DNS on the LAN | You manage multiple devices on the same network | More scalable than editing every device manually |
| Add hosts file entries | You only need to fix a few devices | Manual and easy to forget on laptops or roaming devices |

NAT 루프백에 대한 자세한 내용은 [위키백과](https://en.m.wikipedia.org/wiki/Network_address_translation#NAT_hairpinning) 페이지를 참조하세요.

홈 네트워크나 NAT 방화벽 뒤에 있는 다른 네트워크 환경에서 RustDesk 서버를 배포할 때, RustDesk 서버와 클라이언트는 **반드시** 다음 중 하나를 사용해야 합니다:
A: 서로 간에 로컬 IP 주소를 사용하거나 OR:
B: NAT 루프백을 지원하고 활성화된 방화벽을 갖추어야 합니다.

귀하가 **공개 IP**나 **도메인**(이론상 공개 IP를 가리킴)을 통해 서버에 연결할 수 없다는 것을 알게 될 수 있습니다.

## 문제
이 예제에서는 LAN 장치들이 `rustdesk.example.com`에 연결하려고 할 때 일어나는 상황을 따라가 보겠습니다. 라우터의 공개 IP는 `172.16.16.1`, 서버의 LAN IP는 `192.168.11.20`, 원하는 도메인은 `rustdesk.example.com`이며, '192.168.11.2'를 사용하는 클라이언트가 있다고 가정해 보겠습니다.

라우터의 NAT 뒤에 서버를 설정할 때, 라우터에 포트 포워딩을 추가해 모든 수신 메시지를 공개 IP 172.16.16.1로 보내는 대신 서버 192.168.11.20으로 보내도록 설정할 수 있습니다.

LAN 장치가 인터넷에 접속하려고 할 때, 예를 들어 8.8.8.8의 웹서버에 요청을 보낼 경우, 해당 요청은 192.168.11.2에서 출발한 것으로 라우터에 전송됩니다. 라우터는 이 요청을 가로채서 8.8.8.8로 보내는 대신 172.16.16.1에서 출발한 것으로 다시 쓰게 됩니다. 8.8.8.8이 172.16.16.1에 응답하면 라우터는 이전 연결을 확인하고 그 응답을 다시 192.168.11.2로 리디렉션합니다.

만약 8.8.8.8의 사용자가 172.16.16.1을 사용해 우리 네트워크에 메시지를 보낸다면, 포트 포워딩 규칙은 172.16.16.1의 목적지를 서버 192.168.11.20으로 바꾸고 요청의 출처는 여전히 8.8.8.8로 유지하여 서버가 직접 8.8.8.8에 응답하도록 합니다.

만약 8.8.8.8의 사용자가 우리 네트워크를 해킹하려고 시도하며 192.168.11.2에서 메시지를 보낸다고 주장한다면, 라우터는 192.168.11.2에서 오는 트래픽이 LAN 장치로부터만 유효하다는 것을 알고 일반적으로 이를 차단합니다.

문제는 LAN 내부로 돌아가는 경우에 발생합니다. 만약 LAN 장치가 `rustdesk.example.com`에 연결하려고 한다면, 이는 `172.16.16.1`가 됩니다. 이때 라우터는 많은 선택지를 고민하게 됩니다. 라우터는 이미 LAN 포트에서 WAN 포트로 192.168.11.2에서 172.16.16.1로 가는 메시지를 보냈으며, WAN 포트에 도착한 이후 이 메시지는 인터넷에서 우리 네트워크를 해킹하려던 위의 예와 구분할 수 없게 됩니다.

NAT 루프백 기능은 이 과정에서 소스 "192.168.11.2" 부분을 효과적으로 변경해, NAT 테이블을 사용해 서버와 클라이언트 간에 메시지를 주고받도록 알려줍니다.

만약 LAN 내부에서만 연결에 문제가 있고 외부에서는 정상적으로 작동한다면, 이것이 바로 귀하가 겪고 있는 문제일 수 있습니다.


## 해결 방법
이 문제를 해결하는 방법은 세 가지가 있습니다.

### 1. 라우터에서 NAT 루프백 설정하기
네트워킹 지식이 있다면 라우터에서 NAT 루프백을 설정할 수 있지만, 이를 설정하려면 네트워킹 지식이 필요합니다. 일부 라우터는 이 설정을 조정할 수 없으므로 모두에게 최적의 옵션은 아닙니다.

{{% notice note %}}
[MikroTik](https://help.mikrotik.com/docs/display/ROS/NAT#NAT-HairpinNAT)의 기사가 이를 매우 잘 설명하고 있습니다. 여기서부터 학습을 시작해보세요.
{{% /notice %}}

### 2. LAN에 DNS 서버 배포하기
먼저, [AdGuard Home](https://github.com/AdguardTeam/AdGuardHome/wiki/Docker)와 [Pi-hole](https://github.com/pi-hole/docker-pi-hole) 중에서 선호하는 것을 선택하세요. Docker를 통해 배포하거나 RustDesk 서버와 같은 서버에 배포할 수도 있습니다. 아래 예제는 이 방법에 대한 몇 가지 단계를 보여줍니다.

두 가지 모두 DNS 기반 광고 차단기이지만, 광고 차단 기능을 비활성화할 수도 있습니다.

먼저, `domain`를 RustDesk 서버의 LAN IP(예: `192.168.11.20`)로 설정하세요. 그런 다음 라우터의 `DHCP` 설정으로 이동하고(주의: WAN이 아님), `First` DNS IP를 AdGuard Home이나 Pi-hole을 배포한 서버로 설정하세요. `Secondary` DNS는 ISP의 DNS나 다른 공용 DNS, 예를 들어 `1.1.1.1` Cloudflare나 `8.8.8.8` Google의 DNS로 설정할 수 있으며, 이렇게 하면 끝납니다!

다음은 예제입니다:
#### AdGuard Home
광고 차단은 문제를 일으킬 수 있으니, 해결 방법을 찾고 싶지 않고 이 기능을 비활성화하고 싶다면 "보호 비활성화" 버튼을 클릭하세요.

![](/docs/en/self-host/nat-loopback-issues/images/adguard_home_disable_protection.png)
<br>

"DNS 재작성" 설정으로 이동하세요.

![](/docs/en/self-host/nat-loopback-issues/images/adguard_home_click_dns_rewrites.png)
<br>

"DNS 재작성 추가"를 클릭한 후, 필드에 `domain`와 서버의 `LAN IP`를 입력하세요.

![](/docs/en/self-host/nat-loopback-issues/images/adguard_home_dns_rewrite_dialog.png)

최종 결과는 다음과 같습니다.

![](/docs/en/self-host/nat-loopback-issues/images/adguard_home_dns_rewrite_final_result.png)

***AdGuard Home을 라우터의 LAN DHCP에 할당하는 것을 잊지 마세요!***
<hr>

#### Pi-hole
광고 차단은 문제를 일으킬 수 있으니, 해결 방법을 찾고 싶지 않고 이 기능을 비활성화하고 싶다면 "차단 비활성화" 하위 메뉴에서 "무기한" 버튼을 클릭하세요.

![](/docs/en/self-host/nat-loopback-issues/images/pi_hole_disable_blocking.png)

"로컬 DNS → DNS 레코드"로 이동하세요.
필드에 `domain`와 `IP`를 입력한 후 "추가"를 클릭하세요.

최종 결과를 확인하려면 이 사진의 노란색 선을 확인하세요.

![](/docs/en/self-host/nat-loopback-issues/images/pi_hole_local_dns_dns_records.png)

***Pi-hole을 라우터의 LAN DHCP에 할당하는 것을 잊지 마세요!***

### 3. hosts 파일에 규칙 추가하기
이 방법은 장치가 소수일 때만 권장됩니다. 장치가 많다면 DNS 방법을 사용하는 것이 더 좋습니다. 그렇지 않으면 서버에 접근해야 하는 각 장치마다 수동으로 작업해야 합니다.

{{% notice warning %}}
이 방법을 노트북과 같은 휴대용 장치에서 사용하면 LAN 밖에서는 서버에 연결할 수 없습니다.
{{% /notice %}}

각 운영체제별 경로:

#### Windows
```text
C:\Windows\system32\drivers\etc\hosts
```
관리자 권한으로 편집하거나 이 파일을 `Desktop`로 복사해 편집할 수 있습니다. 편집한 후 원래 경로로 다시 복사하세요.

#### macOS
```text
/etc/hosts
```
`vim`을 사용할 수 있으며, 미리 설치되어 있습니다.
```sh
sudo vim /etc/hosts
```

#### Linux
```text
/etc/hosts
```
`vim`나 `nano`를 사용할 수 있습니다.
```sh
sudo vim /etc/hosts
```

<hr>

형식은 세 운영체제 모두 동일합니다. `IP`를 먼저 입력한 후 `domain`를 입력하세요. 한 줄에 하나씩 입력합니다.

예를 들어:
```text
192.168.11.20   rustdesk.example.com
```
