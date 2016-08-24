---
layout: post
title:  "몇 가지 windbg의 사용 예제"
categories: kernel
tags: wdm windbg windows
---
{% highlight bash %}
1: kd> !devobj 0xfffffa80193604c0
Device object (fffffa80193604c0) is for:
 HarddiskVolume3 \Driver\volmgr DriverObject fffffa801930d160
Current Irp 00000000 RefCount 17 Type 00000007 Flags 00003050
Vpb fffffa801934a270 Dacl fffff9a1004790d0 DevExt fffffa8019360610 DevObjExt fffffa8019360778 Dope fffffa801935a2e0 DevNode fffffa801936c300
ExtensionFlags (0000000000)  
Characteristics (0000000000)  
AttachedDevice (Upper) fffffa80196ec1f0 \Driver\fvevol
Device queue is not busy.
{% endhighlight %}

{% highlight bash %}
1: kd> !vpb 0xfffffa801934a270
Vpb at 0xfffffa801934a270
Flags: 0x1 mounted
DeviceObject: 0xfffffa80190e1030
RealDevice:   0xfffffa80193604c0
RefCount: 17
Volume Label:
{% endhighlight %}

{% highlight bash %}
3: kd> !drvobj \Driver\volmgr
Driver object (fffffa801913b7c0) is for:
 \Driver\volmgr
Driver Extension List: (id , addr)

Device Object list:
fffffa80193624c0  fffffa80193544c0  fffffa80193524c0  fffffa801934a4c0
fffffa80191818d0
{% endhighlight %}

{% highlight bash %}
3: kd> !object 0xfffffa8019aa6220
Object: 0xfffffa8019aa6220  Type: (0xfffffa8018e462a0) Device
    ObjectHeader: fffffa8019aa61f0 (new version)
    HandleCount: 0  PointerCount: 4
{% endhighlight %}

{% highlight bash %}
3: kd> !devnode
Dumping IopRootDeviceNode (= 0xfffffa8018dc57f0)
DevNode 0xfffffa8018dc57f0 for PDO 0xfffffa8018da6e30
  Parent 0000000000   Sibling 0000000000   Child 0xfffffa8018e00c10   
  InstancePath is "HTREE\ROOT\0"
  State = DeviceNodeStarted (0x308)
  Previous State = DeviceNodeEnumerateCompletion (0x30d)
  StateHistory[06] = DeviceNodeEnumerateCompletion (0x30d)
  StateHistory[05] = DeviceNodeEnumeratePending (0x30c)
  StateHistory[04] = DeviceNodeStarted (0x308)
  StateHistory[03] = DeviceNodeEnumerateCompletion (0x30d)
  StateHistory[02] = DeviceNodeEnumeratePending (0x30c)
  StateHistory[01] = DeviceNodeStarted (0x308)
  StateHistory[00] = DeviceNodeUninitialized (0x301)
  StateHistory[19] = Unknown State (0x0)
  StateHistory[18] = Unknown State (0x0)
  StateHistory[17] = Unknown State (0x0)
  StateHistory[16] = Unknown State (0x0)
  StateHistory[15] = Unknown State (0x0)
  StateHistory[14] = Unknown State (0x0)
  StateHistory[13] = Unknown State (0x0)
  StateHistory[12] = Unknown State (0x0)
  StateHistory[11] = Unknown State (0x0)
  StateHistory[10] = Unknown State (0x0)
  StateHistory[09] = Unknown State (0x0)
  StateHistory[08] = Unknown State (0x0)
  StateHistory[07] = Unknown State (0x0)
  Flags (0x00000131)  DNF_MADEUP, DNF_ENUMERATED,
                      DNF_IDS_QUERIED, DNF_NO_RESOURCE_REQUIRED
  DisableableDepends = 5 (from children)
{% endhighlight %}

{% highlight bash %}
2: kd> !handle 0xffffffff800007ac

PROCESS fffffa8018db5040
    SessionId: none  Cid: 0004    Peb: 00000000  ParentCid: 0000
    DirBase: 00187000  ObjectTable: fffff8a000001720  HandleCount: 490.
    Image: System

Kernel handle table at fffff8a000001720 with 490 entries in use

800007ac: Object: 0xfffffa801a850b20  GrantedAccess: 00000003 Entry: fffff8a000f81eb0
Object: fffffa801a850b20  Type: (fffffa8018e4d2a0) File
    ObjectHeader: fffffa801a850af0 (new version)
        HandleCount: 1  PointerCount: 1
{% endhighlight %}

{% highlight bash %}
1: kd> !fileobj 0xfffffa801af5b8d0

Device Object: 0xfffffa80193544c0   \Driver\volmgr
Vpb: 0xfffffa801abea180
Event signalled
Access: Read Write SharedRead SharedWrite

Flags:  0x44000a
	Synchronous IO
	No Intermediate Buffering
	Handle Created
	Volume Open

FsContext: 0xfffffa801b0e0310	FsContext2: 0xfffff8a00114a160
CurrentByteOffset: 0
Cache Data:
  Section Object Pointers: fffffa801ac37a90
  Shared Cache Map: 00000000
{% endhighlight %}

{% highlight bash %}
3: kd> !irpfind

*** CacheSize too low - increasing to 102 MB

Max cache size is       : 107343872 bytes (0x1997c KB)
Total memory in cache   : 7324 bytes (0x8 KB)
Number of regions cached: 40
102 full reads broken into 111 partial reads
    counts: 67 cached/44 uncached, 60.36% cached
    bytes : 2595 cached/4764 uncached, 35.26% cached
** Transition PTEs are implicitly decoded
** Prototype PTEs are implicitly decoded

Scanning large pool allocation table for Tag 'Irp?' (fffffa801a333000 : fffffa801a3f3000)

  Irp            [ Thread ]         irpStack: (Mj,Mn)   DevObj          [Driver]         MDL Process
fffffa801922d430 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0xfffff80000000003
fffffa801a2ff900 [0000000000000000] Irp is complete (CurrentLocation 4 > StackCount 3)
fffffa80193d3360 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa80192254f0 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa8019753390 [fffffa801a9f9b50] irpStack: ( 3, 0)  fffffa801a9c9980 [ \Driver\mouclass]
fffffa80193c5450 [0000000000000000] Irp is complete (CurrentLocation 9 > StackCount 8)
fffffa8019a9a6d0 [0000000000000000] Irp is complete (CurrentLocation 2 > StackCount 1)
fffffa80192a7330 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x5364615602050002
fffffa8019b69570 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0xfffffa801b4e5f21
fffffa8019b69c60 [fffffa801b070060] irpStack: ( c, 2)  fffffa8019add030 [ \FileSystem\Ntfs]
fffffa801b32e040 [0000000000000000] Irp is complete (CurrentLocation 2 > StackCount 1)
fffffa801936b3a0 [0000000000000000] irpStack: ( f, 5)  fffffa80198ae9a0 [ \Driver\AFD]
fffffa801b119800 [fffffa801b125ad0] irpStack: ( e, 0)  fffffa8018da5850 [ \Driver\Beep]
fffffa801a22c8b0 [fffffa801b454060] irpStack: ( c, 2)  fffffa8019add030 [ \FileSystem\Ntfs]
fffffa80192234f0 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0xfffffa801aa034a0
fffffa8019219c10 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa8019219e10 [0000000000000000] irpStack: ( f, 0)  00000000 [00000000: Could not read device object or _DEVICE_OBJECT not found
]
fffffa8019355600 [0000000000000000] Irp is complete (CurrentLocation 5 > StackCount 4)
fffffa80192434f0 [0000000000000000] irpStack: ( f, 0)  00000000 [00000000: Could not read device object or _DEVICE_OBJECT not found
]
fffffa801921d4f0 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa801a2e4c60 [fffffa801b920630] irpStack: ( c, 2)  fffffa8019add030 [ \FileSystem\Ntfs]
fffffa80193c3320 [0000000000000000] Irp is complete (CurrentLocation 2 > StackCount 1)
fffffa80193c3450 [0000000000000000] irpStack: ( f, 0)  fffffa8019c29050 [ \Driver\usbuhci]
fffffa80192a9330 [0000000000000000] Irp is complete (CurrentLocation 5 > StackCount 4)

