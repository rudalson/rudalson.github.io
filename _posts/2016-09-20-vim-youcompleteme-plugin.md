---
layout: post
title:  "Vim에 YouCompleteMe plugin 사용해보기"
categories: tool
tags: vim youcompleteme plugin study
---
이글은 스터디에서 나온 이야기를 정리한 것임

### 성 , [20.09.16 06:38]

[youcompleteme](https://github.com/Valloric/YouCompleteMe) 는 vim 에서 사용할 수 있는 자동완성 플러그인 입니다. 지원하는 언어는 C 패밀리(C, C++, Obj-C, Obj-C++) 와, C#, Go, Python, JS, Rust, Typescript 등 상당히 많은 언어의 자동완성을 제공합니다.

자주 쓰는 키워드가 있다면 가장 상단에 나열됩니다. 코드를 개발하는 사람이 아니라 이게 얼마나 유용한지는 잘 모르지만, 그래픽 기반 IDE 에서 쓰는 만큼이나 편하다고 합니다. 플러그인 이름이 예쁜데요, 참 낯간지러운 말이지만, 목적과 어울리는 이름을 잘 지었다는 생각이 듭니다.

설치가 꽤 복잡한 편이라서, 설명서에는 설치 설명만 절반입니다. 배포판마다 설치 시나리오가 조금씩 달라 케이스별로 조금씩 다릅니다. 요구하는 것도 참 많은데요, 자세한건 제작자의 공식 홈페이지의 메뉴얼을 참조하시는게 좋겠습니다. 

[공식 홈페이지의 메뉴얼]( https://valloric.github.io/YouCompleteMe )

(요구사항이 다 맞춰졌다는 가정 하에) 간단하게 정리하면 다음과 같은 흐름입니다.

1. youcompleteme 플러그인을 vim 경로에 복사
2. 각 언어별 컴플리터(completer) 를 컴파일

다행인것은, 우분투 에서는 플러그인을 이미 컴파일 한 채로 제공하기 때문에, 단순히 다음의 단계만 따르면 됩니다.

1. 플러그인 패키지 설치
`sudo apt install vim-youcompleteme`
2. Vim Addons Manager 를 통해 설치된 youcompleteme 패키지 활성화
`vam install youcompleteme`

우분투에서 설치되는 vim 플러그인 패키지는 설치하자마자 바로 활성화 되는것이 아닌, `VAM(Vim Addons Manager)` 라는 vim 플러그인 관리 프로그램을 통해서 활성화할 수 있습니다. (vundle 이나 plug 를 쓰시는 분들은 마찬가지로 제작자 홈페이지를 참조해주세요)

원래 바닥부터 설치한다면 각 언어별 컴플리터를 따로 컴파일 해야하는데, 우분투에서 제공하는 패키지로 설치하셨다면 이 과정은 패스해도 됩니다. 몇가지 의존성이 걸린 패키지가 함께 설치되는데,  미리 컴파일된 컴플리터가 함께 설치되기 때문입니다. 내용이 궁금하신 분들은 `ycmd` 패키지의 내용을 잘 살펴보시면 됩니다. `ycmd` 는 `youcompleteme` 플러그인과 통신하는 서버 역할을 합니다. vim 의 YCM 플러그인은 사실 thin client 정도의 역할밖에 하지 않습니다.


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

실제로 completion 이 동작 하는지 확인해봅니다. C 로 작성된 소스를 열고, 키워드를 입력해보면 자동완성되는 것을 확인하실 수 있습니다. (이때, 파일명을 반드시 `.c` 로 해주십시요. YCM 은 현재 파일에 적합한 문법등을 추측하는데, 그 중 하나가 확장자이기 때문입니다.)

![동작이미지](https://camo.githubusercontent.com/1f3f922431d5363224b20e99467ff28b04e810e2/687474703a2f2f692e696d6775722e636f6d2f304f50346f6f642e676966)

설치가 되었고, vim 에서 로드가 되면 이렇게 명령어 모드에서 `Ycm` 으로 시작하는 명령어가 자동완성 됩니다.

![플러그인설치완료]({{ site.url }}/assets/2016/photo_2016-09-20_09-30-58.jpg)

사실 이건 아주 기본적인 자동완성만 되는거라 잘 쓴다고 볼 수는 없습니다. 제대로 사용하겠다고 마음 먹으셨다면, [일반적인 사용 예](https://valloric.github.io/YouCompleteMe/#general-usage) 정도는 읽고 넘어가시기를 바랍니다. (영어가 짧아 모두 소개를 못해드려 죄송합니다)

큰 특징을 몇개 적자면 다음과 같습니다.

* 너무 많은 수의 자동완성이 잡힌다면, 계속 입력해보세요, 입력값을 기준으로 자동완성 범위가 점점 좁혀집니다
* 똑똑한 대소문자 구분을 합니다, 만약 소문자로만 입력하면 `대소문자 구분` 모드로 들어갑니다. 그러나 입력값중에 대문자가 들어가면 소문자도 함께 잡아냅니다. (예를들어, "foo" 라는 문자는 "foo", "Foo", "fOO" 같은 문자로도 잡아낼 수 있습니다.)
* 선택은 `Tab` 과 `Shift+Tab` 으로 합니다. 키는 재지정 할 수 있습니다.
* YCM 은 여러개의 자동완성 엔진을 가지고 있습니다. 식별자 기반의 자동완성은 현재 파일과 방문한 다른 파일, 그리고 tags 파일을 기준으로, 파일 타입 별로 구분된 식별자를 찾아냅니다.
* 여러개의 의미(Semantic) 기반의 엔진도 있습니다. `libclang` 기반의 컴플리터는 C 언어군에 대해 의미 기반 자동완성 기능을 제공합니다. `Jedi` 기반 컴플리터는 파이선에 대한 의미 기반 자동완성을 제공합니다. 또한, `Omnifunc` 기반의 컴플리터는 특정 언어에 대한 컴플리터가 없을 경우 사용됩니다.
* 코드 조각(snippet) 관리 도구인 `UltiSnips` 나, 파일 경로 자동완성 기능을 함께 사용할 수 있습니다.

오역의 위험이 있으므로, 정확한 내용은 상기의 홈페이지를 반드시 참조해 주십시요.

* 작성하는 프로젝트에 전용 설정파일(`.ycm_extra_conf.py`)을 생성하여, 프로젝트마다 설정을 따로 가져갈 수 있습니다.  `youcompleteme` 플러그인이 동작하면서 프로젝트 디렉토리 내의 설정파일을 읽어내어 참조하는 라이브러리 라던가, 언어 버전(예를들면 C99 인지, C11 인지..) 에 따라 자동완성의 행동이나 범위가 달라집니다. 프로젝트 특정의 설정이 없는 경우에는 `ycm_extra_conf.py` 의 global 설정이 사용되며, `이 파일을 복사하여 프로젝트에 맞게 변환하여 사용할 수 있습니다.

* [YcmCompleter Subcommands](https://valloric.github.io/YouCompleteMe/#ycmcompleter-subcommands) 을 참조하시면, 소스탐색에 관련된 기능을 상당히 많이 제공합니다. 예를들면 소스의 "정의된 부분 따라가기" 같은 기능들인데, 기본적으로는 기능만 제공되고 단축키 지정같은건 개인이 알아서 해야 합니다. `기능은 제공할테니 사용은 알아서 잘 커스터마이징 해서 하시면 됩니다. 근데 vim 설정할줄은 아시죠??` 같은 느낌으로...

설치 메뉴얼, 퀵스타트, 설정 옵션 에 대한 설명인데, 다 읽는것도 사실 한바닥입니다. 리눅스 상에서 직접 코딩하시는 분이거나, vim 을 엄청 커스터마이징 잘 해서 쓰시는분, (혹은 그러는걸 좋아하시는 분) 이 아니라면 먼저 vim 과 vim 플러그인에 대해서 시간투자를 좀 하셔야 겠구요, 그게 아니라면 맘편하게 GUI 기반 IDE 를 사용하는게 정신건강에 여러모로 이롭겠습니다ㅠㅠ

#### 요약:

1. youcompleteme 플러그인은 vim 용 자동완성 플러그인이다. 
2. 그러나 설정및 설치가 복잡하고, 코딩에 특화된 기능이 대부분이라, 
3. 리눅스 vim(GUI 기반의 gvim 을 포함하여)에서 주로 작업하는 "의욕있는 프로그래머" 라면 공부해볼만 하다.


#### 스크린캐스트의 응용

기술의 사용 예를 동영상으로 편집하여 올리는 스크린캐스트가 많은데, vim 같은 편집기는 직접 한번 보는것의 효용이 크므로 스크린캐스트가 상당히 흥합니다. 유튜브에서 단순히 vim screencast 라고 검색하셔도 상당히 많은 영상이 나옵니다. 어떻게 쓰는지 감이 안온다면, 영상으로 한번 보시면 이해가 빠릅니다.

다음은 `Ultrasnip` (snipet "코드조각" 들을 관리해주는 vim 플러그인) 과 youcompleteme 의 스크린캐스트 입니다. 8분 정도 되고, 영어설명이 나옵니다만, 그냥 동작 화면만 멍하니 감상하셔도 됩니다. (그림같이 조작하는군요.. 대단)

[Vim screencast #10: Snippets and autocomplete Links: - YouCompleteMe:](https://www.youtube.com/watch?v=WeppptWfV-0)

(이런 플러그인을 이용한다고 갑자기 스킬이 올라간다거나 하는건 아니겠고, 생산성 향상을 위한 도구들이겠지만,  도구를 배우는 것 조차 들여야 할 노력이나 공부해야 할것들이 꽤 있으니, 이런데에 취미나 재미를 붙이지 않으면 새로운 것들을 습득하기도 쉽지는 않을거 같네요...)

Ultisnip 은 snippet 들을 관리해주는 vim 플러그인 입니다. 설명보다 2:36 초 짜리 영상 하나 보시는게 빠르겠네요

[UltiSnips Screencasts Ep 1 - What are snippets](https://www.youtube.com/watch?v=Zik6u0klD40)

* YCM 플러그인 활성화
{% highlight bash %}
$ sudo apt install vim-youcompleteme
$ vam install youcompleteme
{% endhighlight %}
* Ultisnip 플러그인 활성화
{% highlight bash %}
$ sudo apt install vim-ultisnips
$ vam install snippets ultisnips
{% endhighlight %}
