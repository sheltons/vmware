---
copyright:
  years: 1994, 2017
lastupdated: "2017-12-18"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# 원격 콘솔 및 가상 미디어를 통한 VMware vSphere ESXi 설치

설치 미디어에서 ESX 배치는 모든 버전에서 유사하며 기존에 보유한 고유 라이센스 사용(BYOL) 시나리오에 사용할 수 있습니다.

## 요구사항
* 사설 네트워크에 대한 액세스 권한이 있는 가상 머신(VM) 인스턴스[Java 지원 브라우저를 사용하여 Windows 또는 Linux에 설치되는 클라우드 컴퓨팅 인스턴스는 vpn.softlayer.com 또는 공용 IP를 통해 액세스 가능함]. VSI는 서버의 IPMI(Intelligent Platform Management Interface) 주소가 위치하는 동일한 사설 네트워크에 있어야 합니다. 
* VMware ESXi VIM 설치 프로그램 ISO의 사본

<!--## Steps -->

## IPMI에 연결
1. 요구사항에 지정되어 있는 VSI에 VMware ISO를 업로드하십시오. (VSI에는 공용 웹 액세스 권한이 있어야 합니다.)
2. 고객 포털에서 IPMI 주소 및 로그인 정보를 수집하십시오.
3. **디바이스** > **디바이스 목록** > **[서버]** > **원격 관리** 탭을 선택하십시오.
4. ESXi 이미지를 저장하는 VSI에 원격 데스크탑(RDP) 또는 원격 X를 연결하십시오.
5. RDP 세션에서 웹 브라우저를 열고 2단계에서 수집한 IPMI 주소를 입력하십시오.
6. 또한 2단계에서 발견된 신임 정보를 사용하여 IPMI 콘솔에 로그인하십시오(일반적으로 루트임).
* IPMI 사용자 액세스 레벨을 기록해 두십시오. 관리자일 수도 있습니다. **운영자**로 설정되어 있는 경우 원격 스토리지에 마운트할 때 문제점이 발생할 수 있습니다. 미디어를 마운트할 수 없는 경우에는 IPMI 신임 정보를 높이는 지원 티켓을 보관하십시오.
7. 홈 로그인  페이지에 있는지 확인하고 **원격 제어** > **콘솔 경로 재지정** > **콘솔 실행**을 클릭하십시오.

## ISO 연결
1. 커널 기반 가상 머신(KVM) 뷰어에서 **가상 미디어** > **가상 스토리지**를 선택하십시오. 사용자 운영 체제 및 Java 버전은 사용자가 Java 기반 뷰어를 시작하도록 액세스를 허용해야 할 수 있습니다. 
2. 가상 스토리지 창에서 **CDROM & ISO** 탭을 클릭하십시오. 논리 드라이브 유형으로 **ISO 파일**을 선택하고 **이미지 열기**를 클릭하십시오.
3. ESXi ISO로 이동하여 **확인**을 클릭하십시오. 그런 다음 **플러그 인**을 클릭하십시오.
4. ISO가 세션에 연결된 후 **확인**을 클릭하십시오.
* IPMI 웹 페이지로 돌아가서 **원격** > **제어** > **서버 재설정** > **조치 수행**을 클릭하여 서버를 다시 시작하십시오. 

### 네트워킹 구성 설치 및 완료
1. ESXi를 설치하십시오.
* 설치가 완료되고 나면 서버를 다시 시작할 수 있도록 ISO에서 **플러그 아웃**하십시오.
2. 서버를 다시 시작하십시오.
3. 재시작된 후에 서버의 IP 주소를 구성하십시오.
4. 디바이스의 구성 탭에서 세부사항을 수집하십시오.

이제 ESXi 호스트를 관리할 수 있습니다.

내구성(Endurance) 또는 성능(Performance) 블록 스토리지를 설정하는 방법에 대한 자세한 정보는 [블록 스토리지 프로비저닝 및 관리](/docs/infrastructure/BlockStorage/provisioning-block_storage.html)를 참조하십시오.

QuantaStor 설정에 대한 자세한 정보는 [QuantaStor용 아키텍처 안내서](architecture-guide-quantastor-vmwaresoftlayer.html)를 참조하십시오.