Searching NonPaged pool (fffffa8018c04000 : fffffa8075c00000) for Tag 'Irp?'

fffffa8018d45430 [fffffa801aa589e0] irpStack: ( d, 0)  fffffa80196a1aa0 [ \FileSystem\Npfs]
fffffa8018d46d00 [fffffa801a9fa060] irpStack: ( 3, 0)  fffffa8019c02960 [ \Driver\kbdclass]
fffffa8018daf010 [0000000000000000] Irp is complete (CurrentLocation 31 > StackCount 30)
fffffa8018db0010 [0000000000000000] Irp is complete (CurrentLocation 31 > StackCount 30)
fffffa8018db1010 [0000000000000000] Irp is complete (CurrentLocation 31 > StackCount 30)
fffffa8018db2010 [0000000000000000] Irp is complete (CurrentLocation 31 > StackCount 30)
fffffa8018db3010 [0000000000000000] Irp is complete (CurrentLocation 31 > StackCount 30)
fffffa8018dba520 [fffffa801aabeb50] irpStack: ( d, 0)  fffffa80196a1aa0 [ \FileSystem\Npfs]
fffffa8018dba650 [fffffa801aaaab50] irpStack: ( d, 0)  fffffa80196a1aa0 [ \FileSystem\Npfs]
fffffa8018dba780 [0000000000000000] Irp is complete (CurrentLocation 9 > StackCount 8)
fffffa8018dc4190 [fffffa801aa2a060] irpStack: ( e, 0)  fffffa801b1069a0 [ \Driver\mpsdrv]
fffffa8018dc4320 [fffffa801aebab50] irpStack: ( d, 0)  fffffa80196a1aa0 [ \FileSystem\Npfs]
fffffa8018df9ee0 [0000000000000000] Irp is complete (CurrentLocation 2 > StackCount 1)
fffffa8018e004a0 [fffffa801ab20b50] irpStack: ( e,2d)  fffffa80198ae9a0 [ \Driver\AFD]
fffffa8018e06010 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa80190d4870 [0000000000000000] irpStack: ( e, 0)  fffffa801961d3e0 [ \Driver\ACPI]
fffffa80190d8ce0 [fffffa801a994060] irpStack: ( d, 0)  fffffa80196a1aa0 [ \FileSystem\Npfs]
fffffa80190d96e0 [0000000000000000] Irp is complete (CurrentLocation 2 > StackCount 1)
fffffa8019134760 [0000000000000000] irpStack: ( f, 0)  00000000 [00000000: Could not read device object or _DEVICE_OBJECT not found
]
fffffa801914a760 [fffffa801a9f9b50] irpStack: ( 3, 0)  fffffa8019c03980 [ \Driver\mouclass]
fffffa801914e760 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0xfffffa801b30c80f
fffffa801914f760 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0xfffffa8019755078
fffffa8019176300 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0xfffffa801b4a7870
fffffa801917b9f0 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0xfffffa801b9632b0
fffffa80191a8a60 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa8019622780 [0000000000000000] Irp is complete (CurrentLocation 2 > StackCount 1)
fffffa8019624430 [0000000000000000] Irp is complete (CurrentLocation 2 > StackCount 1)
fffffa80196aee10 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000200000000
fffffa80196b2c60 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0xfffffa8019add180
fffffa80196b3e10 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa80196b6e10 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0xfffffa801b3d3bd8
fffffa80196b8e10 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x000000b800000018
fffffa80196bac60 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa80196bb860 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa80196bce10 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa80196be6f0 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa80196bf3e0 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa8019701c60 [fffffa801a9fa060] irpStack: ( 3, 0)  fffffa8019b792e0 [ \Driver\kbdclass]
fffffa80197166f0 [fffffa80197156c0] irpStack: ( e, 0)  fffffa8018de95e0 [ \Driver\WMIxWDM]
fffffa8019730bd0 [fffffa801a9f9b50] irpStack: ( 3, 0)  fffffa801a9c5060 [ \Driver\mouclass]
fffffa8019b57010 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa8019c09730 [0000000000000000] Irp is complete (CurrentLocation 4 > StackCount 3)
fffffa8019c22950 [fffffa801b27bb50] irpStack: ( e,20)  fffffa80198ae9a0 [ \Driver\AFD] 0xfffffa801aa034a0
fffffa8019c6d370 [0000000000000000] Irp is complete (CurrentLocation 2 > StackCount 1) 0x0000000000000000
fffffa8019c6d4f0 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa8019c6d6f0 [0000000000000000] Irp is complete (CurrentLocation 5 > StackCount 4)
fffffa8019c6dac0 [fffffa801aaaab50] irpStack: ( d, 0)  fffffa80196a1aa0 [ \FileSystem\Npfs]
fffffa8019f6fa60 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa8019f6fe10 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa8019f70a60 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa8019f70e10 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa801a1df290 [0000000000000000] irpStack: ( f, 0)  fffffa8019c29050 [ \Driver\usbuhci]
fffffa801a1e1e10 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa801a1e2a00 [0000000000000000] Irp is complete (CurrentLocation 4 > StackCount 3)
fffffa801a235a60 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa801a235e10 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa801a238a60 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa801a238e10 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa801a239a60 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa801a239e10 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa801a23ea60 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa801a23ee10 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000003200000000
fffffa801a242a60 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa801a242e10 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa801a243b00 [fffffa801aebab50] irpStack: ( e,20)  fffffa80198ae9a0 [ \Driver\AFD] 0xfffffa801a97a5f0
fffffa801a247a60 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa801a247e10 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa801a284010 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2)
fffffa801a2906b0 [0000000000000000] Irp is complete (CurrentLocation 2 > StackCount 1)
fffffa801a292a60 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa801a292e10 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa801a293a60 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa801a293e10 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa801a294a60 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa801a294e10 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa801a295a60 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa801a295e10 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa801a296a60 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa801a296e10 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa801a297a60 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa801a297e10 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa801a298a60 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa801a298e10 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa801a299a60 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa801a299e10 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa801a29aa60 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0xfffffa8019750078
fffffa801a29ae10 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa801a2a1c60 [0000000000000000] irpStack: ( f, 0)  fffffa8019c29050 [ \Driver\usbuhci]
fffffa801a2a89e0 [fffffa801aaa18a0] irpStack: ( e,20)  fffffa80198ae9a0 [ \Driver\AFD] 0xfffffa801aa17b30
fffffa801a2f7b70 [0000000000000000] Irp is complete (CurrentLocation 2 > StackCount 1)
fffffa801a950490 [0000000000000000] irpStack: ( f, 0)  fffffa8019c29050 [ \Driver\usbuhci]
fffffa801a950840 [0000000000000000] irpStack: ( 3, 0)  fffffa801a9c7b30 [ \Driver\HidUsb] 0x0000000000000000
fffffa801a981c50 [fffffa801aebab50] irpStack: ( e,20)  fffffa80198ae9a0 [ \Driver\AFD] 0xfffffa801a97a5f0
fffffa801a9c2770 [0000000000000000] irpStack: ( 3, 0)  fffffa801a9a7530 [ \Driver\HidUsb] 0x0000000000000000
fffffa801a9c6310 [fffffa801b49eb50] irpStack: ( d, 0)  fffffa80196a1aa0 [ \FileSystem\Npfs]
fffffa801a9c6820 [0000000000000000] irpStack: ( f, 0)  fffffa8019c29050 [ \Driver\usbuhci]
fffffa801a9d2370 [fffffa801ae9cb50] irpStack: ( d, 0)  fffffa80196a1aa0 [ \FileSystem\Npfs]
fffffa801a9dd790 [fffffa801b3d94c0] irpStack: ( e,20)  fffffa80198ae9a0 [ \Driver\AFD] 0xfffffa801b39db30
fffffa801aa069d0 [fffffa801a9f9b50] irpStack: ( 3, 0)  fffffa8019b72cf0 [ \Driver\mouclass]
fffffa801aa135a0 [0000000000000000] Irp is complete (CurrentLocation 2 > StackCount 1)
fffffa801aa2b670 [fffffa801aa2a060] irpStack: ( e, 0)  fffffa801b1069a0 [ \Driver\mpsdrv]
fffffa801aa2c370 [fffffa801b584060] irpStack: ( d, 0)  fffffa8019add030 [ \FileSystem\Ntfs] 0xfffffa801b46a3d0
fffffa801aa53360 [fffffa801b015060] irpStack: ( d, 0)  fffffa80196a1aa0 [ \FileSystem\Npfs]
fffffa801aa57010 [fffffa801b349590] irpStack: ( c, 2)  fffffa8019add030 [ \FileSystem\Ntfs]
fffffa801aa70290 [0000000000000000] Irp is complete (CurrentLocation 2 > StackCount 1)
fffffa801aa74010 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x7866744e02100002
fffffa801aa7e2f0 [fffffa801b032a00] irpStack: ( d, 0)  fffffa80196a1aa0 [ \FileSystem\Npfs]
fffffa801aa87010 [fffffa801aff1710] irpStack: ( d, 0)  fffffa8019add030 [ \FileSystem\Ntfs]
fffffa801aa9d010 [fffffa801b3dfb50] irpStack: ( c, 2)  fffffa8019add030 [ \FileSystem\Ntfs]
fffffa801aac8010 [fffffa801aff1710] irpStack: ( d, 0)  fffffa80196a1aa0 [ \FileSystem\Npfs]
fffffa801aac8700 [fffffa801aa98b50] irpStack: ( d, 0)  fffffa80196a1aa0 [ \FileSystem\Npfs]
fffffa801aadfe10 [fffffa801aa02560] irpStack: ( e,20)  fffffa80198ae9a0 [ \Driver\AFD] 0xfffffa801a943060
fffffa801aae3d10 [fffffa801aa62b50] irpStack: ( d, 0)  fffffa80196a1aa0 [ \FileSystem\Npfs]
fffffa801ab06b80 [fffffa801aeba060] irpStack: ( e, 5)  fffffa80198ae9a0 [ \Driver\AFD] 0xfffffa801aa91060
fffffa801ab0e9b0 [fffffa801aebbb50] irpStack: ( d, 0)  fffffa80196a1aa0 [ \FileSystem\Npfs]
fffffa801ab10c30 [fffffa801ab1e8e0] irpStack: ( d, 0)  fffffa80196a1aa0 [ \FileSystem\Npfs]
fffffa801ab17ee0 [fffffa801ab2cb50] irpStack: ( d, 0)  fffffa80196a1aa0 [ \FileSystem\Npfs]
fffffa801ab2f010 [0000000000000000] Irp is complete (CurrentLocation 2 > StackCount 1)
fffffa801ab3ac80 [fffffa801a994060] irpStack: ( e,20)  fffffa80198ae9a0 [ \Driver\AFD] 0xfffffa801a943060
fffffa801ab3d500 [0000000000000000] Irp is complete (CurrentLocation 2 > StackCount 1)
fffffa801ae9ea70 [0000000000000000] Irp is complete (CurrentLocation 2 > StackCount 1)
fffffa801aea68f0 [fffffa801b231060] irpStack: ( e, 0)  fffffa8018de95e0 [ \Driver\WMIxWDM]
fffffa801aeb6900 [fffffa801ab1e8e0] irpStack: ( e,20)  fffffa80198ae9a0 [ \Driver\AFD] 0xfffffa801ab19960
fffffa801aec1010 [fffffa801aaeeb50] irpStack: ( d, 0)  fffffa8019add030 [ \FileSystem\Ntfs] 0x0000000000000000
fffffa801aed6010 [fffffa801b349590] irpStack: ( c, 2)  fffffa8019add030 [ \FileSystem\Ntfs]
fffffa801aedb010 [fffffa801aff1710] irpStack: ( c, 2)  fffffa8019add030 [ \FileSystem\Ntfs]
fffffa801aedb3c0 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa801aee09a0 [fffffa801ab42060] irpStack: ( d, 0)  fffffa80196a1aa0 [ \FileSystem\Npfs]
fffffa801aee48f0 [fffffa801aa05b50] irpStack: ( d, 0)  fffffa80196a1aa0 [ \FileSystem\Npfs]
fffffa801aee5a40 [0000000000000000] Irp is complete (CurrentLocation 2 > StackCount 1)
fffffa801aeecb80 [fffffa801aa2a060] irpStack: ( e, 0)  fffffa801b1069a0 [ \Driver\mpsdrv]
fffffa801aefcc60 [fffffa801af23b50] irpStack: ( c, 2)  fffffa8019add030 [ \FileSystem\Ntfs]
fffffa801aefdc60 [fffffa801b039060] irpStack: ( c, 2)  fffffa8019add030 [ \FileSystem\Ntfs]
fffffa801af043e0 [fffffa801b125ad0] irpStack: ( e, 0)  fffffa8018da5850 [ \Driver\Beep]
fffffa801af0d010 [fffffa801aaa18a0] irpStack: ( c, 2)  fffffa8019add030 [ \FileSystem\Ntfs]
fffffa801af0e250 [fffffa801b125ad0] irpStack: ( e, 0)  fffffa8018da5850 [ \Driver\Beep]
fffffa801af25230 [fffffa801b125ad0] irpStack: ( e, 0)  fffffa8018da5850 [ \Driver\Beep]
fffffa801af34d10 [fffffa801af21b50] irpStack: ( d, 0)  fffffa80196a1aa0 [ \FileSystem\Npfs]
fffffa801af37d80 [fffffa801aa2a060] irpStack: ( e, 0)  fffffa801b1069a0 [ \Driver\mpsdrv]
fffffa801af3dee0 [fffffa801b011060] irpStack: ( e, 0)  fffffa8019b1d8f0 [ \Driver\NetBT] 0xfffffa801a97a5f0
fffffa801af3e870 [fffffa801b125ad0] irpStack: ( e, 0)  fffffa8018da5850 [ \Driver\Beep]
fffffa801af3f360 [fffffa801aaa18a0] irpStack: ( c, 2)  fffffa8019add030 [ \FileSystem\Ntfs]
fffffa801af40de0 [fffffa801b030780] irpStack: ( d, 0)  fffffa80196a1aa0 [ \FileSystem\Npfs]
fffffa801af4f520 [fffffa801b125ad0] irpStack: ( e, 0)  fffffa8018da5850 [ \Driver\Beep]
fffffa801af8fe10 [fffffa801ab1e8e0] irpStack: ( e,20)  fffffa80198ae9a0 [ \Driver\AFD] 0xfffffa801ab19960
fffffa801afd53e0 [0000000000000000] irpStack: ( f, d)  fffffa80198ae9a0 [ \Driver\AFD]
fffffa801afd7c20 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa801afd8cb0 [fffffa801b1acb50] irpStack: ( e, 0)  fffffa801b102d40 [ \FileSystem\bowser]
fffffa801afd9760 [fffffa801b05a060] irpStack: ( e,20)  fffffa80198ae9a0 [ \Driver\AFD] 0xfffffa801af13b30
fffffa801afdf180 [fffffa801b3b4b50] irpStack: ( c, 2)  fffffa8019add030 [ \FileSystem\Ntfs]
fffffa801afef320 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000003200000000
fffffa801affc900 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0xfffff8a0019fa4f0
fffffa801b018820 [fffffa801b580060] irpStack: ( d, 0)  fffffa80196a1aa0 [ \FileSystem\Npfs]
fffffa801b018950 [fffffa801b004060] irpStack: ( e, 0)  fffffa8019b1d8f0 [ \Driver\NetBT] 0xfffffa801a97a5f0
fffffa801b02e010 [fffffa801b02c060] irpStack: ( e,2d)  fffffa80198ae9a0 [ \Driver\AFD]
fffffa801b0523a0 [fffffa801afa2b50] irpStack: ( d, 0)  fffffa8019b05030 [ \Driver\CSC]
fffffa801b077aa0 [fffffa801b05a060] irpStack: ( e,20)  fffffa80198ae9a0 [ \Driver\AFD] 0xfffffa801af13b30
fffffa801b078e10 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa801b084950 [fffffa801b125ad0] irpStack: ( e, 0)  fffffa8018da5850 [ \Driver\Beep]
fffffa801b09c260 [fffffa801b27bb50] irpStack: ( e,20)  fffffa80198ae9a0 [ \Driver\AFD] 0xfffffa801aa034a0
fffffa801b0ae8f0 [fffffa801b5255d0] irpStack: ( e, 0)  fffffa8019b50600 [ \Driver\nsiproxy]
fffffa801b0cf830 [fffffa801b125ad0] irpStack: ( e, 0)  fffffa8018da5850 [ \Driver\Beep]
fffffa801b0cf9e0 [fffffa801b125ad0] irpStack: ( e, 0)  fffffa8018da5850 [ \Driver\Beep]
fffffa801b0d0ad0 [fffffa801b125ad0] irpStack: ( e, 0)  fffffa8018da5850 [ \Driver\Beep]
fffffa801b0e03e0 [fffffa801aebc060] irpStack: ( e, 3)  fffffa80198ae9a0 [ \Driver\AFD]
fffffa801b0e2b60 [fffffa801ab20b50] irpStack: ( e,2d)  fffffa80198ae9a0 [ \Driver\AFD]
fffffa801b162010 [0000000000000000] Irp is complete (CurrentLocation 2 > StackCount 1)
fffffa801b1b6550 [fffffa801b1b3530] irpStack: ( c, 2)  fffffa8019add030 [ \FileSystem\Ntfs]
fffffa801b1fbcc0 [0000000000000000] Irp is complete (CurrentLocation 2 > StackCount 1)
fffffa801b216930 [fffffa801b349590] irpStack: ( c, 2)  fffffa8019add030 [ \FileSystem\Ntfs]
fffffa801b227ee0 [fffffa801b014540] irpStack: ( e, 0)  fffffa8019b50600 [ \Driver\nsiproxy]
fffffa801b249330 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x00780065002e0074
fffffa801b24c720 [fffffa801b349590] irpStack: ( c, 2)  fffffa8019add030 [ \FileSystem\Ntfs]
fffffa801b263c60 [fffffa801b349590] irpStack: ( c, 2)  fffffa8019add030 [ \FileSystem\Ntfs]
fffffa801b26dee0 [fffffa801b36f060] irpStack: ( d, 0)  fffffa80196a1aa0 [ \FileSystem\Npfs]
fffffa801b27b900 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa801b27d010 [fffffa801b3dfb50] irpStack: ( c, 2)  fffffa8019add030 [ \FileSystem\Ntfs]
fffffa801b29f8b0 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa801b2e5010 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0xfffffa801b30c80f
fffffa801b2e55c0 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0xfffffa8019387280
fffffa801b2e6c60 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000020
fffffa801b2e78b0 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0xfffffa801b2e98f0
fffffa801b2e7c60 [fffffa801b349590] irpStack: ( c, 2)  fffffa8019add030 [ \FileSystem\Ntfs]
fffffa801b2e85c0 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa801b2e9a60 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa801b2eaa60 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000080005
fffffa801b2ed010 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000416
fffffa801b2eee10 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa801b2efc60 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0xfffff8a0013ffca8
fffffa801b2f0e10 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa801b2f2010 [fffffa801b349590] irpStack: ( c, 2)  fffffa8019add030 [ \FileSystem\Ntfs]
fffffa801b2f2a60 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0xfffff8a0013ffca8
fffffa801b2f2c60 [fffffa801ab31370] irpStack: ( c, 2)  fffffa8019add030 [ \FileSystem\Ntfs]
fffffa801b308290 [0000000000000000] Irp is complete (CurrentLocation 2 > StackCount 1)
fffffa801b3095c0 [0000000000000000] Irp is complete (CurrentLocation 2 > StackCount 1)
fffffa801b309ee0 [0000000000000000] Irp is complete (CurrentLocation 2 > StackCount 1)
fffffa801b30a010 [fffffa801b349590] irpStack: ( c, 2)  fffffa8019add030 [ \FileSystem\Ntfs]
fffffa801b30a880 [fffffa801b349590] irpStack: ( c, 2)  fffffa8019add030 [ \FileSystem\Ntfs]
fffffa801b30b6b0 [0000000000000000] Irp is complete (CurrentLocation 2 > StackCount 1)
fffffa801b30bc60 [0000000000000000] Irp is complete (CurrentLocation 2 > StackCount 1)
fffffa801b310010 [fffffa801b349590] irpStack: ( c, 2)  fffffa8019add030 [ \FileSystem\Ntfs]
fffffa801b311660 [fffffa801b349590] irpStack: ( c, 2)  fffffa8019add030 [ \FileSystem\Ntfs]
fffffa801b312c60 [0000000000000000] irpStack: (16, 0)  fffffa8019c57050 [ \Driver\usbehci]
fffffa801b324860 [0000000000000000] Irp is complete (CurrentLocation 2 > StackCount 1)
fffffa801b335250 [0000000000000000] Irp is complete (CurrentLocation 2 > StackCount 1)
fffffa801b3355c0 [0000000000000000] Irp is complete (CurrentLocation 2 > StackCount 1)
fffffa801b335930 [0000000000000000] Irp is complete (CurrentLocation 2 > StackCount 1)
fffffa801b335ca0 [0000000000000000] Irp is complete (CurrentLocation 2 > StackCount 1)
fffffa801b336490 [0000000000000000] Irp is complete (CurrentLocation 2 > StackCount 1)
fffffa801b336800 [0000000000000000] Irp is complete (CurrentLocation 2 > StackCount 1)
fffffa801b336b70 [0000000000000000] Irp is complete (CurrentLocation 2 > StackCount 1)
fffffa801b336ee0 [0000000000000000] Irp is complete (CurrentLocation 2 > StackCount 1)
fffffa801b337010 [0000000000000000] Irp is complete (CurrentLocation 2 > StackCount 1)
fffffa801b337250 [0000000000000000] Irp is complete (CurrentLocation 2 > StackCount 1)
fffffa801b3375c0 [0000000000000000] Irp is complete (CurrentLocation 2 > StackCount 1)
fffffa801b337930 [0000000000000000] Irp is complete (CurrentLocation 2 > StackCount 1)
fffffa801b337ca0 [0000000000000000] Irp is complete (CurrentLocation 2 > StackCount 1)
fffffa801b338490 [0000000000000000] Irp is complete (CurrentLocation 2 > StackCount 1)
fffffa801b338800 [0000000000000000] Irp is complete (CurrentLocation 2 > StackCount 1)
fffffa801b338b70 [0000000000000000] Irp is complete (CurrentLocation 2 > StackCount 1)
fffffa801b338ee0 [0000000000000000] Irp is complete (CurrentLocation 2 > StackCount 1)
fffffa801b339010 [0000000000000000] Irp is complete (CurrentLocation 2 > StackCount 1)
fffffa801b339250 [0000000000000000] Irp is complete (CurrentLocation 2 > StackCount 1)
fffffa801b3395c0 [0000000000000000] Irp is complete (CurrentLocation 2 > StackCount 1)
fffffa801b339930 [0000000000000000] Irp is complete (CurrentLocation 2 > StackCount 1)
fffffa801b339ca0 [0000000000000000] Irp is complete (CurrentLocation 2 > StackCount 1)
fffffa801b33aee0 [fffffa801b3ac630] irpStack: ( d, 0)  fffffa80196a1aa0 [ \FileSystem\Npfs]
fffffa801b33b010 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa801b3636b0 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa801b365870 [0000000000000000] irpStack: ( f, d)  fffffa80198ae9a0 [ \Driver\AFD]
fffffa801b3688e0 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa801b369a10 [fffffa801aa98b50] irpStack: ( e,20)  fffffa80198ae9a0 [ \Driver\AFD] 0xfffffa801aa17b30
fffffa801b37e560 [0000000000000000] irpStack: (16, 0)  fffffa801a998060 [ \Driver\usbhub]
fffffa801b393170 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa801b3a6530 [fffffa801b1acb50] irpStack: ( e, 0)  fffffa801b102d40 [ \FileSystem\bowser]
fffffa801b3a7b60 [fffffa801b1acb50] irpStack: ( e, 0)  fffffa801b102d40 [ \FileSystem\bowser] 0xfffffa801af13b30
fffffa801b3a7d60 [fffffa801b1acb50] irpStack: ( e, 0)  fffffa801b102d40 [ \FileSystem\bowser]
fffffa801b3a8010 [fffffa801b3e0ab0] irpStack: ( e,20)  fffffa80198ae9a0 [ \Driver\AFD] 0xfffffa801b39db30
fffffa801b3ae580 [fffffa801ab26b50] irpStack: ( c, 2)  fffffa8019add030 [ \FileSystem\Ntfs]
fffffa801b3afb10 [fffffa801b349590] irpStack: ( c, 2)  fffffa8019add030 [ \FileSystem\Ntfs]
fffffa801b3c53c0 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa801b3d9010 [fffffa801b349590] irpStack: ( c, 2)  fffffa8019add030 [ \FileSystem\Ntfs]
fffffa801b3d9c60 [fffffa801b349590] irpStack: ( c, 2)  fffffa8019add030 [ \FileSystem\Ntfs]
fffffa801b3ee010 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0xfffffa801b46a3d0
fffffa801b413c90 [0000000000000000] Irp is complete (CurrentLocation 2 > StackCount 1)
fffffa801b41d330 [0000000000000000] Irp is complete (CurrentLocation 4 > StackCount 3)
fffffa801b423880 [fffffa801b349590] irpStack: ( c, 2)  fffffa8019add030 [ \FileSystem\Ntfs]
fffffa801b448970 [fffffa801b3dfb50] irpStack: ( c, 2)  fffffa8019add030 [ \FileSystem\Ntfs]
fffffa801b4656f0 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0xfffffa80196e4a30
fffffa801b51be10 [0000000000000000] Irp is complete (CurrentLocation 4 > StackCount 3)
fffffa801b51c2b0 [0000000000000000] Irp is complete (CurrentLocation 4 > StackCount 3)
fffffa801b51d390 [0000000000000000] Irp is complete (CurrentLocation 4 > StackCount 3)
fffffa801b51de10 [0000000000000000] Irp is complete (CurrentLocation 4 > StackCount 3)
fffffa801b522420 [0000000000000000] Irp is complete (CurrentLocation 4 > StackCount 3)
fffffa801b527010 [0000000000000000] Irp is complete (CurrentLocation 4 > StackCount 3)
fffffa801b5272d0 [0000000000000000] Irp is complete (CurrentLocation 4 > StackCount 3)
fffffa801b5282d0 [0000000000000000] Irp is complete (CurrentLocation 4 > StackCount 3)
fffffa801b529870 [0000000000000000] Irp is complete (CurrentLocation 4 > StackCount 3)
fffffa801b52a940 [0000000000000000] Irp is complete (CurrentLocation 4 > StackCount 3)
fffffa801b52bb00 [0000000000000000] Irp is complete (CurrentLocation 4 > StackCount 3)
fffffa801b52cbd0 [0000000000000000] Irp is complete (CurrentLocation 4 > StackCount 3)
fffffa801b52dbd0 [0000000000000000] Irp is complete (CurrentLocation 4 > StackCount 3)
fffffa801b52ebd0 [0000000000000000] Irp is complete (CurrentLocation 4 > StackCount 3)
fffffa801b52fbd0 [0000000000000000] Irp is complete (CurrentLocation 4 > StackCount 3)
fffffa801b530bd0 [0000000000000000] Irp is complete (CurrentLocation 4 > StackCount 3)
fffffa801b531e10 [0000000000000000] Irp is complete (CurrentLocation 4 > StackCount 3)
fffffa801b532bd0 [0000000000000000] Irp is complete (CurrentLocation 4 > StackCount 3)
fffffa801b533e10 [0000000000000000] Irp is complete (CurrentLocation 4 > StackCount 3)
fffffa801b534e10 [0000000000000000] Irp is complete (CurrentLocation 4 > StackCount 3)
fffffa801b535270 [0000000000000000] Irp is complete (CurrentLocation 4 > StackCount 3)
fffffa801b535e10 [0000000000000000] Irp is complete (CurrentLocation 4 > StackCount 3)
fffffa801b537010 [0000000000000000] Irp is complete (CurrentLocation 4 > StackCount 3)
fffffa801b538010 [0000000000000000] Irp is complete (CurrentLocation 4 > StackCount 3)
fffffa801b539010 [0000000000000000] Irp is complete (CurrentLocation 4 > StackCount 3)
fffffa801b53a010 [0000000000000000] Irp is complete (CurrentLocation 4 > StackCount 3)
fffffa801b53b010 [0000000000000000] Irp is complete (CurrentLocation 4 > StackCount 3)
fffffa801b53c010 [0000000000000000] Irp is complete (CurrentLocation 4 > StackCount 3)
fffffa801b53d010 [0000000000000000] Irp is complete (CurrentLocation 4 > StackCount 3)
fffffa801b53d230 [0000000000000000] Irp is complete (CurrentLocation 4 > StackCount 3)
fffffa801b53e300 [fffffa801b91a580] irpStack: ( e,2d)  fffffa80198ae9a0 [ \Driver\AFD]
fffffa801b543560 [fffffa801aead060] irpStack: ( e, 0)  fffffa8018de95e0 [ \Driver\WMIxWDM]
fffffa801b559010 [fffffa801b91a580] irpStack: ( e,2d)  fffffa80198ae9a0 [ \Driver\AFD]
fffffa801b5794f0 [fffffa801b57c060] irpStack: ( e, 0)  fffffa8018de95e0 [ \Driver\WMIxWDM]
fffffa801b5a3400 [fffffa801b349590] irpStack: ( c, 2)  fffffa8019add030 [ \FileSystem\Ntfs]
fffffa801b937aa0 [0000000000000000] Irp is complete (CurrentLocation 3 > StackCount 2) 0x0000000000000000
fffffa801c4e9370 [fffffa801aaa18a0] irpStack: ( d, 0)  fffffa80196a1aa0 [ \FileSystem\Npfs]
{% endhighlight %}

