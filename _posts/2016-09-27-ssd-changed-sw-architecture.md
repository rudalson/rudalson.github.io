---
layout: post
title:  "SSD가 가져온 SW 아키텍쳐 변화에 관해"
categories: system
tags: ssd study
---

디스크 마운트 할때 fstab 에 기록하잖아요? 거기서 ssd 같은 애들은 디스크 스케쥴러를 아예 없는걸로 설정해야 한다고 하더라구요. 기존에 스케쥴러는 전통적인 방식의 하드디스크를 염두에 두고 만든 것들이라, ssd 에서는 의미가 없다고. 파일을 완전 소거해주는 프로그램도 ssd 와서는 큰 의미가 없어진것 같고요

[Low-Latency IO-Scheduler](https://wiki.debian.org/SSDOptimization#Low-Latency_IO-Scheduler)

ssd 를 위한 시스템 최적화 방안 목록인데요 여기서 디스크 스케쥴러를 noop 으로 선택하라는 부분이 있습니다

`The default I/O scheduler queues data to minimize seeks on HDDs, which is not necessary for SSDs. Thus, use the "deadline" scheduler that just ensures bulk transactions won't slow down small transactions:`

|기본 I/O 스케쥴러는 하드디스크의 데이터 탐색 속도를 최소화 하기 위한 것인데, SSD 에선 필요없다. 따라서, "deadline" 스케쥴러를 사용하는 것은 대량의 트랜잭션에서 작은 단위의 트랜잭션의 속도가 줄어들지 않음을 의미한다.|

https://wiki.debian.org/SSDOptimization#A.2Fetc.2Ffstab_example
{% highlight bash %}
# /etc/fstab: static file system information.
#
# Use 'vol_id --uuid' to print the universally unique identifier for a
# device; this may be used with UUID= as a more robust way to name devices
# that works even if disks are added and removed. See fstab(5).
#
# <file system> <mount point>   <type>  <options>       <dump>  <pass>
### SSD: discard,noatime
### match battery operation default for commit JOURNAL_COMMIT_TIME_AC in Add files in /etc/pm/config.d/*
/dev/mapper/goofy-root /               ext4    discard,noatime,commit=600,errors=remount-ro 0       1
# /boot was on /dev/sda1 during installation
UUID=709cbe4a-80c1-46cb-8bb1-dbce3059d1f7 /boot           ext4    discard,noatime,commit=600,defaults        0       2
### SSD: discard
/dev/mapper/goofy-swap none            swap    sw,discard              0       0
/dev/mapper/goofy-chroot /srv/chroot         btrfs    ssd,discard,noatime 0       2
/dev/scd0       /media/cdrom0   udf,iso9660 user,noauto     0       0
{% endhighlight %}
이것은 fstab 의 예시입니다. 여기 보시면 ssd,discard,noatime 가 있는데요.

1. discard 는 trim 기능 활성화
2. ssd 는 스케쥴러 끔 의 다른 이름으로 알고 있습니다.
3. noatime 은 access time 을 파일에 일일이 기록하지 않음으로써 속도 향상을 꾀하는것이고.

그리고 이거땜에 btrfs 같은 파일시스템이 갑자기 빛을 봤다고 들었어요. 정확히는 몰라서 좀 얼버무려야 할거같은데ㅋㅋ 이 파일시스템에서 쓰는 자료구조 형태가 이리저리 막 참조하는 형태라서, (즉 지역 연속성이 없어서) 일반 하드디스크에서 쓰면 굉장히 비효율적이라고 하더군요.

cow 같은게 그렇습니다.

[Is Btrfs optimized for SSD?](https://btrfs.wiki.kernel.org/index.php/FAQ#Is_Btrfs_optimized_for_SSD.3F)

Btrfs 는 SSD 에 최적화 되어 있나요?

There are some optimizations for SSD drives, and you can enable them by mounting with -o ssd. As of 2.6.31-rc1, this mount option will be enabled if Btrfs is able to detect non-rotating storage. SSD is going to be a big part of future storage, and the Btrfs developers plan on tuning for it heavily. Note that -o ssd will not enable TRIM/discard.

SSD를 위한 최적화가 존재합니다. 커널 2.6.31-rc1 이후 버전에서 마운트 할때 옵션으로 ssd 를 붙이면 됩니다. 이 옵션은 Btrfs가 비 회전식 디스크를 탐지할 수 있도록 해줍니다. SSD 는 미래의 저장장치이기 때문에, Btrfs 개발자들도 이에 대한 준비를 하고 있습니다. 단, ssd 옵션을 켠다고 해서 자동으로 trim/discard 기능이 활성화되지는 않습니다

어떤 부분이 왜 좋아졌다에 대한 언급은 안나와 있네요, 이 부분은 그냥 무시해 주십시요~

cow 를 예로 들면서 btrfs 에서 사용하는 btree 알고리즘이 왜 ssd 상에서 유용한가를 누가 설명했던것 같은데

[B-Tree 성능 그리고 Copy On Write B-Tree](http://d2.naver.com/helloworld/162498)

여기서부터 주장하는 내용은 제가 증명할수가 없네요... `BTRFS 파일시스템이 사용하는 자료구조는 전통적 방식의 하드디스크에서 비효율적이다.` 를 주장하고 싶었는데, 그 주장을 뒷받침할만한 벤치마크나 원전을 찾지를 못하겠습니다

찾다보니 이런 내용도 있군요,

[ 파일 : IBM Research Report - BTRFS,  The Linux B-tree Filesystem.pdf ]

p31, 6.2 Flash disk

In order to improve performance, the number of reads had to be reduced. The core problem turned out to be the way the Linux virtual memory (VM) system  was  used.   The  VM  assumes  that  allocated  pages  will  be  used  for a while.  BTRFS, however, uses many temporary pages due to its copy-on-write nature, where data can move around on the disk.  This mismatch was causing the VM to hold many out of date pages, reducing cache e ectiveness

성능을 향상시키기 위해서는 읽기 횟수를 줄여야 한다. 가장 큰 문제는 리눅스 가상 메모리를 사용하는 방식 인데, 가상 메모리는 할당된 페이지를 일정 동안 사용한다. BTRFS 에서는 데이터가 디스크상 어디라도 움직일 수 있는 COW 의 특성 때문에 너무나 많은 임시 페이지들을 할당하게 된다. 이것으로 인해 가상 메모리는 오래된 페이지들로 인해 불일치가 발생하고, 캐쉬 효율이 떨어지게 된다.

Btrfs 에서 ssd 를 사용할때 성능 측정을 한 것인데, 일반적인 4K 단위 블록을 쓸 때에는 성능이 오르락 내리락 한 걸 말하고 있습니다.

This in turn, caused additional paging in the free space allocation tree, which was thrown out of cache due to cache pressure. The fix was for BTRFS to try
harder to discard from the VM pages that have become invalid.

이후, 캐쉬 부족으로 인해 자유 공간 할당 트리에서 추가적인 페이징 요청이 발생한다. BTRFS 는 가상 메모리 페이지에서 불일치된 페이지들을 떨궈내며 이를 수정하려고 시도한다.

![Performance  in  the  kernel  3.3.   File  creation  rate  is  unsteady, throughput is a series of peaks and valleys]({{ site.url }}/assets/2016/photo_2016-09-27_19-17-22.jpg)
Figure  21:  Performance  in  the  kernel  3.3.   File  creation  rate  is  unsteady, throughput is a series of peaks and valleys.

리눅스 커널 3.3 에서는 상기 언급한 문제때문에 성능이 이렇게 널뛰기를 하는데요

![Performance in the kernel 3.4, with 4KB metadata pages]({{ site.url }}/assets/2016/photo_2016-09-27_19-20-46.jpg)
Figure 22:  Performance in the kernel 3.4, with 4KB metadata pages.

리눅스 커널 3.4 에서는 접근 횟수를 최소화 하여 동일한 4K 블록에서도 균일한 성능을 낼 수 있게 해주었다고 하네요

리눅스 커널 3.4 에서 btrfs 에 대해 대체 뭘 바꿨더니 저렇게 향상되었는지 궁금해서 찾아봤습니다.

https://btrfs.wiki.kernel.org/index.php/Changelog#v3.4_.28May_2012.29

성 제호, [27.09.16 09:36]
Reworked the way in which metadata interacts with the page cache. page->private now points to the btrfs extent_buffer object, which makes everything faster. The code was changed so it now writes a whole extent buffer at a time instead of allowing individual pages to go down. It is now more aggressive about dropping pages for metadata blocks that were freed due to COW. Overall, metadata caching is much faster now. (Josef Bacik)

메타데이터가 페이지 캐쉬와 상호작용 하는 방식을 재작업 하였다. page->private 는 이제 btrfs 의 extent_buffer 객체를 바라보게 되며, 이로인해 성능이 향상되었다. 코드는 개별 페이지를 내려보내는 대신에, extent_buffer 전체를 쓰는 식으로 변경되었다. 이 조치는 COW 도중에 해제된 메타데이터 블록을 조금 더 공격적으로 떨궈낼 수 있게 되었다. 전체적으로 메타데이터 캐싱은 더 빨라지게 되었다. (Josef Bacik)

성 제호, [27.09.16 09:37]
분명 이거 lwn.net 에 기고되어있을거라는데 오백원 겁니다

성 제호, [27.09.16 09:37]
찾아봐야지

성 제호, [27.09.16 09:40]
[다음 유저에게 답장 : 성 제호]
꽥.. LWN 에는 없고, 이 논문에 언급된게 원전이군요..ㅠㅠ  짤막한 저널로 낼줄 알았더니..

성 제호, [27.09.16 09:43]
실제 커널에 반영된 커밋을 찾아봤는데, 아마 이걸로 추정? 됩니다..

https://git.kernel.org/cgit/linux/kernel/git/stable/linux-stable.git/commit/?id=4f2de97acee6532b36dd6e995b858343771ad126

성 제호, [27.09.16 09:43]
Josef Bacik 이란 분이 원래는 레뎃에 있다가 지금은 페북으로 가셨군요. 모든것을 흡수해버리는 페북..ㄷㄷ

없다캐라, [27.09.16 09:44]
이곳 commit가 맞는것 같아요.

성 제호, [27.09.16 09:45]
끝까지 쫓아가 보다가 소스코드를 만나면 콘크리트 벽을 만나는 느낌입니다..

성 제호, [27.09.16 09:45]
영어를 해석하거나, 저널을 찾거나, 책을 찾거나.. 는 의지만 있으면 어떻게든 하겠는데 소스는...

성 제호, [27.09.16 09:46]
암튼 패치 하나로 성능이 이렇게 좋아질 수 있는걸 보니 뭔가 대단하네요... 신기합니다

성 제호, [27.09.16 09:46]
그러니까 업데이트는 철저히..! (응?)

없다캐라, [27.09.16 09:47]
난 저 과정이 더 신기함 ㅋㅋㅋ

성 제호, [27.09.16 09:47]
인제 얌전히 현식님을 기다리겠습니다

성 제호, [27.09.16 09:48]
ㅠㅠ

없다캐라, [27.09.16 09:50]
여튼 대단한 탐구였습니다. 제호사마 덕에 좀더 제대로 알 수 있는 계기가 된것 같네요

성 제호, [27.09.16 09:52]
근데 해석만한거라서 사실 무슨뜻인지 모르겠어요, VFS 의 메타데이터와 페이지캐시가 어떻게 움직이는지, 가상 메모리는 어덯게 동작하는지 알아야 해석한 말을 이해할 수 있을것같스빈다

성 제호, [27.09.16 09:52]
그냥 해석만 하고 만거라.. 소화를 못시키겠네요..

성 제호, [27.09.16 09:52]
소화불량..ㅠ

성 제호, [27.09.16 09:52]
그냥 업데이트를 하니가 성능이 좋아지네!? 를 교훈으로 업데이트를 열심히 하자는 교훈으로 삼겠습니다

성 제호, [27.09.16 09:55]
[다음 유저에게 답장 : 성 제호]
이제 다시 원래 얘기로 돌아가자면, 이 분이 아티클을 쓰셨을 2012 년 당시에는 확실히 이문제가 있었습니다. CoW 에서의 공간낭비 문제 말이죠... 그런데 커널 패치가 실제로 일어난 해도 2012년 입니다. 아마 바닐라 커널이 적용되고, 실제 배포판까지 전달될때까지는 보통 1년 정도 걸리니까, 비슷한 시점에 문제가 해결된것이 아닌가, 하고 생각해봅니다

성 제호, [27.09.16 09:56]
오래전부터 개발팀도 이 문제를 인식하고 있어서(성능이 불균일하게 나옴) 개발하다가, 2012년에 바닐라 커널의 메인라인에 패치가 추가되었고,

이 아티클을 작성하신 박사님(?) 도 이 문제를 그때 제기하신듯... 한게 아닌가 하고 추측해봅니다.

(근데 두 문제가 겹치는것인지는 확실히 모르겠네요. 비슷한 문제로 보이긴하는데...)
