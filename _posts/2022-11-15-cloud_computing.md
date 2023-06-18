---
layout: post
title: 클라우드 컴퓨팅(Cloud computing)
tags: [Cloud]
image:
  path: /images/abstract-1.jpg
  feature: abstract-1.jpg
---

이번 글에서는 클라우드 컴퓨팅(Cloud computing)에 대해 알아보겠습니다.

## 클라우드 컴퓨팅이란?

일반적으로 이전의 소프트웨어 판매 방식은 각 가정마다 보급되어 있는 PC에 설치되는 것을 목표로 했습니다. 예를 들면, 마이크로소프트의 경우에는 각 가정의 PC에 자신들의 서비스인 Powerpoint나 Excel를 설치할 수 있는 USB나 CD의 판매량을 늘리는 것이 목표였습니다.

하지만, 이 경우 PC에 설치된 OS(Operating system)에 의존적일 뿐만아니라, 더 나아가 PC의 성능에 따라서도 느려지거나 오류가 나는 등의 한계점이 존재했습니다. 그럼에도 불구하고, 우리는 잘 적응하며 살고 있었습니다.

그러나 빠른 인터넷 속도를 기반으로 클라우드(Cloud)라는 개념이 등장하면서, 이제 서비스를 별도의 설치 없이 온라인으로 제공할 수 있게 되었습니다.