{% highlight bash %}
0: kd> !locks 85205ec8

Resource @ 0x85205ec8    Exclusively owned
    Contention Count = 7
    NumberOfExclusiveWaiters = 7
     Threads: 88b9fcb0-01<*>
     Threads Waiting On Exclusive Access:
              850bbd78       89b2b3f0       89754030       84753d78  <<<
remember ME
              84e92b78       853cf760       89b44578       
{% endhighlight %}

{% highlight bash %}
0: kd> !thread 88b9fcb0
THREAD 88b9fcb0  Cid 1548.16e4  Teb: 7ffac000 Win32Thread: fe288838 WAIT:
(Executive) KernelMode Non-Alertable
    850a5f88  NotificationEvent
IRP List:
    85102de0: (0006,0220) Flags: 00000884  Mdl: 00000000
    86693de0: (0006,0220) Flags: 00000884  Mdl: 00000000
Not impersonating
DeviceMap                 80fe7a68
Owning Process            b469a3c8       Image:         PSDrt.exe
Attached Process          N/A            Image:         N/A
Wait Start TickCount      171370         Ticks: 26804 (0:00:06:58.145)
Context Switch Count      78             
UserTime                  00:00:00.015
KernelTime                00:00:00.031
Win32 Start Address 0x745529e1
Stack Init 8b488000 Current 8b4870e0 Base 8b488000 Limit 8b485000 Call 0
Priority 14 BasePriority 8 PriorityDecrement 4 IoPriority 2 PagePriority 5
ChildEBP RetAddr  Args to Child              
8b4870f8 820fb352 88b9fcb0 88b9fd38 00000000 nt!KiSwapContext+0x26 (FPO: [Uses EBP] [0,0,4])
8b48713c 82096f28 88b9fcb0 00004005 86331be8 nt!KiSwapThread+0x44f
8b487190 8a615901 850a5f88 00000000 00000000 nt!KeWaitForSingleObject+0x492
8b487210 8a696a29 86331be8 c00000d8 012c58df Ntfs!NtfsPreRequestProcessingExtend+0x6b0 (FPO: [Non-Fpo])
8b487308 820939c6 850a5020 85102de0 85102de0 Ntfs!NtfsFsdCreate+0x18a (FPO: [Non-Fpo])
8b487320 826e8ba7 85102de0 00000000 85102e98 nt!IofCallDriver+0x63
8b487344 826fb643 8b487364 866a3ac8 00000000 fltmgr!FltpLegacyProcessingAfterPreCallbacksCompleted+0x251 (FPO: [Non-Fpo])
8b487390 820939c6 866a3ac8 85205b78 88909f64 fltmgr!FltpCreate+0x2a1 (FPO: [Non-Fpo])
8b4873a8 8228e195 705c78be 930a9b9c 86331d18 nt!IofCallDriver+0x63
8b487478 8227c5e1 86331d30 00000000 930a9af8 nt!IopParseDevice+0xf61
8b487508 82289b62 00000000 8b487560 00000240 nt!ObpLookupObjectName+0x5a8
8b48756c 8228f29c 8b487708 00000000 00000000 nt!ObOpenObjectByName+0x13c
8b4875e0 822940ba 8b487748 00100180 8b487708 nt!IopCreateFile+0x63b
8b48763c 826fd7dc 8b487748 00100180 8b487708 nt!IoCreateFileEx+0x9d
8b4876c0 82706058 88919310 865deb48 8b487748 fltmgr!FltCreateFileEx2+0xae (FPO: [Non-Fpo])
8b487728 8270667e 865deb48 8b487748 00000000 fltmgr!FltOpenVolume+0x66 (FPO: [Non-Fpo])
8b487740 8f89f6c6 865deb48 8b487794 c3523008 fltmgr!FltQueryVolumeInformation+0x14 (FPO: [Non-Fpo])
8b4877a8 8f8a54ac 00000000 8afac4a0 00000002 SRTSP+0x3423
8b4877ec 826fc8a0 8b487808 00000005 00000008 SRTSP+0x1763
8b487820 826fd09f 865deb48 00000005 09274055 fltmgr!FltpDoInstanceSetupNotification+0x66 (FPO: [Non-Fpo])
8b48786c 826fd457 88919310 85205b78 00000005 fltmgr!FltpInitInstance+0x245 (FPO: [Non-Fpo])
8b4878dc 826fd55d 88919310 85205b78 00000005 fltmgr!FltpCreateInstanceFromName+0x285 (FPO: [Non-Fpo])
8b487948 827064ca 88919310 85205b78 00000005 fltmgr!FltpEnumerateRegistryInstances+0xf9 (FPO: [Non-Fpo])
8b487998 826fb5a7 85205b78 895372d8 86693eb4 fltmgr!FltpDoFilterNotificationForNewVolume+0xe0 (FPO: [Non-Fpo])
8b4879dc 820939c6 866a3ac8 85205b78 86693de0 fltmgr!FltpCreate+0x205 (FPO: [Non-Fpo])
8b4879f4 b4f87063 88b9fedc 89537220 86693de0 nt!IofCallDriver+0x63
WARNING: Stack unwind information not available. Following frames may be wrong.
8b487aa0 b4f871c9 89537220 86693de0 8b487ac8 MySpy+0x1063
8b487ab0 820939c6 89537220 86693de0 851e4b64 MySpy+0x11c9
8b487ac8 8228e195 705c775e 86666dc4 86331d18 nt!IofCallDriver+0x63
8b487b98 8227c5e1 86331d30 00000000 86666d20 nt!IopParseDevice+0xf61
8b487c28 82289b62 00000000 8b487c80 00000040 nt!ObpLookupObjectName+0x5a8
8b487c88 8228f29c 041cf1bc 00000000 820e9201 nt!ObOpenObjectByName+0x13c
8b487cfc 82255077 041cf1f8 00100000 041cf1bc nt!IopCreateFile+0x63b
8b487d44 82099c7a 041cf1f8 00100000 041cf1bc nt!NtOpenFile+0x2a
8b487d44 779b5e74 041cf1f8 00100000 041cf1bc nt!KiFastCallEntry+0x12a (FPO: [0,3] TrapFrame @ 8b487d64)
041cf1f0 00000000 00000000 00000000 00000000 0x779b5e74

