---
layout: post
title:  "Ceph Day Seoul 후기(2016)"
categories: etc
tags: ceph conference sk
---
2016.08.26 [Ceph Day Seoul](http://ceph.com/cephdays/ceph-day-apac-roadshow-seoul/) 에 참가하였다. Ceph에 대해 관심은 많았지만 잘 알지는 못했기에 후기의 수준이 높지는 않겠지만 최대한 성의껏 듣고 기록을 남겨본다.

# AFCeph: Problem, Progress, and Plan
Byung-Su Park / Myoungwon Oh, SK telecom

All flash Storage를 하게 된 연유와 그 이유로 나온 sk all-flash ceph storage 소개.
하드웨어 적으로 더 빠른 녀석이 필요하다.. 그리고 지속적으로 Ceph 커뮤니티에 협력하면서 자체 스토리지의 optimizing작업을 진행해 옴

Journal 기능이 전체 퍼포먼스에 영향을 준다고 하기에 그 부분은 nvram으로 꽂아 넣음
비싼걸 마구마구 집어 넣어서 빠르기는 하겠다.


# Ceph on Storage appliance - Case Study and Performance for S3 object storage
Yong-Hun Jung, Fujitsu

후지쯔가 Ceph 활동을 얼마나 열심히 하는지 설명하면서 CD10000 이라는 제품 홍보
All-flash는 아니지만 Ceph에 특화된 스토리지 하드웨이이며 OwnCloud 와 연동하여 얼마나 잘 사용할 수 있는지 소개
후지쯔는 주로 disk bottle-neck 관련 개선에 기여
Hammer 버전 사용(현재는 Jewel 버전 나온 상태)

# Ceph on All-Flash Storage – Breaking Performance Barriers
Haf Saba, Director, Sales Engineering, APJ, SanDisk

한국이 흥미롭다고 함. 비메모리 세계 1, 2위 그리고 nand쪽에서도 세계 탑급 회사가 2개나 존재한다라고 한국을 마구 추켜세우면서 SanDisk꺼 좋다고 홍보.
SDS 전반적인 개념부터 소개하면서 자사의 InfiniFlash 라는 제품 소개. SK 제품처럼 올플래시이며 하드웨어적으로는 겹침.
다만 Ceph뿐만 아니라 OpenStack 같은 오픈소스들에 잘 대응되도록 한 것같음. Open Source community에 얼마나 많은 기여를 하는지 소개
들은 얘기로는 기술적으로는 엄청 좋은게 맞는데 크게 히트치지 못했다고함.
Ceph, OpenStack과 일찍부터 협업을 해왔으며 차세대 SDS 시장에 대해 잘 준비된 모습.
Code Erasure 사용
Flash is on par or cheaper than buying HDDs
그 이상의 기술적인 설명은 모르겠음.

# Ceph on ARM
Jeff Chu, ARM

영어가 너무 빠른데 대충 ARM 서버 자체가 얼마나 share가 늘어나는지. 분산환경에서 ARM쪽의 서버시장에 대한 우월함(question)에 대한 이야기가 많이 거론되는데 정확한 내용은 이해가 안감

# The Anatomoy of Ceph IO
Sang-Hoon Kim, Dong-Yun Lee, Sanghoon Han, Kisik Jeong, and Jin-Soo Kim / 성균관대

Ceph의 Write traffic에 주목
Read는 캐시를 탈 때가 많아서 순수한 IO에 대한 Ceph 퍼포먼스 측정에 변수도 많다.
실 사용 중에는 Read보다 Write에 대한 비중이 높다
RBD 부분 살펴 봄


# Delivering cost-effective, high performance Ceph cluster with Today’s NVM Express* Solid State Drives and Tomorrow’s Intel® Optane™ technology
Jack Zhang, Intel

화장실 가느라 거의 못들음. 인텔의 차세대 ram(?) 기술인 3D Xpoint 라는게 있는 것 같은데 이게 스토리지와 엮이면서 엄청난 속도를 낸다는 이야기를 한 것 같음.
인텔도 Ceph 기여 많이 함.
영어가 너무 빠름.

# Ceph: a decade in the making and still going strong
Patrick McGarry, Red Hat

계획에는 없는 세션 같은데 시간이 비는지 Ceph 커뮤니티의 리더인 Patrick이 직접 소개 해줌.

Ceph 창시자인 Sage Weil은 InkTank 라는 회사를 세운것 같음. Sage
Open Stack의 급성장을 등에 업고 잘나간다.

주요 기술
RADOS – distributed object storage cluster (2005)
EBOFS – local object storage (2004/2006)
CRUSH – hashing for the real world (2005)
Paxos monitors – cluster consensus (2006)

fuse를 사용하지 않고 kernel 단에 직접 구현
초반에는 Torvalds에 kernel merge 거부당하는데 결국엔 v2.6.34(2010 초) 에 kernel 메인라인에 포함

# Bring Ceph to Enterprise - Setup a 100T mobile cluster in 30 min
Chris Chon, SUSE

Suse Korea의 전철민 과장. 외국인이 아니었음. 다행.
iscsi를 통해 Ceph Block Device 접근하는 과정에서 몇몇 개선을 한 Suse Enterprise 기능을 소개함
Suse Enterprise는 유료이나 Leap 버전은 무료고 거의 비슷하다고 이것부터 써볼 것을 권유 (Rhel과 Cent 같은 관계?)
그리고 상용 스토리지의 통합 관리 시스템인 오픈소스 OpenATTIC 소개
NAS, SAN 등의 기존 스토리지 관리 및 Ceph 관리 및 모니터링 기능.
