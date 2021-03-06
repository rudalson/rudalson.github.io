---
layout: post
title:  "이미지 혹은 동영상 파일에 exiftool을 사용하여 메타정보 수정하기"
categories: etc
tags: exiftool GPS geotagging longitude latitude exif
---
폰으로 사진을 찍으면 GPS 위치 정보를 기록해주지만 가끔 그것을 놓칠때가 있다. GPS 수신이 힘든 지하나 장소인 경우일 대도 있지만 멀쩡한 곳에서 기록이 남아있지 않을 때가 있다. 느낌상 `아이폰`이 비교적 괜찮은 편이고 `삼성`도 2~3년 전부터는 좋아진거 같다.(나에겐 `갤럭시S5`에서 GPS를 놓친 사진이 꽤 있다) LG는 현재(2019) `V30` 모델을 사용중인데 가끔 놓친다. `넥서스`는 5, 5X를 사용해봤는데 훌륭했다. 현재는 구하기 힘든 `팬텍`도 과거 종종 놓치는 편이었다.

이건 뭐 그냥 개인의 체감일 뿐이지만 해마다 3000~4000 장 정도의 사진을 관리하고 또 메타정보를 활용할 때는 아쉬운 부분이기도 한데 결국 GPS tagging이 되지 않은 사진은 별도 작업처리를 해줘야 한다.

GPS 정보를 이미지에 집어넣기 위해 `Python`이랑 `Go` 둘 가지고 각각 관련 라이브러리를 구해서 작성하려는데 둘 다 만족스럽지 못했다. `exif` 에서도 카메라, 제조사, 시기, 버전 등등 의 조건으로 타입이 다양했고 GPS info version도 조금씩 차이가 있는데 이를 다 수용해서 편하게 사용하려면 이분야를 어느 정도는 알아야 될 것 같았다. 거기까지는 하고싶지 않아서 그냥 다른 대안을 찾다가 [ExifTool 11.63](https://www.sno.phy.queensu.ca/~phil/exiftool/)을 사용하였다.

그리고 `카톡`으로 원본이 아닌 사진을 받은 경우도 꽤 있는데 여기에 촬영시각 같은 정보도 넣어줄 수 있다.

이런 내용들을 정리해보겠다.

# ExifTool
help를 보는데 양이 어마어마하고 눈에 잘 들어오지는 않았다. 그래서 사용했던 것 몇개만 기록에 남겨본다.

기본적으로 한 파일에 대한 exif 정보를 보기 위해서는
```bash
exiftool -l "2015-12-07 20.42.30.jpg"
ExifTool Version Number
      11.63
File Name
      2015-12-07 20.42.30.jpg
Directory
      .
File Size
      4.8 MB
...
Make
      SAMSUNG
Camera Model Name
      SM-G906K
...
```
꽤나 많은 정보들이 담겨 있다. 이곳에서 GPS 관련 정보들도 찾아 볼 수 있다. 심지어 `동영상` 파일에도 위치정보는 있다. 하지만 이렇게 하나하나 살펴보면서 작업하기에는 힘드므로 아래처럼 gps 관련 정보만 나오게 해서 볼 수 있다.

## GPS

나에게 필요한 것은 `latitude(위도)`, `longitude(경도)` 값이다. latitude, longitude 순으로 하는 것은 `구글맵`에서도 저 위치값 순서대로 나오기 때문이다. 그리고 이와 같이 알아야 할 것으로 기준선을 나타내는 `~ref` 붙은 값들이다. 우리나랑에서는 `North`, `East` 순으로 하면 되는 것 같다.

```bash
exiftool -filename -gpslatituderef -gpslatitude -gpslongituderef -gpslongitude -T -n ./ > out.txt
```
* -T : Output in tabular format
* -n : No print conversion

여기서 구한 값을 토대로 gps 정보값을 입력하기 위해서는 아래처럼 해주면 된다. 다른 사진에서 가져오기 힘든 경우라면 `구급맵` 같은 곳에서 직접 구해도 된다.
가령 `남산공원`의 위치를 넣고 싶으면 아래그림처럼 구급맵에서 위치를 찍고 거기서 나오는 gps 값을 구해오면 된다.

![서울남산공원]({{ site.url }}/assets/2019/2019-08-27-gps-sample.png)

```bash
exiftool -GPSLatitudeRef=N -GPSLatitude=37.550987 -GPSLongitudeRef=E -GPSLongitude=126.990905 "2015-12-07 20.42.30.jpg"
```
예에서는 한개의 `2015-12-07 20.42.30.jpg` 파일만 적용하였지만 `와일드문자`를 적용하면 여러 파일을 한번에 수정할 수 있다.

## 촬영시각

기본적으로 `dateTimeOriginal` 옵션값으로 date, time 순으로 문자열을 입력하면 된다.

```
exiftool -xmp:dateTimeOriginal="2015:11:05 18:38:54" *.jpg
```
이런 방식은 입력한 값으로 바로 정해지지만 어떤 기준에 + 몇시간 이런식으로 상대적인 시간을 넣는 것도 가능하다. 

그리고 나같은 경우는 `파일명`이 곧 `촬영시각`으로 관리하는 편이다. 하지만 메타정보가 날아간 경우는 짐작한 시간으로 파일명을 만들었다. 한마디로 촬영추정치 시간이다. 이렇게 파일명에 시간정보가 있다면 이것을 바로 `촬영시각` 정보로 입력해 넣을 수 있다.
```
exiftool "-alldates<filename" 2015-08-13*
```
이렇게 하면 `2015-08-13`일로 시작하는 파일들은 자신의 파일명에 `date`, `time` 값으로 촬영시각에 바로 넣어버린다.


ExifTool은 워낙 기능이 많고 지원하는 것 또한 부족함 없다. 개발자가 지속적으로 관리하고 수정사항을 누적시키고 있기 때문에 독보적인 위치에 있지 않을 까 싶다. 감사히 잘 쓰겠습니다.

## 파일명 변경
파일명을 촬영시각순으로 원하는 포맷대로 변경하려면
```
exiftool  "-FileName<$EXIF:DateTimeOriginal" -d "%Y-%m-%d %H.%M.%S%%-c.%%le" .
```

동영상 파일은 EXIF가 아니기 때문에 아래처럼 생성시간 기준으로 해주면 될 것 같다
```
exiftool  --ext jpg "-FileName<CreateDate" -d "%Y-%m-%d %H.%M.%S%%-c.%%le" .
```
--ext는 exclusive, -ext는 inclusive 속성이다.