Summary: there is deadlock between THREAD 88b9fcb0 and THREAD
84753d78.

Thread 84753d78 requests exclusive access to ERESOUCE 85205ec8 which is owned
by thread 88b9fcb0. However thread 88b9fcb0 is waiting for kernel event
850a5f88 which will be signaled by thread 84753d78 after it gains exclusive
access to ERESOUCE 85205ec8. Hence the deadlock.

Event 850a5f88 has tag Txfp, it is owned by NTFS, it will be set in
Ntfs!TxfInitializeVolume.
{% endhighlight %}

{% highlight bash %}
0: kd> !pool 850a5f88
Pool page 850a5f88 region is Nonpaged pool
 850a5000 size:  c40 previous size:    0  (Allocated)  Devi (Protected)
 850a5c40 size:   30 previous size:  c40  (Allocated)  Ntfn
 850a5c70 size:   48 previous size:   30  (Allocated)  Vadl
*850a5cb8 size:  348 previous size:   48  (Allocated) *Txfp
		Owning component : Unknown (update pooltag.txt)

After some thought, it seems that thread 84753d78 is the system thread who is
responsible of handling volume mount operations
(Ntfs!NtfsCommonFileSystemControl), in 2 steps:  
1)Ntfs!NtfsMountVolume and
2)Ntfs!TxfInitializeVolume .

