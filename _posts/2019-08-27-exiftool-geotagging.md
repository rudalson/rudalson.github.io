---
layout: post
title:  "이미지 파일에 exiftool을 사용하여 GPS정보 geotagging 하기"
categories: etc
tags: exiftool GPS geotagging longitude latitude
---
폰으로 사진을 찍으면 GPS 위치 정보를 기록해주지만 가끔 그것을 놓칠때가 있다. GPS 수신이 힘든 지하나 장소인 경우일 대도 있지만 멀쩡한 곳에서 기록이 남아있지 않을 때가 있다.
느낌상 `아이폰`이 비교적 괜찮은 편이고 `삼성`도 2~3년 전부터는 좋아진거 같다.(S5에서도 GPS를 놓친 사진이 꽤 있다) LG는 현재(2019) `V30` 모델을 사용중인데 가끔 놓친다. `넥서스`는 5, 5X를 사용해봤는데 훌륭했다.
현재는 구하기 힘든 `팬텍`도 과거 종종 놓치는 편이었다.

이건 뭐 그냥 개인의 체감정도일 뿐이지만 해마다 3000~4000 장 정도의 사진을 관리하고 또 GPS 정보를 활용할 때는 아쉬운 부분이기도 한데 결국 GPS tagging이 되지 않은 사진은 별도 작업처리를 해줘야 한다.

GPS 정보를 이미지에 집어넣기 위해 `Python`이랑 `Go` 둘 가지고 각각 관련 라이브러리를 구해서 작성하려는데 둘 다 만족스럽지 못했다. `exif` 에서도 카메라, 제조사, 시기, 버전 등등 의 조건으로 타입이 다양했고 GPS info version도 조금씩 차이가 있는데 이를 다 수용해서 편하게 사용하려면 이분야를 어느 정도는 알아야 될 것 같았다. 거기까지는 하고싶지 않아서 그냥 다른 대안을 찾다가 [ExifTool 11.63](https://www.sno.phy.queensu.ca/~phil/exiftool/)을 사용하였다.

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
꽤나 많은 정보들이 담겨 있다. 이곳에서 GPS 관련 정보들도 찾아 볼 수 있다. 하지만 이렇게 하나하나 살펴보기에는 힘드므로 아래처럼 gps 관련 정보만 나오게 해서도 볼 수 있다.

## GPS

나에게 필요한 것은 `latitude(위도)`, `longitude(경도)` 값이다. latitude, longitude 순으로 하는 것은 `구글맵`에서도 저 위치값 순서대로 나오기 때문이다. 그리고 이와 같이 알아야 할 것으로 기준선을 나타내는 `~ref` 붙은 값들이다. 우리나랑에서는 `North`, `East` 순으로 하면 되는 것 같다.

```bash
exiftool -filename -gpslatituderef -gpslatitude -gpslongituderef -gpslongitude -T -n ./ > out.txt
```

여기서 구한 값을 토대로 gps 정보값을 입력하기 위해서는 아래처럼 해주면 된다. 가령 `남산공원` 위치를 넣고 싶으면 다른 사진으로 부터 가지고 오던지 아니면 구글맵에서 위치를 찍고 거기서 나오는 gps 값을 구해오면 된다.

![서울남산공원]({{ site.url }}/assets/2019/2019-08-27-gps-sample.png)

```bash
exiftool -GPSLatitudeRef=N -GPSLatitude=37.550987 -GPSLongitudeRef=E -GPSLongitude=126.990905 "2015-12-07 20.42.30.jpg"
```


