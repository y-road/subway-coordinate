# test

![Build Status](https://img.shields.io/badge/version-v0.3.0-orange) ![Build Status](https://img.shields.io/badge/I_love-socar-blue)

# 뚜벅이의 지하철 생활
```
네이버 맵을 통해 서울숲 근방의 지하철역들을 파악합니다
```
## Libraries
Retrofit2 | Okhttp3 | NaverMap | Coroutine | RxBinding (throttleLast)
```kotlin
    //Retrofit2
    implementation 'com.squareup.retrofit2:retrofit:2.9.0'
    implementation 'com.squareup.retrofit2:converter-gson:2.9.0'
    //Okhttp3
    implementation 'com.squareup.okhttp3:okhttp:4.9.3'
    implementation 'com.squareup.okhttp3:logging-interceptor:4.8.0'
    //Naver Map
    implementation 'com.naver.maps:map-sdk:3.15.0'
    //Coroutine
    implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-core:1.6.2'
    implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-android:1.6.2'
    //RxBinding
    implementation "com.jakewharton.rxbinding4:rxbinding:4.0.0"
```

## 김뚜벅씨가 구상한 앱의 초안

안드로이드 API Level 23 이상을 지원
 - minSdk 23

UTF-8, JSON 형식의 서울의 지하철역 중 일부의 위·경도 좌표, HTTP 통신
 - 간단한 RestAPI Server 서비스(무료) 이용
 - (https://my-json-server.typicode.com/y-road/subway-coordinate/stations)

갓-디자이너 님의 디자인 👏
 - 1개의 액티비티 구조

서울숲 역을 중심으로 4km 이내에 있는 역들을 표시
 - 두 좌표간 거리를 구해 서울숲 역으로부터 4km 내 역들을 표시 (Utils.kt\getDistance())

지도는 구글 또는 네이버의 안드로이드 지도 라이브러리를 사용
 - 최근 국내 대표적인 서비스들에서 자주 사용되는 Naver Map 선택

지도 제공자 라이선스 표시만 남겨두고 모두 숨김
 - ```kotlin
    isZoomControlEnabled = false
    isCompassEnabled = false
    isLocationButtonEnabled = false
    isScaleBarEnabled = false
```

## 김뚜벅씨가 만들고 싶은 앱
```
 -  Git-flow에 따른 코드, 커밋 Convention에 따른 커밋 메시지, Semantic Versioning에 따른 태그 관리
 - MVVM 패턴
 - 출발역과 도착역의 View 선택시 지도의 중심 이동 - 중복 터치 방지를 위해 throttleLast 사용 (RxBinding)
 - Map에 표시될 Marker 초기화시 Coroutine 활용
 - 화면 회전시 데이터 유지, 로케일 변경(한국어, 영어), 배율 변경 지원
 - Okhttp3와 Retrofit2를 활용한 네트워크 에러 대응
 - 앱 실행 전 res\raw\station_coordinate.json 을 활용한 mock (SubwayApplication.kt)
 - 제공된 aws의 Json의 경우 시간이 경과됨에 따라 파라미터 값 중 'X-Amz-Expires=86400&X-Amz-Signature' 값이 변경되어 지속적인 통신이 불가능해 간단한 RestAPI Server 서비스(무료)를 이용함 (https://my-json-server.typicode.com/y-road/subway-coordinate/stations)
```
