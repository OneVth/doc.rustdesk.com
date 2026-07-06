---
title: 윈도우
description: "Windows에 RustDesk를 설치하고 배포하세요. Windows 클라이언트 설정 안내, MSI 패키징, 무인 설치 옵션 및 휴대용 권한 상승 문서를 확인하세요."
keywords: ["rustdesk windows", "rustdesk windows install", "rustdesk msi", "rustdesk silent install", "rustdesk portable elevation", "rustdesk windows deployment"]
weight: 4
---

이 Windows 가이드를 사용하여 RustDesk를 설치하고, Windows 클라이언트를 패키지화하거나 배포하며, 관리되는 환경에서 MSI 또는 휴대용 권한 상승 워크플로우를 처리하십시오.

## 어떤 Windows 가이드를 선택해야 하나요?

| Need | Best guide |
| --- | --- |
| Standard Windows client install | [Windows client overview](/docs/ko/client/windows/) |
| Managed deployment, silent install, or packaging | [MSI](/docs/ko/client/windows/msi/) |
| Portable mode with elevation support | [Windows Portable Elevation](/docs/ko/client/windows/windows-portable-elevation/) |

## Windows 빠른 답변

- 대부분의 최종 사용자는 표준 설치 프로그램을 사용하십시오.
- 기업용 롤아웃 및 스크립트화된 배포에는 MSI 가이드를 사용하십시오.
- 휴대용 모드에서 관리자 프롬프트가 필요한 경우 휴대용 권한 상승을 사용하십시오.
- 자체 호스팅하는 경우, 이러한 가이드와 [Client Deployment](/docs/ko/self-host/client-deployment/)를 결합하여 서버 측 롤아웃 설정을 구성하십시오.

{{% children depth="3" showhidden="true" %}}