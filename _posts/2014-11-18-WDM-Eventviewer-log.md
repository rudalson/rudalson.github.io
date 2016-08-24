---
layout: post
title:  "윈도우즈 커널 드라이버에서 EventViewer를 위한 로그 작성"
categories: kernel
tags: wdm eventviewer log windows
---
커널 드라이버에서 Event Log를 생성하기 위해서는

* 구현부
* Message 리소스 작성
* 시스템에 리소스 path 작성

이렇게 3부분을 살펴 보아야 한다.

## System Event Log 작성을 위한 코드 작성
아래의 function과 data structure를 사용한다.

### Function
* IoWriteErrorLogEntry
* IoAllocateErrorLogEntry

### Data Structure
* IO_ERROR_LOG_PACKET

IoAllocateErrorLogEntry()으로 IO_ERROR_LOG_PACKET 타입의 메모리를 할당받은 후 적절한 값을 assign 후 IoWriteErrorLogEntry()으로 log를 작성하면 된다.
개념적으로는 IoFreeErrorLogEntry()이 필요하지만 드라이버에서 이를 직접 호출할 필요는 없다.

![친절한 스크린샷]({{ site.url }}/assets/2014/WDM_eventviewer_IO_ERROR_LOG_PACKET.png)

IO_ERROR_LOG_PACKET 구조체 값 뒷부분에 필요한 argument들을 연속적으로 넣어주면 된다.
이 구조체 값에는 몇몇 주요한 변수들이 있는데 이는 MSDN 문서를 참고한다.

과정은 단순하며 2가지 정도 주의할 점이 있다.

* 인자로 사용될 메모리 사이즈
* 2byte 문자열을 다룰 경우 IRQL 제한

인자의 사이즈 제한
IO_ERROR_LOG_PACKET 의 instance의 메모리 사이즈는 패킷 헤더 포함 size가 ERROR_LOG_MAXIMUM_SIZE를 넘지 못하며 헤더 패킷 size 48을 빼주면 52글자 정도 밖에는 쓸수 없다.
IoWriteErrorLogEntry passes at most 52 characters?
따라서 mc 파일로 작성된 것 외에 argument로는 52 character 그러니까 대략 104byte 정도 더 추가할 수 있다.

2byte 유니코드 사용으로 인한 IRQL 제한
1byte 문자열을 2byte 문자열로 변환할 때 라이브러리에서 제공해주는 일반적인 함수들은 유니코드 변환에 따른 테이블을 참조한다. 이것으로 인해 IRQL 문제가 발생한다.
참고로 변환에 사용해봤던 함수들은 RtlStringCbLength(), swprintf_s() 이며 모두 문제를 일으켰다. (DW-23)
IRQL for RtlStringxxx functions

## MC파일 작성
EventViewer에 남길 메시지를 별도의 text 리소스 파일로 작성해야 한다.
{% highlight bash %}
mvolmsg.mc
MessageID       = 5001
Severity        = Informational
SymbolicName    = MSG_DRIVER_LOAD
Language        = English
Driver is loading successfully.
.
MessageID       = 5002
Severity        = Error
SymbolicName    = MSG_NO_JOB
Language        = English
No Job on %2.
.
MessageID       = 5003
Severity        = Error
SymbolicName    = MSG_FIND_JOB_ERROR
Language        = English
cannot find same job on %2.
.


...
{% endhighlight %}
여기에 대한 좀더 자세한 내용은 Creating the Error Message Text File 을 참조한다.

## MC.exe compile

MC.exe는 Visual Studio 설치 폴더 중 별도로 있는 util 이며 이를 찾으려면 Windows 버전에 맞는 곳의 bin 경로에서 찾으면 된다.
(참고로 난 c:\Program Files (x86)\Windows Kits\8.1\bin\x64\)
mc 파일에 필요한 스트링 내용을 정리한 후 이를 message binary로 만드는 것은 아래의 명령어로 할 수 있다.
mc xxxxmsg.mc
이에 대한 자세한 내용은 Defining Custom Error Types를 참고하면 된다.
이렇게 mc build를 하고 나면
xxxxmsg.rc
xxxxmsg.h
MSG00001.bin
이렇게 3 파일이 생성되며 이를 모두 visual studio 프로젝트에 포함시켜 주면 된다.

## 출력 메시지 path 지정

드라이버에서 해야 할 작업을 모두 마쳤다면 마지막으로 레지스트리에 출력 메시지 binary의 path를 지정해 주는 일을 추가해 주어야 한다.
message binary의 위치가 지정되지 않았다면 아래처럼 실제 메시지 내용을 EventViewer에 보여주지 못한다.

![친절한 스크린샷]({{ site.url }}/assets/2014/WDM_eventviewer_failed_eventvwr_msg.png)

xxxx.sys 를 빌드하면서 메시지도 포함시켰다면 xxxx.sys의 경로를 레지스트리에 지정해 주어야 하는데
HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\EventLog\System\DriverName 경로에 아래의 값을 추가한다.
(관련내용 : Registering as a Source of Error Messages)

* EventMessageFile (REG_EXPAND_SZ)
* TypesSupported (REG_DWORD)



이렇게 설정 후 별도의 리붓 없이 EventViewer를 실행시키면


이렇게 메시지가 정상적으로 보여진다.

## Reference
* Writing to the System Event Log
* Registering as a Source of Error Messages
* Using MC.exe, message resources and the NT event log in your own projects
* Try, Except, Finally and IoWriteErrorLogEntry (Part 1)
* Message Compiler
