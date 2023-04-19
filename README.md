# ios-weather-forecast README


[1. Team Introduce](#-team-)

<!-- [2. 객체의 역할](#-객체의-역할과-책임) -->

[2. 구현 화면](#-구현-화면)

[3. 학습키워드 및 학습내용](#-학습-키워드-및-학습-내용)

[4. STEP별 구현내용](#-step별-구현-내용)

[5. 트러블 슈팅](#-트러블-슈팅)

[6. 질문과 답변](#-질문과-답변)

<br>

## 💫 Team 💫
🏃🏻🏃🏻‍♂️💨 **프로젝트 기간:** `23.03.13` ~ `23.04.14`

|<img src="https://avatars.githubusercontent.com/u/113027391?v=4" width=200>|<img src="https://avatars.githubusercontent.com/u/45708630?v=4" width=200>|
|:---:|:---:|
|[d.o.](https://github.com/doshkor)|[Lust3r](https://github.com/llimental)|

<br>

## 📱 구현 화면
<img src = "https://user-images.githubusercontent.com/45708630/230910433-6e28a900-243b-48bb-b9a6-b3f415bb2f9c.gif" width = 300>

<br>

<!-- ## 🔍 객체의 역할과 책임

| 객체 | 타입 | 역할과 책임 |
| --- | --- | --- |
|Forecast|Struct|5일 예보 DTO|
|Weather|Struct|현재 날씨 예보 DTO|
|LocationManager|Class||
|LocationManagerDelegate|Protocol||
|NetworkManager|Class||
|OpenWeatherURLProtocol|Protocol||
|WeatherCollectionView|Class||
|WeatherCollectionViewCell|Class||
|WeatherCollectionViewHeader|Class||
|ViewController|Class||
|ViewController+Extensions|Extension|| -->

<br>

## 📚 학습 키워드 및 학습 내용
### 1. CodingKey

> 출처: Apple 공식문서 - [CodingKey](https://developer.apple.com/documentation/swift/codingkey)
> 
- A type that can be used as a key for encoding and decoding.
- 프로젝트에서 사용할 프로퍼티의 이름과 JSON 데이터를 통해 넘어오는 데이터 명이 다르기 때문에 이를 맞춰주기 위해 CodingKey를 사용했습니다

### 2. URLSession

> 출처: Apple 공식문서 - [URLSession](https://developer.apple.com/documentation/foundation/urlsession)
> 
- An object that coordinates a group of related, network data transfer tasks.
- URL 주소로부터 데이터를 다운로드하고 엔드포인트에 데이터를 업로드하기 위한 API를 제공
- API를 사용하여 앱이 실행되고 있지 않을 때 또는 iOS에서 앱이 일시 중단된 동안 백그라운드 다운로드를 수행할 수도 있음
- 이번 프로젝트에서는 API 호출 String을 URL로 URLRequest를 만들고, 이를 가지고 URLSession을 사용하여 JSON 데이터를 받는 작업을 진행했습니다

### 3. Core Location

> 출처: Apple 공식문서 - [Core Location](https://developer.apple.com/documentation/corelocation)
> 
- 장치의 지리적 위치, 고도, 방향, 또는 주변 iBeacon 장치에 대한 상대적인 위치를 결정하는 서비스 제공
- Wi-Fi, GPS, Bluetooth, 자력계, 기압계 및 셀룰러 하드웨어를 포함, 장치에서 사용 가능한 모든 구성 요소를 사용하여 데이터 수집
- 이번 프로젝트에서는 이를 통해 시뮬레이터의 설정된 좌표를 불러오고, 그 좌표를 사용하여 geoCoder에 넘기거나 NetworkManager가 callAPI() 메서드를 사용하는데 이용할 수 있도록 했다.

### 4. Geocoder

> 출처: Apple 공식문서 - [CLGeocoder](https://developer.apple.com/documentation/corelocation/clgeocoder)
> 
- 지리적 좌표와 장소 이름 사이를 변환하기 위한 인터페이스
- `Geocoder` 객체는 정방향 또는 역방향 지오코딩 방법을 사용하여 요청을 시작
- 정방향 지오코딩 요청은 사용자가 읽을 수 있는 주소를 사용하여 위도 및 경도를 찾음
- 역방향 지오코딩 요청은 위도 및 경도 값을 가져와 사용자가 읽을 수 있는 주소를 찾음
- 두 유형 모두 `CLPlacemark` 개체를 사용하여 결과 반환

### 5. CLPlacemark

> 출처: Apple 공식문서 - [CLPlacemark](https://developer.apple.com/documentation/corelocation/clplacemark)
> 
- 지리적 좌표에 대한 사용자에게 친숙한 설명. 장소 이름, 주소 및 기타 관련 정보를 포함함
- `CLPlacemark` 객체는 지정된 위도 및 경도에 대한 장소 표시 데이터 저장
- `CLGeocoder` 개체를 사용하여 지리적 좌표를 역방향 지오코딩하면 해당 위치에 대한 설명 정보가 포함된 `CLPlacemark` 개체를 받게 됨
- 장소표시 세부정보를 가져올 때, 다음과 같은 프로퍼티를 사용할 수 있음
    - **thoroughfare**: 장소 표시와 연결된 상세 주소
    - **subThoroughfare**: 장소 표시에 대한 추가 도로 수준 정보
    - **locality**: 장소 표시와 연결된 도시
    - **subLocality**: 장소 표시에 대한 추가 도시 수준 정보
    - **administrativeArea**: 장소 표시와 연결된 시/도
    - **subAdministrativeArea**: 장소 표시에 대한 추가 관리 영역 정보
    - **postalCode**: 장소 표시와 연결된 우편 번호
- 이번 프로젝트에서는 위 항목을 각각 출력해 보았을 때, `administrativeArea`, `locality`, `sublocality`, `thoroughfare`, `subThoroughfare`가 적합하다 생각하여 선별하여 사용했습니다.

    ``` Swift
    geoCoder.reverseGeocodeLocation(location) { placemarks, error in
                guard let placemark = placemarks?.first else { return }
                var address = ""

                if let administrativeArea = placemark.administrativeArea { address += administrativeArea }

                if let locality = placemark.locality { address += " \(locality)" }

                if let subLocality = placemark.subLocality { address += " \(subLocality)" }

                if let thoroughfare = placemark.thoroughfare { address += " \(thoroughfare)" }

                if let subThoroughfare = placemark.subThoroughfare { address += " \(subThoroughfare)" }

                print("address: ", address)
            }
    ```

### 6. CollectionView Header

> 출처: Apple 공식문서 - [ReusableSupplementaryView](https://developer.apple.com/documentation/uikit/uicollectionview/1618068-dequeuereusablesupplementaryview)
> 
- `Header`/`Footer`를 사용하기 위해서는 `collectionView`의 `ReusableSupplementaryView`를 사용해야 했습니다.
- `elementKind`에 `Header`인지 `Footer`인지 정해주고, `identifier`에는 `cell`과 같이 `Header`에 `static let`으로 생성한 `identifer`를 넣어주어 생성했습니다.

</br>

### 7. Refresh Control

> 출처: Apple 공식문서 - [Refresh Control](https://developer.apple.com/documentation/uikit/uirefreshcontrol)
> 
- A standard control that can initiate the refreshing of a scroll view’s contents.
``` swift
collectionView.refreshControl = UIRefreshControl()
collectionView.refreshControl?.addTarget(self, action: #selector(refreshData), for: .valueChanged)
    
@objc func refreshData() {
    locationManager.startUpdatingLocation()
    collectionView.refreshControl?.endRefreshing()
}
```
- `UIRefreshControl` 인스턴스를 `CollectionView`에 넣어준 후, `selector`에서 `locationManager`의 `startUpdatingLocation()` 메서드를 실행함으로써 새 위치 정보를 받아와 `fetchData()`까지 이뤄지도록 했습니다.

</br>

### 8. async/await

> 출처: swift 공식문서 - [Concurrency](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/concurrency/)
> 
- 비동기 함수 또는 비동기 메서드는 실행 도중에 일시 중단될 수 있는 특별한 종류의 함수 또는 메서드입니다. 이는 완료될 때까지 실행되거나 오류가 발생하거나 반환되지 않는 일반적인 동기 함수 및 메서드와 대조됩니다.
- 기존 비동기 코드에서는 `URLSession`을 사용하는 코드와 그것으로부터 받아진 데이터를 활용한 아이콘 불러오기 메서드가 겹쳐 코드가 복잡했지만 `async/await`으로 비동기 함수 처리를 함으로써 간결한 구조를 만들 수 있었습니다.

</br>

### 9. DateFormatter

> 출처: Apple 공식문서 - [DateFormatter](https://developer.apple.com/documentation/foundation/dateformatter)
> 
- 요구사항에 맞게 날짜 표시를 하기 위해서는 날짜 데이터를 `DateFormatter`를 사용하여 변환해주어야 했습니다.
```swift
let dateFormatter = DateFormatter()
dateFormatter.dateFormat = "MM/dd(E) HH시"

let conversionTimeDataToDate = Date(timeIntervalSinceReferenceDate: TimeInterval(forecastData.timeOfData))

cell.timeLabel.text = dateFormatter.string(from: conversionTimeDataToDate)
```
- `dateFormatter` 인스턴스를 생성해준 후, 포맷을 출력하길 원하는 형식으로 맞춰주었습니다.
- 이후 `Date(timeIntervalSinceReferenceDate)`를 통해 날짜 정보를 `Date` 타입으로 변환해주었고, 앞서 맞춰준 `dateFormatter`의 양식으로 변환하여 `timeLabel`에 사용하였습니다.

<br>

## 🔨 STEP별 구현 내용

### STEP 1 모델/네트워킹 타입 구현
- [Open Weather Map에서 제공하는 날씨 API](https://openweathermap.org/)의 데이터 형식을 고려하여 모델 타입을 구현
- 앱에서는 현재 날씨를 보여주는 기능과 5일 예보를 보여주는 기능 모두 갖춤
- API를 통해 전달받은 JSON 데이터를 활용할 수 있도록 모델 타입을 구현
- [Open Weather Map에서 제공하는 날씨 API](https://openweathermap.org/)의 데이터를 실제로 받아올 수 있는 네트워킹 타입을 구현
- 🗝️ **keyword**: CodingKey, URLSession
- [STEP 1 PR 🔗](https://github.com/tasty-code/ios-weather-forecast/pull/5)

### STEP 2 위치정보 확인 및 날씨 API 호출
- 현재 위치의 위도와 경도 확인
- 위도와 경도를 활용해 현재 위치의 주소 확인
- 현재 위치의 날씨와 현재 위치의 5일 예보를 날씨 API를 통해 데이터를 요청하고 받아오는 기능 구현 
- 🗝️ keyword: CoreLocation, URLSession, JSON
- [STEP 2 PR 🔗](https://github.com/tasty-code/ios-weather-forecast/pull/11)

### STEP 3 UI구현
- 앱 UI 구현
    - 오토레이아웃을 활용한 코드 구현
    - CollectionView 활용
    - Configuration 활용
    - 배경 이미지 사용
- 화면 위에서 아래로 끌었다 놓으면 정보 새로고침
- 🗝️ keyword: CollectionView, Refresh Control, DateFormatter, AutoLayout
- [STEP 3 PR 🔗](https://github.com/tasty-code/ios-weather-forecast/pull/17)

<br>

## 🚀 트러블 슈팅

### 1.    API 호출 및 데이터 저장 로직 함수화

- 현재 스텝에서 서로 다른 url에 대하여 호출해야 하고, 추후에 추가적인 url 호출이 필요한 경우를 위해 해당 로직을 함수화하여 사용하고자 하였습니다
    - `dataTask()` 에서 `Result Type`을 통해 결과값을 전달합니다
    - 메서드를 사용하는 곳에서 결과값을 저장하는 로직을 작성할 수 있습니다
- 제네릭 함수 정의시 추론이 되지 않는 경우 Type을 파라미터에 작성하여 오류를 피할 수 있다는 것을 알게 되었습니다

**기존코드**

```swift
private func callAPI() {
    guard let currentWeatherBaseURL = URL(string: currentWeatherKey) else { return }
    let weatherURLRequest = URLRequest(url: currentWeatherBaseURL)
        
    let currentWeatherTask = URLSession.shared.dataTask(with: weatherURLRequest) { data, response, error in
        if let data = data {
            do {
                try self.currentWeather = JSONDecoder().decode(CurrentWeather.self, from: data)
            } catch {
                print(error)
            }
        }
    }
    currentWeatherTask.resume()
}
```

**변경코드**

```swift
func dataTask<T: Decodable>(URLRequest: URLRequest, myType: T.Type, completion: @escaping (Result<T,Error>)->()) {
        let myTask = URLSession.shared.dataTask(with: URLRequest) { data, response, error in
            if let data = data {
                guard let decodedData = self.decode(jsonData: data, type: myType) else { return }
                completion(.success(decodedData))
            } else if let error = error {
                completion(.failure(error))
            }
        }
        myTask.resume()
    }
```

### 2. API 호출 키 은닉화

- 바로 API key를 사용하고 push를 하면 키가 공개되기 때문에 은닉화할 방법을 찾아야 했음.
- gitignore와 다른 방법을 찾아봤으나 이번에 적용한 방법은 plist를 만들고, 해당 파일의 변경사항을 추적하지 않도록 워크트리에서 제외하였습니다.

### 3. 네트워킹 타입 구현

- 학습 후 기능 구현까지는 오래 걸리지 않았으나 공통적으로 사용하는 코드를 분리하고, 별도의 네트워킹 타입을 구현하는 데에서 많은 어려움이 있었습니다.
긴 고민 끝에 URL 처리를 위한 String 작업을 하는 URLProtocol과 URLSession 작업을 하는 NetworkTaskProtocol을 만들었습니다.
그리고 그 둘을 채택하고, 위도 경도 프로퍼티와 모델 인스턴스를 가지고 API를 호출하는 NetworkManager를 만들어 역할을 위임하였습니다.
이를 통해 ViewController는 NetworkManager 인스턴스만을 가지고 있을 수 있게 되었습니다.

### 4. 위치 정보 동의 문구
- 위치 정보 권한을 얻기 위해서는 동의 문구를 작성해야 했습니다. `Always and When In Use`, `When In Use` 두 개를 추가하는 예시만 있길래 전자가 항상 허용과 사용할 때 허용 두 케이스를 다 포함한다 생각하여 하나만 설정해도 되지 않을까 생각했습니다. 하지만 한 개만 했을 때는 아래와 같은 오류(민감한 데이터에 사용 설명 없이 접근하려 한다)가 발생했고, 두 조건이 반드시 포함되어야 한다고 명시되어 있어 그것을 근거로 작성하게 되었습니다.
![](https://i.imgur.com/ZN11pYl.png)

### 5. NotificationCenter
- LocationManager에서 받은 위도, 경도 데이터를 어떻게 `NetworkManager`에서 사용할까 고민하다 `NotificationCenter`를 사용하면 좋겠다고 생각했습니다. 두 객체를 직접 연결하기엔 부담스러웠으나 `Observer`를 통해 전달하는 것은 보다 낫다고 판단했고, 실제로 그렇게 구현하여 `callAPI()` 메서드를 재호출하여 데이터를 받아오는 것까지 성공하였습니다.

    ``` Swift
    init() {
            NotificationCenter.default.addObserver(self, selector: #selector(getCoordinate(notification:)), name: Notification.Name.location, object: nil)
        }

        // MARK: - Helper
        @objc func getCoordinate(notification: Notification) {
            guard let coordinate = notification.userInfo?[NotificationKey.coordinate] as? CLLocationCoordinate2D else { return }
            latitude = coordinate.latitude
            longitude = coordinate.longitude
            callAPI()
        }
    ```
### 6. 시뮬레이터 결과 문제
- 처음에 `Core Location`을 통해 주소를 불러올 때, 둘의 결과가 다른 문제가 있었습니다. d.o.는 주소가 경기도 고양시 에서 끝나 뒤에는 @로 마무리되었고, lust3r는 전체 주소가 다 나오는 등 동일한 결과를 볼 수 없었습니다.
이 문제는 저희 위치정보 정확성을 `kCLLocationAccuracyBest`로 했는데 시뮬레이터에서 각각 `정확한 위치` 옵션을 끔/켬 으로 다르게 했기 때문임을 알게 되어 해결하게 되었습니다.

### 7. errorDescription VS localizedDescription

- `error.localizedDescription`을 사용하기 위해 `LocalizedError` 프로토콜을 채택하고, 구현 요구사항인 `errorDescription: String?` 을 정의하였어요!
- “그렇다면 왜 `error.localizedDescription`은 옵셔널 값 `String?`을 리턴하지 않는가?” 하는 궁금점이 있었어요!
- ⁉️🙄 내가 정의한 속성은 `String?`인데 사용하는데서는 `String` 이 출력되는 이유는 뭐지? 했는데
- ‼️🫢 `errorDescription`과 `localizedDescription`은 다른 것이었다는 것을 알게 되었어요!

    ``` Swift
    public protocol LocalizedError : Error {

        /// A localized message describing what error occurred.
        var errorDescription: String? { get }

        /// A localized message describing the reason for the failure.
        var failureReason: String? { get }

        /// A localized message describing how one might recover from the failure.
        var recoverySuggestion: String? { get }

        /// A localized message providing "help" text if the user requests help.
        var helpAnchor: String? { get }
    }

    extension Error {

        /// Retrieve the localized description for this error.
        public var localizedDescription: String { get }
    }
    ```
    
### 8. `NotificationCenter` VS `Delegate` VS `Completion`

네트워크 통신을 통해 받은 데이터를 객체 간 주고 받을 때 `NotificationCenter`를 사용했었는데요!

`post`, `addObserver`를 데이터 전송이 필요한 코드 사이에 직접 작성하기 때문에 파일이나 컴포넌트로 분리되지 않아요. 그로 인해 코드가 많아질수록 전송한 데이터를 어디서 받는지 확인하기 어려웠어요.

그래서 다른 방식으로 데이터를 전달할 수 있는 방법에 대해 알아보았어요!
- `NotificationCenter`
    - `NotificationCenter` 싱글턴 객체를 통해 이루어진다
    - 앱 내부에서 발생하는 이벤트에 대한 알림을 다수의 객체에 효과적으로 처리할 수 있다.
    - 런타임에 동작하기 때문에 컴파일 타임에 확인할 수 없는 예상하지 못한 문제점이 발생할 수 있다
</br>
- `delegate` 
    - `Protocol`을 통해 다른 객체와의 관계를 보다 의미있고 명확하게 전달할 수 있다
    - 구현해야 하는 코드가 많아진다
    - 메모리 누수에 대해 생각해야 한다
    - 하나의 객체가 여러 개의 `delegate`를 가질 수 없다
</br>
- `closure`
    - 다른 객체와의 의존성을 최소화할 수 있다
    - 객체와의 관계에 대한 이해를 코드를 통해 이해하기 어렵다

결론!
Location 데이터를 `delegate` 방식을 통해 dataTask 처리를 하였어요
또한 Network 요청 후 받은 데이터처리는 `closure를` 통해 전달하도록 구현해보았어요

</br>

### 9. Async/Await을 통한 동시성 프로그래밍
- 기존에는 Escaping Closure를 통해 weatherAPI, forecastAPI를 호출한 값과 그로부터 얻은 아이콘 정보를 가지고 이미지를 가져왔어요. 그러나 해당 방식은 여러 작업 간의 시점을 맞추기가 어렵고, 코드 들여쓰기가 많아져 가독성이 떨어지는 불편함이 있었어요.
- 이를 해결하기 위해 Async/Await을 사용하여 로직을 변경해 보았고, 기존 대비 더 효율적으로 코드를 작성할 수 있었어요.
- **기존 코드**
    ```swift
    private func callWeatherAPI()  {
        do {
            let weatherURLString = weatherURL(lat: latitude, lon: longitude)
            let weatherURL = try getURL(string: weatherURLString)
            var weatherURLRequest = URLRequest(url: weatherURL)
            weatherURLRequest.httpMethod = "GET"
            dataTask(URLRequest: weatherURLRequest, myType: Weather.self) { result in
                switch result {
                case .success(let data):
                    self.weatherData = data
                    print("(fetched)weatherData")
                case .failure(let error):
                    print("dataTask error: ", error)
                }
            }
        } catch {
            print(error.localizedDescription)
        }
    }
    ```
- **변경 코드**
    ```swift
    func callWeatherAPIConcurrency(latitude: Double, longitude: Double) async throws -> Weather? {
        let weatherURLString = weatherURL(lat: latitude, lon: longitude)
        let weatherURL = try getURL(string: weatherURLString)
        var weatherURLRequest = URLRequest(url: weatherURL)

        weatherURLRequest.httpMethod = "GET"

        let (data, _) = try await URLSession.shared.data(for: weatherURLRequest)
        let weather = try JSONDecoder().decode(Weather.self, from: data)
        print("[NetworkManager](fetched)weather")
        return weather
    }
    ```
- **ViewController+Extensions.swift**
    ```swift
    extension ViewController: LocationManagerDelegate {
        func fetchData() {
            guard let coordinate = locationManager.getCoordinate() else { return }
            Task {
                weather = try await networkManager.callWeatherAPIConcurrency(latitude: coordinate.latitude, longitude: coordinate.longitude)
                forecast = try await networkManager.callForecastAPIConcurrency(latitude: coordinate.latitude, longitude: coordinate.longitude)

                guard let weatherIconString = weather?.weather.first?.icon else { return }
                weatherIcon = try await networkManager.getWeatherIconCuncurrency(weatherStatus: weatherIconString)

                guard let forecastList = forecast?.list else { return }
                forecastIcons = try await networkManager.getForecastIconCuncurrency(forecastList: forecastList)
            }
        }
    }
    ```

<br>

## 🙋🏻 질문과 답변
### 1.  프로그램의 구조에 대한 질문

```swift

 ── WeatherForecast
    ├── AppDelegate.swift
    ├── Info.plist
    ├── Model
    │   ├── Forecast.swift
    │   └── Weather.swift
    ├── Network
    │   ├── NetworkManager.swift
    │   └── Protocol
    │       ├── NetworkTaskProtocol.swift
    │       └── OpenWeatherURLProtocol.swift
    ├── SceneDelegate.swift
    ├── ViewController.swift
    ├── WeatherForecast++Bundle.swift
    └── WeatherInfo.plist
```

- 기존에 ViewController 에 `callAPI()` 를 포함한 모든 저장속성과 메서드가 존재하였는데요
- `NetworkTaskProtocol` : dataTask 로직을 분리
- `OpenWeatherURLProtocol` : url 값 관리 및 URLRequest 에 필요한 URL 제공
- `NetworkManager` : 위의 두 개의 프로토콜을 채택하며, 네트워크 모델 타입의 저장속성을 가집니다. 네트워크 요청 및 데이터 저장의 역할을 가집니다.

ViewController에서 `var netWorkManager: NetworkManager()`  형태로 데이터 통신의 역할을 가지는 객체로 구현하였습니다. ViewController에 코드가 최소한을 존재하도록 역할과 책임을 나누고 고민하였는데요! 혹시 이렇게 구현한 방식이 자연스러운지?? 어색한 부분이 있다면 어떤 부분일까요?? 소대가 보시기에 어떠실까요?

> **리뷰어 소대의 의견**
    >
    > `ViewController`에서 모든 역할을 수행하는 것보다 지금과 같이 구현한 것이 더 자연스럽다고 생각합니다.👍 

### 2. 모델 구현에 대한 질문

- 처음에 API 데이터를 가지고 어떤 값이 반환되는지 확인한 후 모델을 작성했는데, 커밋을 진행하면서 종종 형식 오류가 발생했습니다.
파싱된 데이터를 보니 처음에 확인했던 형식과는 차이점이 있어서 다시 모델을 작성하였습니다.
그런데 이처럼 API 파싱 데이터가 바뀌게 되면 지금처럼 작은 프로젝트에서는 모델을 바꿔도 큰 영향이 없지만 큰 프로젝트에서는 모델이 변경되면 많은 부분에 영향을 미칠 것이라 생각됩니다.
현업에서는 이렇게 API를 사용할 때 모델을 어떻게 구현하시는지, 그리고 변경사항이 있을 때 어떻게 처리하시는지 궁금합니다!

> **리뷰어 소대의 의견**
    >
    > 모델은 보통 서버에서 작성, 사용하는 API 문서에 따라 구현합니다. 만약 모델 변경이 생길 경우, 모바일에서도 코드 수정이 불가피할 수 있습니다. 이런 부분은 초기 설계와 커뮤니케이션이 하나의 방법이 될 것 같습니다!

### 3. 시뮬레이터 결과가 다르게 나와요!
- 둘 다 같은 코드를 pull 받아 사용했음에도 시뮬레이터가 서로 다른 결과를 보여줬습니다. 앞서 고민과 해결 4번에서처럼 실행한 환경이 달라서 그런 것이면 해결이 되었을텐데, 똑같이 실행했는데도 문제가 발생했습니다.
가령 마지막 커밋 기준으로 lust3r의 시뮬레이터는 address 출력이 한 번, weather/forecast 요청이 각 2번(초기 호출 + 옵저버 호출)이뤄지는데
d.o.의 시뮬레이터는 address 출력이 두 번, weather/forecast 요청이 각 3번(초기 호출 + 옵저버 호출 + ?) 이뤄졌습니다.
시뮬레이터에 설치된 프로젝트 앱을 삭제도 해보고 이것저것 시도를 해봤는데도 해결이 되지 않았습니다.

> **리뷰어 소대의 의견**
> 
> Core Location 을 사용해 주소를 가져오는 로직상에서는 문제는 없어보입니다.
코드 상에서 로케이션이 업데이트되면 날씨 API를 호출하게 되는데 d.o. 의 시뮬레이터에서 address 가 두번 출력되는 것으로 보아 옵저버 호출이 두번 이루어져 날씨 API도 두번 호출되지 않았을까 싶습니다. 시뮬레이터에서 Location 설정이 동일한지 다시 한번 확인이 필요할 것 같습니다!

### 4. 주소가 잘 나오지 않아요!
- `geoCoder`의 `reverseGeocodeLocation`을 사용했을 때, 그 하위요소로 주소를 표시할 수 있었습니다. 그런데 이전에는 주소가 잘 표현된 것으로 보이는데 지금은 도로명 주소로 바뀌어서인지, 아니면 버전이 올라가면서 변환 방식이 변경된 것인지 주소가 정확히 표시되지 않았습니다.
가령 저희 새싹 건물을 기준으로 코드는 '서울특별시 용산구 한강로3가 청파로 109'를 보여줘야 하는데, 실제 결과는 '서울특별시 서울특별시 한강로3가 청파로 109'를 보여줍니다. 애초에 데이터가 저렇게 들어왔나 싶어 `Placemark`, `description`을 출력해보니 위도 경도를 통해 들어온 데이터는 용산구가 들어가 있었습니다.
해결해보고자 `placemark`의 모든 정보를 출력해보고 메서드를 찾아보았지만 '용산구'를 끌어낼 수 없었습니다. 혹시 이걸 표시하게 할 수 있는 방법이 있을까요?

  ![](https://i.imgur.com/MrHBqDi.png)
  
> **리뷰어 소대의 의견**
> 
> 해당 문제는 애플 사용하는 지도 데이터에 문제(도로명, 지번 등)가 있지 않을까 싶습니다. (시뮬레이터에 따라 다르게 동작하는 부분을 확인할 수 있었습니다.) 실제 기기에서도 같은 결과가 출력되는지 확인이 필요할 것 같습니다!

### 5. ViewController의 역할
- `weather`, `forecast` 인스턴스 말고도, iconData를 `collectionView`에서 사용해야 하다보니, `ViewController`에 아이콘 데이터를 갖는 인스턴스를 두 개 생성하였습니다(`weatherIcon`, `forecastIcons`). 이뿐만 아니라 각각은 데이터가 바뀌면 `collectionView`에 반영하기 위해 `collectionView reload`를 didset에서 하게 되는데 그러다보니 `ViewController`에서 하는게 많고 각각이 `reloadData()`를 수행하니 무겁다고도 느껴집니다. 그렇다고 분리하자니 작업이 이뤄지는 곳이 `ViewController`이기 때문에 고민이 됩니다. 혹시 이러한 구조 개선에 대한 소대의 의견 받을 수 있을까요?

> **리뷰어 소대의 의견**
> 
> 매번 reloadData() 로 셀을 갱신하면 무겁다고 생각이 들 수 있을 것 같습니다. 이에 대해서 `reloadItems(at: [IndexPath])` 과 같은 메서드를 사용하여 업데이트가 필요한 부분만 사용하는 것이 좋을 수 있을 것 같습니다. 두 방법 다 장단점이 있어 확인해보면 좋을 것 같습니다!
아래 링크의 더 효율적이고 개선된 방식으로 컬렉션 뷰를 구현하는 것도 방법이 될 것 같습니다!
[Implementing Modern Collection Views](https://developer.apple.com/documentation/uikit/views_and_controls/collection_views/implementing_modern_collection_views)

</br>

### 6. 비동기 작업의 종료 시점 맞추기
- `ViewController+Extensions.swift` 에서 `fetchData()` 메서드를 통해 각 비동기 작업을 `Task`로 묶어 호출을 했고, 이렇게 받아진 데이터를 `CollectionView`의 각 항목을 구성하는데에 사용해봤어요. 그런데 실제로 실행해보면 첫 번째 실행에서는 일반적인 `weather`, `forecast` 데이터만 먼저 출력이 되고, `Header`와 `cell`의 `icon data`는 살짝 늦게 출력이 됨을 알 수 있었어요. 두 번째 부터는 앱에 대한 로딩이 빨라져서 그런지 거의 동시에 출력되긴 하지만 첫 실행에서도 동일하게 보여주기 위해서는 어떤 작업이 더 필요할 지 여쭤보고 싶어요!

> **리뷰어 소대의 의견**
> 
> Swift Concurrency 동작 원리에 대해 이해가 더 필요할 것 같습니다. Task 내 await 구문이 여러 개가 존재할 경우 어떻게 동작하는지 알아보면 질문과 같은 현상을 더 잘 이해할 수 있지 않을까 싶습니다!

</br>

### 7. LocationManager 권한 문제
```swift
func locationManagerDidChangeAuthorization(_ manager: CLLocationManager) {
    switch manager.authorizationStatus {
    case .authorizedWhenInUse, .authorizedAlways:
        manager.requestLocation()
    default:
        manager.requestWhenInUseAuthorization()
    }
}
```
- 위 코드와 같이 `locationManagerDidChangeAuthorization(_ manager:)` 메서드를 사용하여 `locationManager`의 권한 상태가 사용할 때, 항상 허용이라면 위치정보를 요청하고, 그 외의 경우에는 권한을 요청하는 분기를 해주었어요.
- 하지만 처음 앱을 실행했을 때 권한 요청이 나오고, 여기서 허용을 하지 않으면 다음 번에 앱을 실행했을 때 권한을 요청하지 않았어요.
- 공식문서에서는
`Core Location`은 사용자의 작업이 인증 상태 변경을 초래하고 앱이 `CLLocationManager`의 인스턴스를 만들 때 앱이 포그라운드에서 실행되든 백그라운드에서 실행되든 항상 `locationManagerDidChangeAuthorization(_:)`을 호출합니다.
라고 되어 있어 시뮬레이터 상에서는 위치정보 동의 설정을 바꿀 수 없지만 앱을 껐다가 재실행하면 인스턴스를 만들 때 다시 요청할 것이라 생각했어요. 하지만 프로젝트를 다시 설치하지 않는 한 재요청이 이뤄지지 않아 이에 대해 여쭤보고 싶었어요!

> **리뷰어 소대의 의견**
> 
> iOS 앱 사용권한에 대해 알아보면 좋을 것 같습니다! 일반적으로 사용자 위치 권한은 요청할 당시 1번만 팝업이 출력됩니다. 그 이후에는 앱에 설정된 권한에 따라 동작하게 됩니다.
locationManagerDidChangeAuthorization 메서드의 용도와 authorizationStatus 의 권한 종류들에 대해서 확인해보면 좋을 것 같습니다!
