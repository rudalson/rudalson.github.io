---
layout: post
title:  "Vim에 YouCompleteMe plugin 사용해보기"
categories: tool
tags: vim youcompleteme plugin study
---
이글은 스터디에서 나온 이야기를 정리한 것임

### 성 , [20.09.16 06:38]

`youcompleteme` 는 vim 에서 사용할 수 있는 자동완성 플러그인 입니다. 지원하는 언어는 C 패밀리(C, C++, Obj-C, Obj-C++) 와, C#, Go, Python, JS, Rust, Typescript 등 상당히 많은 언어의 자동완성을 제공합니다.

이 자동완성은 자동으로 완성됩니다. 자주 쓰는 키워드가 있다면 가장 상단에 나열됩니다. 코드를 개발하는 사람이 아니라 이게 얼마나 유용한지는 잘 모르지만, 그래픽 기반 IDE 에서 쓰는 만큼이나 편하다고 합니다.

[https://valloric.github.io/YouCompleteMe/](https://valloric.github.io/YouCompleteMe/)

설치가 꽤 복잡한 편이라서, 설명서에는 설치 설명만 절반입니다. 배포판마다 설치 시나리오가 조금씩 달라 케이스별로 조금씩 다릅니다.

1. youcompleteme 플러그인을 vim 경로에 복사
2. 각 언어별 컴플리터(completer) 를 컴파일

다행인것은, 우분투에서는 이 플러그인을 이미 컴파일 한 채로 제공하기 때문에, 단순히 다음의 단계만 따르면 됩니다.

1. 플러그인 패키지 설치
sudo apt install vim-youcompleteme
2. Vim Addons Manager 를 통해 설치된 youcompleteme 패키지 활성화

우분투에서 설치되는 vim 플러그인 패키지는 설치하자마자 바로 활성화 되는것이 아닌, `VAM(Vim Addons Manager)` 라는 서드파티 프로그램을 통해서 활성화할 수 있습니다.

다음 명령을 통해, vam 에서 youcompleteme 플러그인의 상황을 확인할 수 있습니다.
{% highlight bash %}
jehos@MacBuntu:~⟫ vam show youcompleteme
Addon: youcompleteme
Status: removed
Description: fast, as-you-type, fuzzy-search code completion engine
Files:
 - autoload/youcompleteme.vim
 - doc/youcompleteme.txt
 - plugin/youcompleteme.vim
{% endhighlight %}
다음의 명령어로 설치된 패키지를 vam 으로 활성화 합니다.

{% highlight bash %}
jehos@MacBuntu:~⟫ vam install youcompleteme
Info: installing removed addon 'youcompleteme' to /home/jehos/.vim
Info: Rebuilding tags since documentation has been modified ...
Processing /home/jehos/.vim/doc/
Info: done.
{% endhighlight %}
다음과 같이 활성화 됩니다.

{% highlight bash %}
jehos@MacBuntu:~⟫ vam show youcompleteme
Addon: youcompleteme
Status: installed
Description: fast, as-you-type, fuzzy-search code completion engine
Files:
 - autoload/youcompleteme.vim
 - doc/youcompleteme.txt
 - plugin/youcompleteme.vim
{% endhighlight %}

실제로 completion 이 동작 하는지 확인해봅니다. C 로 작성된 소스를 열고, 키워드를 입력해보면 자동완성되는 것을 확인하실 수 있습니다.

![플러그인설치완료]({{ site.url }}/assets/2016/photo_2016-09-20_09-30-58.jpg)

설치가 되었고, vim 에서 로드가 되면 이렇게 명령어 모드에서 `Ycm` 으로 시작하는 명령어가 자동완성 됩니다.

사실 이건 아주 기본적인 자동완성만 되는거라 잘 쓴다고 볼 수는 없구요,

* 작성하는 프로젝트마다 전용 설정파일을 디렉토리에 생성해 놓으라고 하네요, 그럼 youcompleteme 플러그인이 동작하면서 해당 설정파일을 읽어내어 참조하는 라이브러리 라던가, 언어 버전(예를들면 C99 인지, C11 인지..) 에 따라 문법을 추측한다던지 그런 동작을 하는것 같습니다.

* 제공하는 자잘한 기능이 엄청 많아요, 예를들면 cscope / ctags 같은데 있는 "정의된 부분 따라가기" 같은 기능들인데, 기본적으로는 기능만 제공되고 단축키 지정같은건 개인이 알아서 해야 합니다. `기능은 제공할테니 사용은 알아서 잘 커스터마이징 해서 하시면 됩니다. 근데 vim 설정할줄은 아시죠??` 같은 느낌...

[https://valloric.github.io/YouCompleteMe/](https://valloric.github.io/YouCompleteMe/)
설치 메뉴얼, 퀵스타트, 설정 옵션 에 대한 설명인데, 다 읽는것도 사실 한바닥입니다.

리눅스 "콘솔" 상에서 직접 코딩하시는 분이거나, vim 을 엄청 커스터마이징 잘 해서 쓰시는분, (혹은 그러는걸 좋아하시는 분) 이 아니라면 vim 과 vim 플러그인에 대해서 시간투자를 좀 하셔야 겠구요, 그게 아니라면 맘편하게 GUI 기반 IDE 를 사용하는게 정신건강에 여러모로 이롭겠습니다ㅠㅠ

#### 요약:

youcompleteme 플러그인은 vim 용 자동완성 플러그인이다. 그러나 설정및 설치가 복잡하고, 코딩에 특화된 기능이 대부분이라, 리눅스 vim(GUI 기반의 gvim 포함)에서 주로 작업하는 "프로그래머" 라면 공부해볼만 하다.

다음은 `ultrasnip` (snipet "코드조각" 들을 관리해주는 vim 플러그인) 과 youcompleteme 의 스크린캐스트(사용법을 영상으로 올리는) 입니다.

8분 정도 되고, 영어설명이 나옵니다만, 그냥 동작 화면만 멍하니 감상하셔도 됩니다. 그럼같이 조작하는군요.. 대단

[Vim screencast #10: Snippets and autocomplete Links: - YouCompleteMe:](https://www.youtube.com/watch?v=WeppptWfV-0)

(이런 플러그인을 이용한다고 갑자기 스킬이 올라간다거나 하는건 아니겠고, 생산성 향상을 위한 도구들이겠지만,  도구를 배우는 것 조차 들여야 할 노력이나 공부해야 할것들이 꽤 있으니, 이런데에 취미나 재미를 붙이지 않으면 새로운 것들을 습득하기도 쉽지는 않을거 같네요...)

근데 이분 왜이렇게 잘하지 싶어서 뒤를 캐봤더니... wincent 라는 아이디를 쓰는, 유명한 vim 플러그인 개발자였네요...

[Githun : wincent (Greg Hurrell)](https://github.com/wincent)

Ultisnip 도 좀 유명한 플러그인인데, snippet 들을 관리해주는 vim 플러그인 입니다. 설명보다 2:36 초 짜리 영상 하나 보시는게 빠르겠네요

[UltiSnips Screencasts Ep 1 - What are snippets](https://www.youtube.com/watch?v=Zik6u0klD40)

참고로 class 서버에 설정해 두었으니, 시도해보세요

* YCM 플러그인 활성화
{% highlight bash %}
vam install youcompleteme
{% endhighlight %}
* Ultisnip 플러그인은 이미 활성화 되어 있음