In step 1, VPB is set up appropriately. The volume can be accessable since VPB
is not null.  The hang is this case occurs because some thread access the volume
after step1 and before step2.

So can I say this is a problem of NTFS?
{% endhighlight %}

{% highlight bash %}
0: kd> uf Ntfs!NtfsCommonFileSystemControl
Ntfs!NtfsCommonFileSystemControl+0x73:
8a6a4872 8d45fc          lea     eax,[ebp-4]
8a6a4875 50              push    eax
8a6a4876 53              push    ebx
8a6a4877 ff7508          push    dword ptr [ebp+8]
8a6a487a e80d90faff      call    Ntfs!NtfsMountVolume (8a64d88c) <<<<<< step 1
8a6a487f 3bc7            cmp     eax,edi
8a6a4881 89450c          mov     dword ptr [ebp+0Ch],eax
8a6a4884 755d            jne     Ntfs!NtfsCommonFileSystemControl+0xe4
(8a6a48e3)
...
Ntfs!NtfsCommonFileSystemControl+0x93:
8a6a4892 56              push    esi
8a6a4893 e8ff60fbff      call    Ntfs!TxfInitializeVolume (8a65a997) <<<<<< step 2
Posting Rules	 
You may not post new threads
You may not post replies
You may not post attachments
You must login to OSR Online AND be a member of the ntfsd list to be able to post.
{% endhighlight %}



