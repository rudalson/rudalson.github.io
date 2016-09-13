---
layout: post
title:  "Linux의 Network 처리 관련 몇가지 질문"
categories: system
tags: linux interrupt nic network study
---
이 , [13.09.16 09:18]
http://d2.naver.com/helloworld/47667

NIC가 패킷을 전송할 때 NIC는 호스트 CPU에 인터럽트(interrupt)를 발생시킨다. 모든 인터럽트에는 인터럽트 번호가 있으며, 운영체제는 이 번호를 이용하여 이 인터럽트를 처리할 수 있는 적합한 드라이버를 찾는다. 드라이버는 인터럽트를 처리할 수 있는 함수(인터럽트 핸들러)를 드라이브가 가동되었을 때 운영체제에 등록해둔다. 운영체제가 핸들러를 호출하고, 핸들러는 전송된 패킷을 운영체제에 반환한다.

"드라이버는 인터럽트를 처리할 수 있는 함수(인터럽트 핸들러)를 드라이브가 가동되었을 때 운영체제에 등록해둔다."

1. 이게 interrupt number가 부팅될때 정해 진다는 얘긴가요?
2. 그럼 hot swap된 device(disk나 nic, cpu 등...)는 인식하는 과정에서 interrupt number가 정해지나요? 
3. 각 device는 고유의 interrupt number가 있고 이걸통해서 cpu랑 communication 하는건가요?
4. interrupt number 한계가 잇나요?
5. 만약 h/w 적으로 soket이 무한대로 있다 치면 이 interrupt number 만큼 device를 달 수 있겟네요?

박, [13.09.16 10:18]
3. 각 device는 고유의 interrupt number가 있고 이걸통해서 cpu랑 communication 하는건가요?
->  yes.
고유 number를 device별로 할당해주는거임. cpu한테 일의 완료를 통지하기 위해 device가 intr.을 assert하면 위의 시퀀스가 수행됨.

4. interrupt number 한계가 잇나요?
-> yes.
intr. 방식은 irq(hw-wired) => msi(message signaled intr.) => msi-x으로 발전해왔고 그 세대에 따라 지원할수 있는 숫자가 16~1024로 늘어남.
또한 한 line만 있는 cpu interrupt line에 APIC(Advanced Programmable Interrupt Controller) chip이 muxing을 해주는 방식으로 연결되어 있는데 이전 8256A chip경우 16개만 지원.(그래서 이전에 더많은 인터럽트를 쓰려면 저 chip을 hierarchical하게 연결함)

5. 만약 h/w 적으로 soket이 무한대로 있다 치면 이 interrupt number 만큼 device를 달 수 있겟네요?
-> maybe.
일단 socket은 sw적으로 nic등의 sw 스택을 추상화해서 부르는말... device인 nic이라고 하는게 문장상 맞음...
제한이 걸리는게 굳이 intr. number만은 아님... pci slot 갯수등에도 제한이 걸림. (버스가 지원하는 장치 갯수)

1. 이게 interrupt number가 부팅될때 정해 진다는 얘긴가요?
-> (좋은질문) 언제 정해지는지 깊이 생각안해봤는데 hw-wired 시절에는 꼽는 위치따라 번호를 배정할테고,
msi/msi-x 시절에는 가용가능한 번호를 동적으로 배정함.

2. 그럼 hot swap된 device(disk나 nic, cpu 등...)는 인식하는 과정에서 interrupt number가 정해지나요?
-> (좋은질문!!!) hot swap/hot plug가 부팅과정에서 행했던 동적할당(msi 경우)을 처리하는것임!!!
(hw wired 시절에는 hot swap/hot plug이 불가능했을듯...)
