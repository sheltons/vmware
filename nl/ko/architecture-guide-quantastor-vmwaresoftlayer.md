---
copyright:
  years: 1994, 2017
lastupdated: "2017-12-18"
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# QuantaStor용 아키텍처 안내서

VMware ESXi 5 환경에 대한 OSNexus QuantaStor 공유 스토리지 솔루션을 주문 및 구성할 수 있습니다. [고급 단일 사이트 VMware 참조 아키텍처](/docs/infrastructure/virtualization/advanced-single-site-vmware-reference-architecturesoftlayer.html) 쿡북과 함께 다음 정보를 사용하여 VMware 환경에 스토리지 옵션 중 하나를 설정하십시오. 

## 1단계: QuantaStor 공유 스토리지 주문

주문하기 전에 사용할 용량 및 I/O 요구사항을 충족시키는 스토리지의 스펙을 판별해야 합니다. 이러한 스펙에는 서버 RAM, 디스크 드라이브의 유형 및 개수, 캐싱을 위한 SSD, RAID 구성 및 네트워킹 구성이 포함됩니다(이 항목들로 제한되지는 않음). 사용자 환경에 대한 올바른 QuantaStor 구성을 선택하는 데 대한 자세한 정보는 [QuantaStor Solution Design Guide for Virtualization ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](http://wiki.osnexus.com/index.php?title=Solution_Design_Guide#Server_Virtualization){: new_window}을 참조하십시오.

예제 환경의 경우 가상 머신(VM)에 필요한 충분한 용량과 I/O를 제공할 수 있는 더 작은 구성이 사용됩니다. 주문하기 전에 VM의 용량 및 I/O 요구사항을 먼저 파악해야 합니다. QuantaStor 서버가 확장 가능하지만, 구성 및 배치에서 지연이 발생하지 않도록 초기에 하드웨어의 크기를 적절하게 조정하는 것이 중요합니다. 

### QuantaStor 주문

다음 단계에 따라 QuantaStor 서버를 주문하십시오. 

1. [{{site.data.keyword.slportal_full}} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://control.softlayer.com/){: new_window}에 로그인하고  **계정 > 주문하기**를 클릭하십시오.
2. 팝업 화면에서 {{site.data.keyword.baremetal_short}}, 월별을 선택하십시오.
3. 서버 목록 페이지에서 환경에 필요한 디스크 수를 저장할 수 있는 적절한 서버를 선택하십시오. [예제 환경의 경우 12개 드라이브 베이가 있는 12개 코어 시스템(즉, 듀얼 6개 코어)이 선택됩니다.]
4. 다음 구성 옵션을 입력하십시오.
  * **데이터 센터:** 이전에 작성된 VLAN 및 ESXi 호스트의 위치
  * **서버:** 듀얼 프로세서 6개 코어 제온
  * **RAM:** 64GB
  * **운영 체제:** OSNexus QuantaStor 3.x(4TB)
  * **하드 드라이브:**
    * 디스크 1 및 2: 500GB SATA(RAID 1)
    * 파티션 템플리트: Linux Basic
    * 디스크 3-10: 600GB SAS 15K(RAID-10)
  * **공용 대역폭:** 사설 네트워크 전용
  * **업링크 포트 속도:** 10Gbps 중복 사설 네트워크 업링크
5. **주문 계속**을 클릭하십시오.

**참고:** 스토리지 서버는 스토리지 배열에 대한 트래픽 로드를 조정하는 데 두 가지 다른 서브넷을 사용할 수 있도록 두 개의 네트워크 인터페이스로 구성됩니다. 

### 구성 완료

이제 장바구니에 QuantaStor 어플라이언스가 담겼습니다. 이제 디바이스가 올바르게 프로비저닝될 수 있도록 사설 VLAN, 호스트 이름 및 도메인을 디바이스에 지정해야 합니다. 

1. 다음 VLAN을 지정하고 디바이스에 대한 호스트 이름을 작성하십시오. QuantaStor 호스트 – 백엔드 VLAN: (환경에서 1102)
2. 지불 방법을 선택하고 이용 약관에 동의한 후 주문 완료를 클릭하십시오.

**참고** 서버가 프로비저닝되어 VPN 또는 가상 서버를 통해 액세스할 수 있을 때까지 3단계를 진행하지 마십시오.

## 2단계: 관리 및 용량 호스트에 대한 iSCSI 소프트웨어 어댑터 사용

각 관리 및 용량 호스트에서 iSCSI 소프트웨어 어댑터를 사용으로 설정해야 하며 iSCSI 볼륨 전에 수집된 해당 IQN(iSCSI Qualified Name)이 마운트됩니다. iSCSI 소프트웨어 어댑터를 사용으로 설정하려면 다음 단계를 사용하십시오.

1. ESXi 관리 또는 용량 호스트로 이동하여 관리, 스토리지, 스토리지 어댑터를 선택하십시오.
2. 초록색 플러스 부호(+)를 클릭하여 iSCSI 소프트웨어 어댑터를 추가하십시오.
3. iSCSI 소프트웨어 어댑터에 해당하는 vmhba를 클릭하고 어댑터가 사용으로 설정되고 나면 어댑터 세부사항 섹션에 있는 iSCSI 이름(그림 1)을 기록하십시오.
4. 각 ESXi 관리 및 용량 호스트의 각 iSCSI 소프트웨어 어댑터에서 1 - 3단계를 수행하십시오.

## 3단계: QuantaStor 구성

서버가 프로비저닝되면 공유된 스토리지 디바이스를 설정할 수 있습니다. 구체적으로, 네트워킹을 설정하고 iSCSI 볼륨을 구성하고 호스트에 볼륨을 지정합니다. 

예를 들어, 볼륨 vmk3에 스토리지 VLAN의 포터블 사설 서브넷 A에 연결되는 vmnic0이 포함되어 있습니다. 볼륨 vmk4에는 스토리지 VLAN의 포터블 사설 서브넷 B에 연결되는 vmnic2가 포함되어 있습니다. 또한 QuantaStor 서버에도 스토리지 VLAN에 연결되는 두 개의 사설 네트워크 어댑터 eth4 및 eth6가 있습니다. 

### 네트워킹 구성

1. 웹 브라우저를 열고 [{{site.data.keyword.slportal_short}} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://control.softlayer.com/){: new_window}의 **디바이스** 페이지에 나열된 QuantaStor IP 주소로 이동하십시오.
2. **관리자 이름** 및 **비밀번호**를 입력하십시오(**디바이스 세부사항** 페이지의 비밀번호 섹션에서 참조). 계속하기 전에 QuantaStor 서버에서 사용하는 사설 네트워크 디바이스를 기록해 두십시오(예: eth4).
3. **스토리지 시스템 > 네트워크 포트**로 이동하십시오.
4. 네트워크 어댑터 목록에서 활성 상태인 사설 어댑터(예: eth4)를 선택하십시오. 마우스 오른쪽 단추를 클릭하여 팝업 메뉴에서 **네트워크 포트 수정**을 선택하십시오.
5. **iSCSI 사용** 선택란을 선택 취소하여 이 어댑터에 대한 iSCSI 연결을 사용 안함으로 변경한 후 **확인**을 클릭하십시오.
6. 활성 상태가 아니지만 사설 네트워크에 지정되어 있는 사설 어댑터를 선택하십시오(예: eth6).
7. eth6 어댑터를 마우스 오른쪽 단추로 클릭하고 팝업 메뉴에서 **네트워크 포트 수정**을 선택하십시오.
8. **네트워크 포트 수정** 화면에서 **정적**의 **구성 유형**을 선택하십시오.
9. 어댑터에 대한 기본 사설 IP 주소, 서브넷 및 게이트웨이를 입력하십시오. [고급 단일 사이트 VMware 참조 아키텍처](/docs/infrastructure/virtualization/advanced-single-site-vmware-reference-architecturesoftlayer.html) 쿡북에서 VLAN 워크시트를 사용 중인 경우 스토리지 행의 주소를 사용하십시오.
10. **iSCSI 사용** 선택란을 선택 취소하여 이 어댑터에 대한 iSCSI 연결을 사용 안함으로 설정하십시오.
11. 사설 어댑터를 마우스 오른쪽 단추로 클릭하고 IP 주소로 구성한 후에 **네트워크 포트 사용**을 선택하여 어댑터를 온라인으로 설정하십시오.
12. IP 주소 확인 시 포트를 사용으로 설정하려면 **확인**을 클릭하십시오.

### 지원 티켓 열기

기본 사설 IP 주소로 두 번째 어댑터를 구성한 후 지원 티켓을 열어야 합니다. 티켓을 열면 VLAN에서 다른 머신이 프로비저닝되는 경우 사용자가 사용한 IP 주소는 사용되지 않습니다.

1. **지원** > **티켓 추가**를 클릭하고 다음 정보를 입력하십시오.
  * 주제: 사설 네트워크 질문
  * 제목: 사설 IP 주소 예약 및 지정
  * 디바이스 연관: 'QuantaStor 서버 선택'
  * 세부사항: VLAN에서 예약 및 지정. 이 IP 주소는 QuantaStor 서버의 두 번째 어댑터에 사용됩니다.

### QuantaStor 구성

이제 배열은 두 가지 어댑터의 연결을 모두 허용할 수 있으며, 스토리지 경로 A 및 스토리지 경로 B 서브넷에 있는 어댑터의 가상 인터페이스를 지정해야 합니다.

  1. 초기 사설 어댑터 인터페이스(eth4)를 마우스 오른쪽 단추로 클릭하고 팝업 메뉴에서 **가상 인터페이스 작성**을 선택하십시오.
  2. 결과 팝업 화면에서 포터블 사설 IP 주소 및 어댑터의 서브넷을 입력하고 **iSCSI 사용**이 선택되어 있는지 확인하십시오.
  3. VLAN 워크시트를 사용 중인 경우 스토리지 경로 A 행의 주소를 사용하십시오.
  4. 포터블 IP 주소 페이지의 참고 섹션에 사용되는 IP 주소를 기록하십시오.
  5. 초기 사설 어댑터 인터페이스(eth4)를 선택하여 가상 인터페이스를 바인드하고 **확인**을 클릭하십시오.
  6. 다른 사설 어댑터 인터페이스(eth6)를 마우스 오른쪽 단추로 클릭하고 팝업 메뉴에서 **가상 인터페이스 작성**을 선택하십시오.
  7. 포터블 사설 IP 주소 및 어댑터의 서브넷을 입력하고 결과 팝업 창에서 **iSCSI 사용**이 선택되었는지 확인하십시오.
  8. VLAN 워크시트를 사용 중인 경우 스토리지 경로 B 행의 주소를 사용하십시오.
  9. 포터블 IP 주소 페이지의 참고 섹션에 사용된 대로 이 IP 주소를 기록하십시오.
  10. 다른 사설 어댑터 인터페이스(eth6)를 선택하여 가상 인터페이스를 바인드하고 **확인**을 클릭하십시오.

IP 주소 및 가상 인터페이스로 QuantaStor 서버가 구성되고 나면 (다른 서브넷에 두 개의 NIC가 있으므로) 발신 트래픽에서 올바른 인터페이스를 사용하도록 라우팅을 구성해야 합니다.

1. 루트 신임 정보를 사용하여 QuantaStor 서버로 SSH를 수행하고 다음 라인을 /etc/network/interfaces에 추가하십시오. eth4 및 eth6는 사설 네트워크에서 NIC라고 가정합니다. 
  * post-up ip route add 10.0.0.0/8 via dev eth4
  * post-up ip route add 10.0.0.0/8 via dev eth6 table eth6
  * post-up ip rule add from table eth6

### 스토리지 풀 작성

다음으로 볼륨 또는 공유를 작성할 수 있으려면 볼륨을 할당하는 데 사용되는 스토리지 풀을 작성해야 합니다. 

1. **스토리지 풀**을 마우스 오른쪽 단추로 클릭하고 **스토리지 풀 작성**을 선택하십시오.
2. **스토리지 풀 작성** 화면에서 다음 정보를 입력하십시오.
  * 이름: 스토리지 풀의 적절한 이름을 입력하십시오. 예: StoragePool-01
  * 풀 유형: 기본값(zfs)
  * I/O 프로파일: 가상화
  * 스토리지 풀에 이용할 디스크: sdb
3. **확인**을 클릭하십시오.

풀에 추가하는 데 sdb를 사용할 수 없는 경우 부트 가능으로 태그가 지정되는 파티션이 존재하기 때문입니다. 파티션과 모든 /etc/fstab 항목을 제거하려면 Quantastor 콘솔에서 gdisk를 사용해야 합니다. 다음으로, 사용할 ESXi에 대한 볼륨을 작성해야 합니다.

### iSCSI 스토리지 볼륨 작성

두 개의 스토리지 볼륨을 작성해야 합니다. 하나의 볼륨은 관리 클러스터의 관리 VM에 사용되며 다른 볼륨은 용량 클러스터의 VM에 사용됩니다. 다음 단계를 완료하여 iSCSI 스토리지 볼륨을 작성하십시오. 

1. **스토리지 볼륨**을 마우스 오른쪽 단추로 클릭하고 팝엽 메뉴에서 **스토리지 볼륨 작성**을 선택하십시오. 
2. 스토리지 볼륨에 대한 정보를 입력하십시오. **참고:** 워크로드 및 스토리지 용량에 따라 구성이 다를 수 있지만, 예에서는 각 스토리지 볼륨에 대해 표 1 및 표 2에 해당 값을 표시합니다. 

|필드|값|
|---|---|
|이름|Mgmt-LUN0|
|스토리지 풀|[이전 단계에서 구성된 스토리지 풀 이름]|
|크기|500GB|
|% 예약됨|50|
|압축|사용|
{: caption="표 1. iSCSI 관리 볼륨" caption-side="top"}

|필드|값|
|---|---|
|이름|Capacity-LUN0|
|스토리지 풀|[이전 단계에서 구성된 스토리지 풀 이름]|
|크기|3TB|
|% 예약됨|50|
|압축|사용|
{: caption="표 2. iSCSI 용량 볼륨" caption-side="top"}

### 볼륨에 대한 호스트 액세스 지정

볼륨이 설정된 후에 각 호스트의 IQN을 통해 ESXi 호스트의 액세스를 허용하려면 QuantaStor를 구성해야 합니다. 

1. QuantaStor 관리 페이지로 이동하여 **호스트** 메뉴를 마우스 오른쪽 단추로 클릭한 후 **호스트 추가**를 선택하십시오.
2. **호스트 추가** 화면에서 다음 정보를 입력하십시오.
  * 호스트 이름: 적절한 호스트 이름을 입력하십시오. 호스트 이름은 완전한 도메인 이름(FQDN)을 포함하지 않지만 호스트에 대해 설명해야 합니다. 예: MyESXiHostName
  * 운영 시스템 유형: VMware
  * 개시자: iSCSI Qualified Name(IQN) 단일 선택 단추
  * iSCSI 규정된 이름: 각 호스트에 대한 IQN.
3. **확인**을 클릭하십시오.

ESXi 환경에 있는 각 관리 및 용량 호스트에 대해 다음 단계를 수행하십시오.

관리 및 용량 클러스터에 각 호스트를 추가한 후에 다음 단계를 수행하십시오.

1. **호스트 그룹** 메뉴를 마우스 오른쪽 단추로 클릭하고 **호스트 그룹 작성…**을 선택하십시오.
2. **이름** 필드에 'ManagementCluster'를 입력하고 관리 클러스터에 있는 모든 호스트를 선택하십시오.
3. **확인**을 클릭하십시오. 특정 볼륨에 지정할 수 있는 호스트 그룹이 작성됩니다.

용량 클러스터에 이 프로세스를 반복하십시오.

1. **스토리지 볼륨** 메뉴를 클릭하십시오.
2. **Mgmt-Lun0** 볼륨을 마우스 오른쪽 단추로 클릭하고 **호스트 액세스 지정/지정 해제**를 선택하십시오.
3. **Mgmt-Lun0**이 드롭 다운 메뉴에 있는 옵션인지 확인하고 이전 단계에서 작성한 호스트 그룹을 선택하십시오. 이 옵션을 통해 관리 클러스터에 있는 각 ESXi 호스트가 Mgmt-Lun0 볼륨에 액세스할 수 있습니다. **Capacity-LUN0**에 대해서도 동일합니다. 

## 4단계: 관리/용량 클러스터의 볼륨에 마운트

vSphere 웹 클라이언트에 로그인하고 **vCenter 인벤토리 목록** 아래의 **호스트**로 이동하십시오.

다음 단계를 사용하여 ESXi 호스트의 볼륨에 마운트하십시오.

1. 호스트를 선택하고 **관리, 스토리지,** > **스토리지 어댑터**를 클릭하십시오.
2. **대상** > **Dynamic Discovery** > **추가…**를 선택하십시오.
3. **대상 서버 추가 전송** 화면에서 스토리지 경로 A의 QuantaStor 스토리지 디바이스에 지정된 IP 주소를 입력하십시오.
4. **3260**을 포트 값으로 두고 **확인**을 클릭하십시오.
5. **Dynamic Discovery** > **추가…**를 클릭하십시오. 스토리지 경로 B에 4단계 및 5단계를 반복하십시오.

ESXI 호스트는 Mgmt-Lun0 볼륨을 발견하기 위해 iSCSI 소프트웨어 어댑터를 다시 스캔할 준비가 되었습니다.

1. **스토리지 어댑터** 화면에서 **스토리지 어댑터 다시 스캔** 아이콘(그림 9)을 선택하십시오.
2. 결과 팝업 화면의 기본 옵션을 그대로 두고 **확인**을 클릭하십시오.
3. 볼륨을 발견한 후 **VMFS-5**로 형식화한 후에 **조치, 새 데이터 저장소**를 클릭하십시오.
4. 형식화가 완료되고 나면 **스토리지 디바이스, 디바이스 세부사항, 다중 경로 지정 편집…**을 클릭하십시오.
5. **경로 선택 정책**에 대해 **라운드 로빈**을 선택하고 **확인**을 클릭하십시오.

이제 Mgmt-Lun0 볼륨이 단일 호스트에 연결되었습니다. 클러스터에 있는 다른 관리 호스트로 돌아가서 볼륨을 추가하는 프로세스를 반복해야 합니다. 그러나 VMFS-5로 이미 형식화되었고, 검색 후에도 그대로 표시되므로 볼륨을 형식화할 필요는 없습니다.

## 5단계: vSphere ESXi 호스트에서 지연된 ACK 사용 안함

스토리지 LUN이 관리 및 용량 호스트에 접속되면 지연된 ACK를 사용 안함으로 설정해야 합니다. 

1. vSphere 환경으로 이동하고 **스토리지 어댑터** > **iSCSI 소프트웨어 어댑터** > **고급 옵션**을 선택하십시오.
2. **편집**을 클릭하고 고급 옵션 화면의 끝까지 스크롤하십시오.
3. **DelayedAck** 선택란을 지우고 **확인**을 클릭하십시오.

VMware에 대하여 지연된 ACK에 대한 자세한 정보는 [VMware’s Knowledge Base](https://kb.vmware.com/s/article/1002598)를 참조하십시오.

이제 [고급 단일 사이트 VMware 참조 아키텍처](/docs/infrastructure/virtualization/advanced-single-site-vmware-reference-architecturesoftlayer.html) 쿡북으로 돌아가서 환경 설정을 완료할 수 있습니다.