Extensions for Debugging Plug and Play Drivers
When you debug Plug and Play drivers, you may find the following debugger extensions useful.


!arbiter
Displays the current system resource arbiters. An arbiter is a piece of code that is exposed by the bus driver that arbitrates requests for resources, and attempts to solve the resource conflicts among the devices connected on that bus.

!cmreslist
Displays the CM_RESOURCE_LIST for the specified device object.

You must know the address of the CM Resource List.

Here is an example:
{% highlight bash %}
kd> !cmreslist 0xe12576e8

CmResourceList at 0xe12576e8  Version 0.0  Interface 0x1  Bus #0
  Entry 0 - Port (0x1) Device Exclusive (0x1)
    Flags (0x01) - PORT_MEMORY PORT_IO
    Range starts at 0x3f8 for 0x8 bytes
  Entry 1 - Interrupt (0x2) Shared (0x3)
    Flags (0x01) - LATCHED
    Level 0x4, Vector 0x4, Affinity 0xffffffff
This shows that the device with this CM resource list is using I/O Range 3F8-3FF and IRQ 4.
{% endhighlight %}

!dcs
This extension is obsolete -- its functionality has been subsumed by !pci. See the !pci 100 example later in this section.

!devext
Displays bus-specific device extension information for a variety of devices.

