---
layout: post
title:  "파이썬으로 exif 정보 다루기"
categories: python
tags: GPS geotagging PIL exifread exif
---
exif 정보를 구해오는 3가지 방법

# Using PIL(Python Image Library)

```shell
$ pip3 install image
```

```python
import PIL.Image
 
img1 = PIL.Image.open("roses.jpg")
meta_data = img1._getexif()
 
print(meta_data)
print(img1.height, img1.width)
```

# Using Exifread

```shell
$ pip3 install exifread
```

```python
import exifread
 
with open("roses.jpg", "rb") as f:
    tags = exifread.process_file(f)
    # print(tags)
 
print(tags["Image Model"])
```

# Using Exif

```shell
$ pip3 install exif
```

```python
from exif import Image
 
with open("roses.jpg", "rb") as f:
    img = Image(f)
    # print(img)
 
print(img.has_exif)
print(img.gps_altitude_ref, img.model, img.datetime)
 
print(img.gps_latitude, img.gps_longitude)
 
img.gps_latitude = (37.0, 31.0, 20.0)
img.gps_longitude = (127.0, 6.0, 59.6)
 
print(img.gps_latitude, img.gps_longitude)
 
with open("roses2.jpg", "wb") as new_image_file:
    new_image_file.write(img.get_file())
```

# Reference
* [Youtube - Extracting Metadata From Images and Audio Files with Python](https://www.youtube.com/watch?v=zX81jd569c4)
* [위도(latitude) 경도(longitude) 관련 변환](http://map.ngii.go.kr/ms/mesrInfo/coordinate.do#none)
* [GPS 위도, 경도 도분, 도분초 변환방법](https://m.blog.naver.com/PostView.nhn?blogId=caolympiad&logNo=220855909060&proxyReferer=https%3A%2F%2Fwww.google.co.kr%2F)