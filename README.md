# test

![Build Status](https://img.shields.io/badge/version-v0.3.0-orange) ![Build Status](https://img.shields.io/badge/I_love-socar-blue)

# 뚜벅이의 지하철 생활
```
네이버 맵을 통해 서울숲 근방의 지하철역들을 파악합니다
```
## Libraries
```Retrofit2``` ```Okhttp3``` ```NaverMap``` ```Coroutine``` ```RxBinding (throttleLast)```
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

1. 안드로이드 API Level 23 이상을 지원
    - ```minSdk 23```

2. UTF-8, JSON 형식의 서울의 지하철역 중 일부의 위·경도 좌표, HTTP 통신
    - 간단한 RestAPI Server [서비스](https://my-json-server.typicode.com) 이용 
    - (https://my-json-server.typicode.com/y-road/subway-coordinate/stations)

3. 갓-디자이너 님의 디자인 
    - UI 구조 : 1개의 액티비티

4. 서울숲 역을 중심으로 4km 이내에 있는 역들을 표시
    - 두 좌표간 거리를 구해 서울숲 역으로부터 4km 내 역들을 표시 ```Utils.kt\getDistance()```

5. 지도는 구글 또는 네이버의 안드로이드 지도 라이브러리를 사용
    - 최근 국내 대표적인 서비스들에서 자주 사용되는 [NaverMap](https://www.ncloud.com/product/applicationService/maps) 선택

6. 지도 제공자 라이선스 표시만 남겨두고 모두 숨김
    - ```kotlin
        isZoomControlEnabled = false
        isCompassEnabled = false
        isLocationButtonEnabled = false
        isScaleBarEnabled = false
      ```

7. 대상이 되는 역들을 모두 표현하는 적당한 범위에서, 드래그, 확대, 축소
    - 초기에 대상 역을 모두 표시하기 위해 줌레벨 조절 및 필요 이상의 줌 레벨 제한
    
8. 환승역은 10dp 노란 원, 환승역이 아니면 10dp 검은 원
    - 역 정보를 ```HashMap``` 형태의 자료구조에 담고 ```count```를 통해 동일한 역이름이 2개 이상 존재하면 환승역으로 판단
    
9. 출발역과 도착역 지정 관련 전체적인 기능
    - 뷰모델에 3개의 변수(사용자가 선택한 역, 출발역, 도착역)에 대해 ```MutableLiveData``` 지정하여 요구 기능 구현

10. 유저가 백 버튼을 눌러 액티비티를 종료할 때는, 선택 상태를 보존하지 않음
    - ```kotlin
        override fun onBackPressed() {
            super.onBackPressed()
            finish()
        }
      ```

11. 화면 아래 출발역과 도착역 관련 전체적인 기능
    - 출발역과 도착역에 대한 ```MutableLiveData```를 지정하여 View 이벤트 처리
    - 중복 터치 이벤트 방지를 위해 ```throttleLast``` 사용
      

## 김뚜벅씨가 만들고 싶은 앱

1. 적절한 커밋 메시지와 주석
    - ```Git-flow```에 따른 코드 관리, ```Git Convention```에 커밋 관리, ```Semantic Versioning```에 따른 태그 관리

2. 잘 알려진 코드 아키텍쳐의 개념과 취지에 맞게 코드를 구성
    - ```MVVM 패턴```

3. Rx 라이브러리를 사용
    - 출발역과 도착역 터치시 지도의 중심 이동: ```RXBinding``` 사용, ```throttleLast```를 이용해 중복 터치 이벤트 방지

4. 네트워킹과 지도 초기화, UI 반응 등이 적절한 스레드에서 비동기적으로 작동
    - Map에 표시할 Marker 초기화시 ```Coroutine``` 활용

5. 안드로이드 컴포넌트들의 생명 주기 관리에 따라, 여러 유저 시나리오에서 UI가 안정적으로 작동
    - 액티비티의 onCreate() 부분에서 뷰모델 메모리에 있는 데이터를 참조하는 방식으로 액티비티 재시작시에도 일관된 UI 유지

6. 시스템 설정에서 배율, 로케일, 화면 방향 등을 조절해도 디자인을 적절하게 반영
    - 화면 회전시 데이터 유지, 로케일 변경(한국어, 영어), 최소한의 다크 모드 지원

7. 네트워크 에러 등의 문제 상황에 대해 적절한 조처
    - ```Okhttp3```와 ```Retrofit2```를 활용한 네트워크 에러 대응

8. 좌표 데이터를 받아오는 네트워킹 코드를 짜지만, 아직 실제로 원격에서 받아오지는 않고, mock 해서 로컬에서 가져오기
    - 앱 실행 전 ```res\raw\station_coordinate.json``` 을 활용한 mock ```SubwayApplication.kt```