!devnode
Displays information about a node in the device tree.

Device node 0 (zero) is the root of the device tree.

Here is an example:
{% highlight bash %}
0: kd> !devnode 0xfffffa8003634af0
DevNode 0xfffffa8003634af0 for PDO 0xfffffa8003658590
  Parent 0xfffffa8003604010   Sibling 0xfffffa80036508e0   Child 0000000000
  InstancePath is "ROOT\SYSTEM\0000"
  ServiceName is "swenum"
  State = DeviceNodeStarted (0x308)
  Previous State = DeviceNodeEnumerateCompletion (0x30d)
  StateHistory[09] = DeviceNodeEnumerateCompletion (0x30d)
  StateHistory[08] = DeviceNodeEnumeratePending (0x30c)
  StateHistory[07] = DeviceNodeStarted (0x308)
  StateHistory[06] = DeviceNodeStartPostWork (0x307)
  StateHistory[05] = DeviceNodeStartCompletion (0x306)
  StateHistory[04] = DeviceNodeStartPending (0x305)
  ...
  Flags (0x6c000131)  DNF_MADEUP, DNF_ENUMERATED,
                      DNF_IDS_QUERIED, DNF_NO_RESOURCE_REQUIRED,
                      DNF_NO_LOWER_DEVICE_FILTERS, DNF_NO_LOWER_CLASS_FILTERS,
                      DNF_NO_UPPER_DEVICE_FILTERS, DNF_NO_UPPER_CLASS_FILTERS
  UserFlags (0x00000008)  DNUF_NOT_DISABLEABLE
  DisableableDepends = 1 (including self)
!devobj
Displays detailed information about a DEVICE_OBJECT.
{% endhighlight %}

Here is an example:
{% highlight bash %}
kd> !devobj 0xff0d4af0

Device object (ff0d4af0) is for:
 00252d \Driver\PnpManager DriverObject ff0d9030
Current Irp 00000000 RefCount 0 Type 00000004 Flags 00001040AttachedDev ff0b59e0

DevExt ff0d4ba8 DevNode ff0d4a08
Device queue is not busy.
{% endhighlight %}
!drivers
The !drivers command is no longer supported. Please use the lm t n command instead.

!drvobj
Displays detailed information about a DRIVER_OBJECT.

Lists all the device objects created by the specified driver.

Here is an example:

kd> !drvobj serial

Driver object (ff0ba630) is for:
 \Driver\Serial
Driver Extension List: (id , addr)

Device Object list:
ffba3040  ff0b4040  ff0b59e0  ff0b5040
!ecb, !ecd, !ecw
(x86 target computers only) Writes a sequence of values into the PCI configuration space.

ib, iw, id
Reads data from an I/O port.