> "클라우드 컴퓨팅은 컴퓨팅 리소스를 인터넷을 통해 서비스로 사용할 수 있는 주문형 서비스입니다. 기업에서 직접 리소스를 조달하거나 구성, 관리할 필요가 없으며 사용한 만큼만 비용을 지불하면 됩니다." - 구글 클라우드(https://cloud.google.com/)

즉, 이제 내 컴퓨터가 인터넷만 접속할 수 있다면, 기존 마이크로소프트의 Powerpoint 등 모든 서비스를 실시간으로 구동 가능해진 것입니다. 

물론 해당 서비스를 이용할 때에는 내가 모르는 어딘가의 다른 컴퓨터 (혹은 서버)에서 서비스가 실행되며 CPU와 하드디스크를 사용하고 있겠지만, 사용하는 입장에서 "어디서 돌아가는지"는 이제 속도만 유지된다면 더이상 중요하지 않게 되게 되었습니다.

## As-a-service (*aas)

다시 말하면, 클라우드 개념의 도입으로 서비스의 제공 범위가 확장되게 된 것입니다.

즉, 기존의 소프트웨어 회사들은 자신의 상품의 어디까지를 서비스(_As a service_)로 제공할지 고민하게 되었습니다.

아래 그림처럼, 

1. OS가 구동될 수 있는 물리적 공간 및 관리 기술까지는 회사에서 제공하고 OS부터 사용자가 결정할 수 있게하는 Infrastructure-as-a-Service (IaaS)

2. OS를 포함한 컴퓨팅 자체를 회사에서 제공하고, 데이터와 Application을 사용자가 설치하거나 제어할 수 있는 Platform-as-a-Service (PaaS)

3. Application을 별도의 설치 없이 사용할 수 있도록 회사에서 모두 제공하는 Software-as-a-Service (SaaS)

등 다양한 개념이 도출되었습니다.

![https://www.redhat.com/cms/managed-files/iaas-paas-saas-diagram4-1638x1046_0.png](https://www.redhat.com/cms/managed-files/iaas-paas-saas-diagram4-1638x1046_0.png){:.center-image}

> 출처: https://www.redhat.com/ko/topics/cloud-computing/what-is-saas

서비스가 다양화됨에따라, 사용자는 이제 자신의 환경을 고려하여 더 최적화된 경험을 할 수 있게 된 것입니다.

대표적으로, 아마존 AWS의 서버 제공 등의 기능는 IaaS이며, 업무용 커뮤니케이션 Application인 슬랙(Slack)이나 파일 관리 Application인 드롭박스(Dropbox) 등은 SaaS입니다.

## 도커, 쿠버네티스

이런 클라우드 컴퓨팅, *aaS의 핵심 기술을 꼽자면 바로 위 그림에서 빨간색(=서비스 제공자가 관리하는) 영역에 공통적으로 들어가는 __가상화(Virtualization)__입니다.

![https://image.samsungsds.com/kr/insights/__icsFiles/afieldfile/2022/02/21/2.jpg?queryString=20221104035409](https://image.samsungsds.com/kr/insights/__icsFiles/afieldfile/2022/02/21/2.jpg?queryString=20221104035409)

> 출처: [삼성 SDS: 쿠버네티스 알아보기 1편: 쿠버네티스와 컨테이너, 도커에 대한 기본 개념](https://www.samsungsds.com/kr/insights/220222_kubernetes1.html)

기존에는 Traditional Deployment처럼 PC의 Hardware를 기반으로 Windows와 같은 OS를 설치한 후, 그 위에 모든 Application을 설치했습니다. Powerpoint, 게임, 동영상 편집기 등등. 하지만, 문제는 많은 Application이 한 OS에 설치되어 서로 충돌이 나거나 성능 저하를 발생시켰습니다.

이에, 가상화 기능이 등장했습니다. 가상화란 쉽게 말하면, 서로 충돌이 나는 등의 문제가 발생하지 않게 OS 상에서 Application들을 다 한꺼번에 설치하는 게 아니라, OS 안에 작은 OS(=Vitual Machine 안에 OS)를 설치하여 Application을 따로 실행하는 것입니다. 그러나, 가상화의 경우 여러 Virtual Machine을 사용할 경우 다수의 OS를 사용하기 때문에 하나의 OS를 사용하는 것보다 무겁게 됩니다.

따라서, OS는 공유하되 Application을 따로 구동할 수 있는 컨테이너(Container)라는 개념을 도입하게 되었습니다. 

![https://image.samsungsds.com/kr/insights/__icsFiles/afieldfile/2022/02/21/1.jpg?queryString=20221104035409](https://image.samsungsds.com/kr/insights/__icsFiles/afieldfile/2022/02/21/1.jpg?queryString=20221104035409)

> 출처: [삼성 SDS: 쿠버네티스 알아보기 1편: 쿠버네티스와 컨테이너, 도커에 대한 기본 개념](https://www.samsungsds.com/kr/insights/220222_kubernetes1.html)

즉, 컨테이너는 Application이 격리되어 실행되는 공간을 의미하게 됩니다.

이러한 컨테이너를 사용고 관리하는 도구가 컨테이너 런타임이며, 대표적으로 도커(Docker)가 있겠습니다. 도커를 활용하며면 컨테이너를 생성하고 백업하거나, 웹 상의 다른 컨테이너를 내려받고 공유할 수 있습니다.

더 나아가, 컨테이너들 간의 자원을 조율하고 주기적으로 업데이트해주는 등 오케스트레이션하는 것을 쿠버네티스(Kubernetes)라고 부르게 됩니다. k3s가 대표적인 쿠버네티스이며, 이 외에도 클라우드 네이티브 컴퓨팅 재단(Cloud Native Computing Foundation)의 인증을 받은 다양한 쿠버네티스가 존재합니다.

* **서비스 디스커버리와 로드 밸런싱:** 쿠버네티스는 DNS 이름을 사용하거나 자체 IP 주소를 사용하여 컨테이너를 노출할 수 있다. 컨테이너에 대한 트래픽이 많으면, 쿠버네티스는 네트워크 트래픽을 로드밸런싱하고 배포하여 배포가 안정적으로 이루어질 수 있다.
스토리지 오케스트레이션 쿠버네티스를 사용하면 로컬 저장소, 공용 클라우드 공급자 등과 같이 원하는 저장소 시스템을 자동으로 탑재 할 수 있다.

* **자동화된 롤아웃과 롤백:** 쿠버네티스를 사용하여 배포된 컨테이너의 원하는 상태를 서술할 수 있으며 현재 상태를 원하는 상태로 설정한 속도에 따라 변경할 수 있다. 예를 들어 쿠버네티스를 자동화해서 배포용 새 컨테이너를 만들고, 기존 컨테이너를 제거하고, 모든 리소스를 새 컨테이너에 적용할 수 있다.

* **자동화된 빈 패킹(bin packing):** 컨테이너화된 작업을 실행하는데 사용할 수 있는 쿠버네티스 클러스터 노드를 제공한다. 각 컨테이너가 필요로 하는 CPU와 메모리(RAM)를 쿠버네티스에게 지시한다. 쿠버네티스는 컨테이너를 노드에 맞추어서 리소스를 가장 잘 사용할 수 있도록 해준다.

* **자동화된 복구(self-healing):** 쿠버네티스는 실패한 컨테이너를 다시 시작하고, 컨테이너를 교체하며, '사용자 정의 상태 검사'에 응답하지 않는 컨테이너를 죽이고, 서비스 준비가 끝날 때까지 그러한 과정을 클라이언트에 보여주지 않는다.

* **시크릿과 구성 관리:** 쿠버네티스를 사용하면 암호, OAuth 토큰 및 SSH 키와 같은 중요한 정보를 저장하고 관리 할 수 있다. 컨테이너 이미지를 재구성하지 않고 스택 구성에 시크릿을 노출하지 않고도 시크릿 및 애플리케이션 구성을 배포 및 업데이트 할 수 있다.

> 출처: [NHN Cloud: NHN Kubernetes Service(NKS) 활용법](https://doc.skill.or.kr/nhn-cloud)

아래를 참고하면 더 자세한 내용을 알아볼 수 있습니다.

> [삼성 SDS: 쿠버네티스 알아보기 1편: 쿠버네티스와 컨테이너, 도커에 대한 기본 개념](https://www.samsungsds.com/kr/insights/220222_kubernetes1.html)

> [삼성 SDS: 쿠버네티스 알아보기 2편: 쿠버네티스를 만나는 여러 가지 방법](https://www.samsungsds.com/kr/insights/kubernetes-2.html)

> [삼성 SDS: 쿠버네티스 알아보기 3편: 쿠버네티스를 이루고 있는 여러 가지 구성 요소](https://www.samsungsds.com/kr/insights/kubernetes-3.html)

> [NHN Cloud: NHN Kubernetes Service(NKS) 활용법](https://doc.skill.or.kr/nhn-cloud)
