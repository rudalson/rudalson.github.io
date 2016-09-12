---
layout: post
title:  "raid 6 volume을 만들고 volume을 쓰기 전 rebuilding 이란"
categories: kernel
tags: kernel study qna
---
이 영주, [12.09.16 10:02]
server의 raid contorller에서 raid 6 volume을 만들고, volume을  쓰기전에 rebuilding 시간이 오래 걸린다고 하는데, 이때의 rebuilding 이란게 뭘 rebuilding 하는것인가요?

박 현식, [12.09.16 10:51]
[다음 유저에게 답장 : 이 영주]
rebuilding은 보통 디스크가 죽었을때 데이터를 살리는 과정을 말하는데...
여기서 말하는 rebuilding은 여러 디스크 사이에서 consistency를 맞추는것... 즉 0으로 다 민다던가, 아님 parity disk를 현재 가비지 값이 들아있는 normal disk 의 데이터를 가지고 인코딩한 값으로 만든다는것.

박 현식, [12.09.16 10:52]
보통은 zero-ing을 해주는것으로 알고 있음...

이 영주, [12.09.16 10:55]
disk 안에 있는 정보를 다 밀고 새로 raid 6 volume 을 만드는데 metadata array... 뭔 얘기를 하더라구요. 그것을 rebuilding 이라고 하면서.
그럼 raid 5,6 volume만들고 난 후 0으로 다 체우기 전까지는 못쓰는건가요?
0으로 궂이 체우는 이유는 뭔가요?

박 현식, [12.09.16 11:05]
metadata array는 무슨 이야긴지 모르겠고...

그럼 raid 5,6 volume만들고 난 후 0으로 다 체우기 전까지는 못쓰는건가요?
-> 사용하려는 스트라이프만 부스트 업해서 빨리 consistency를 맞추고, 사용가능하게 해줄것임...
0으로 궂이 체우는 이유는 뭔가요?
-> 0으로 채우거나 하는 consistency를 맞추는 행동은 disk array를 만든뒤 사용하다가 디스크가 죽었을 경우 rebuilding(내가 첫번째로 말한 개념)을 해야 하는데 array가 consistent한 상황이 아니면 rebuild가 안되기 때문...
(array를 in place update를 가정안하는 log structure시스템으로 구현하거나, 아니면 스트라이프당 valid bit를 두면 consistency를 맞추는 작업이 없어짐 - 최근 hba에 포함된 hw raid 컨틀롤러는 다 valid bit을 사용하는것으로 알고 있음... sw raid인 linux md도 그렇고...)