These three commands are useful for determining whether a certain I/O range is claimed by a device other than the driver being debugged. A byte value of 0xFF at a port indicates that the port is not in use.

!ioreslist
Displays the specified IO_RESOURCE_REQUIREMENTS_LIST.

!irp
Displays information about an IRP.

!irpfind
Displays information about all IRPs currently allocated in the target system, or information about those IRPs whose fields match the specified search criteria.

!pci
(x86 target computers only) Displays the current status of the PCI buses and any devices attached to them. It can also display the PCI configuration space.

The following example displays the devices on the primary bus:

{% highlight bash %}
kd> !pci
PCI Bus 0
00:0  8086:1237.02  Cmd[0106:.mb..s]  Sts[2280:.....]  Device  Host bridge
0d:0  8086:7000.01  Cmd[0007:imb...]  Sts[0280:.....]  Device  ISA bridge
0d:1  8086:7010.00  Cmd[0005:i.b...]  Sts[0280:.....]  Device  IDE controller
0e:0  1011:0021.01  Cmd[0107:imb..s]  Sts[0280:.....]  PciBridge 0->1-1  PCI-PCI
 bridge
10:0  5333:8811.43  Cmd[0023:im.v..]  Sts[0200:.....]  Device  VGA compatible controller
{% endhighlight %}


The following example displays the devices for the secondary bus, with verbose output:

{% highlight bash %}
kd> !pci 1 1

PCI Bus 1
08:0  10b7:5900.00  Cmd[0107:imb..s]  Sts[0200:.....]  Device  Ethernet
      cf8:80014000  IntPin:1  IntLine:f  Rom:fa000000  cis:0  cap:0
      IO[0]:fce1

09:0  9004:8178.00  Cmd[0117:imb..s]  Sts[0280:.....]  Device  SCSI controller
      cf8:80014800  IntPin:1  IntLine:f  Rom:fa000000  cis:0  cap:0
      IO[0]:f801       MEM[1]:f9fff000

0b:0  9004:5800.10  Cmd[0116:.mb..s]  Sts[0200:.....]  Device  SubID:9004:8940
1394 host controller
      cf8:80015800  IntPin:1  IntLine:e  Rom:fa000000  cis:0  cap:0
      MEM[0]:f9ffec00
{% endhighlight %}

The following example displays the PCI configuration space for the SCSI controller (bus 1, device 9, function 0):

{% highlight bash %}
kd> !pci 100 1 9 0
00: 9004    ;VendorID=9004
02: 8178    ;DeviceID=8178
04: 0117    ;Command=SERREnable,MemWriteEnable,BusInitiate,MemSpaceEnable,IOSpac
eEnable
06: 0280    ;Status=FB2BCapable,DEVSELTiming:1
08: 00      ;RevisionID=00
09: 00      ;ProgIF=00 (SCSI bus controller)
0a: 00      ;SubClass=00
0b: 01      ;BaseClass=01 (Mass storage controller)
0c: 08      ;CacheLineSize=Burst8DW
0d: 20      ;LatencyTimer=20
0e: 00      ;HeaderType=00
0f: 00      ;BIST=00
10: 0000f801;BAR0=0000f801
14: f9fff000;BAR1=f9fff000
18: 00000000;BAR2=00000000
1c: 00000000;BAR3=00000000
20: 00000000;BAR4=00000000
24: 00000000;BAR5=00000000
28: 00000000;CBCISPtr=00000000
2c: 0000    ;SubSysVenID=0000
2e: 0000    ;SubSysID=0000
30: fa000000;ROMBAR=fa000000
34: 00000000;Reserved=00000000
38: 00000000;Reserved=00000000
3c: 0f      ;IntLine=0f
3d: 01      ;IntPin=01
3e: 08      ;MinGnt=08
3f: 08      ;MaxLat=08
40: 00001580,00001580,00000000,00000000,00000000,00000000,00000000,00000000
60: 00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000
80: 00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000
a0: 00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000
c0: 00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000
e0: 00000000,00000000,00000000,00000000,00000000,00000000,00000000,00000000
{% endhighlight %}

!pcitree
Displays information about PCI device objects, including child PCI buses and CardBus buses, as well as the devices attached to them.

!pnpevent
Displays the PnP device event queue.

!rellist
Displays a PnP relation list and any related CM_RESOURCE_LIST and IO_RESOURCE_LIST structures.



{% highlight bash %}
kd> s 0xffffe000`00200000 L1600000 'W' 'D' '4' '4'
ffffe000`0027c324  57 44 34 34 f1 40 bb 68-c6 3e 7e 70 64 72 62 64  WD44.@.h.>~pdrbd
ffffe000`0027f414  57 44 34 34 c1 77 bb 68-c6 3e 7e 70 64 72 62 64  WD44.w.h.>~pdrbd
ffffe000`00296604  57 44 34 34 d0 22 10 20-00 d0 ff ff 64 72 62 64  WD44.". ....drbd
ffffe000`00296674  57 44 34 34 70 00 79 00-30 00 00 00 64 72 62 64  WD44p.y.0...drbd
ffffe000`0031b4e4  57 44 34 34 46 72 65 65-42 53 44 20 64 72 62 64  WD44FreeBSD drbd
ffffe000`0031b644  57 44 34 34 00 00 00 00-00 00 00 00 64 72 62 64  WD44........drbd
{% endhighlight %}



{% highlight bash %}
kd> db 0xffffe000`01b33c70+8
ffffe000`01b33c78  00 00 00 00 00 00 00 00-00 00 00 00 00 00 00 00  ................
ffffe000`01b33c88  a0 d8 31 00 00 e0 ff ff-00 00 00 00 00 00 00 00  ..1.............
ffffe000`01b33c98  40 f6 b5 01 00 e0 ff ff-00 00 00 00 00 00 00 00  @...............
ffffe000`01b33ca8  10 a0 10 05 00 e0 ff ff-00 00 00 00 00 00 00 00  ................
ffffe000`01b33cb8  00 fb be 01 00 e0 ff ff-00 00 00 00 00 00 00 00  ................
ffffe000`01b33cc8  00 00 00 00 00 00 00 00-00 00 00 00 00 00 00 00  ................
ffffe000`01b33cd8  00 00 00 00 00 00 00 00-00 00 00 00 00 00 00 00  ................
ffffe000`01b33ce8  a0 d0 3f 00 00 e0 ff ff-00 00 00 00 00 00 00 00  ..?.............
{% endhighlight %}

{% highlight bash %}
kd> !stacks 2 drbd
Proc.Thread  .Thread  Ticks   ThreadState Blocker
                            [fffff8013ed5d3c0 Idle]
                            [ffffe00000089040 System]
*** ERROR: Module load completed but symbols could not be loaded for mt_scflt.sys
*** ERROR: Symbol file could not be found.  Defaulted to export symbols for vmci.sys -
*** ERROR: Symbol file could not be found.  Defaulted to export symbols for vsock.sys -
   4.0000cc  ffffe00001019880 fffffee1 Blocked    nt!KiSwapContext+0x76
                                        nt!KiSwapThread+0x14e
                                        nt!KiCommitThreadWait+0x127
                                        nt!KeWaitForMultipleObjects+0x228
                                        drbd!run_singlethread_workqueue+0x8d
                                        nt!PspSystemThreadStartup+0x58
                                        nt!KiStartSystemThread+0x16
										...
{% endhighlight %}
