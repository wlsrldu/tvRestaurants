# tvRestaurants
tv에 나온 맛집 정보 어플리케이션 개발과정 기록
- 데이터 수집 : python3, selenium, google geocoding api, firestore
- iOS : swift


tv맛집 어플리케이션 다운로드 받기 

[iOS Appstore](https://google.com, "appstore link")

[Android PlayStore](https://google.com, "google play store link")

## 데이터 수집
웹 크롤링을 이용해 tv에 나온 맛집정보 수집

## 웹 크롤링 
개발환경 설정
```
Requests, BeautifulSoup4
작업환경 : 작업 환경 : 맥 OS + 크롬 + 파이참
```

pip설치
```
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
sudo python3 get-pip.py
sudo easy_install pip
```

Requests설치
```
pip install requests
```

beautifulsoup4설치
```
pip install beautifulsoup4
```

맥 기본 파이썬 2.7버전 >> 최신버전을 기본으로 설정

```
// 파이썬 설치 
brew install python 
// python3 > python 명령으로 대체 방법 
vi ~/.zprofile (macOS 카탈리나) 
// 추가 내용 
alias python='python3' 
// 소스 적용 
source ~/.zprofile 
// python 실행 
python
```

html 파싱이용하여 수요미식회 정보 취득

soup사용방법
```
soup.select('원하는 정보')  # select('원하는 정보') -->  단 하나만 있더라도, 복수 가능한 형태로 되어있음
soup.select('태그명')
soup.select('.클래스명')
soup.select('상위태그명 > 하위태그명 > 하위태그명')
soup.select('상위태그명.클래스명 > 하위태그명.클래스명')    # 바로 아래의(자식) 태그를 선택시에는 > 기호를 사용
soup.select('상위태그명.클래스명 하~위태그명')              # 아래의(자손) 태그를 선택시에는   띄어쓰기 사용
soup.select('상위태그명 > 바로아래태그명 하~위태그명')     
soup.select('.클래스명')
soup.select('#아이디명')                  # 태그는 여러개에 사용 가능하나 아이디는 한번만 사용 가능함! ==> 선택하기 좋음
soup.select('태그명.클래스명)
soup.select('#아이디명 > 태그명.클래스명)
soup.select('태그명[속성1=값1]')
```

selenium, bs4 사용하여 웹 크롤링하여 맛집정보 수집
```
######################################
    #logic
    SCROLL_PAUSE_TIME = 1.5 
    last_height = driver.execute_script("return document.body.scrollHeight") 
    isLoading = True
    while isLoading:
        for i in range(4): 
            driver.execute_script("window.scrollTo(0, document.body.scrollHeight);") 
            time.sleep(SCROLL_PAUSE_TIME) 
            
            if len(driver.find_elements_by_css_selector("button[class^='btn_more_list']"))>0:
                driver.find_element_by_css_selector("button[class^='btn_more_list']").click()
            else:
                isLoading = False
            
            new_height = driver.execute_script("return document.body.scrollHeight") 
            if new_height == last_height: 
                break 
            last_height = new_height

    ######################################
    #get html
    driver.implicitly_wait(2)
    html = driver.page_source
    soup = BeautifulSoup(html, 'html.parser')
    restaurantList = soup.find('ul', class_='restaurant_list')

    # print(len(restaurantList))
    #print(restaurantList.text)


    ######################################
    #do parse
    dataList = []
    for data in restaurantList:
        modelRestaurant = {}

        titleTag = data.find(attrs={'class': 'box_module_title'})
        stitleTag = data.find(attrs={'class': 'box_module_stitle'})
        imgTag = data.find(attrs={'class': 'box_module_image'})
        
        addressTag = data.select_one('div.mil_inner_spot > span.il_text')
        foodTypeTag = data.select_one('div.mil_inner_kind > span.il_text')
        programTag = data.select_one('div.mil_inner_tv > span.il_text')

        if titleTag is not None:
            modelRestaurant['title'] = titleTag.text
            # print(titleTag.text)

        if stitleTag is not None:
            modelRestaurant['stitle'] = stitleTag.text.strip()
            # print(stitleTag.text.strip())

        if addressTag is not None:
            modelRestaurant['address'] = addressTag.text
            # print(addressTag.text)        

        if foodTypeTag is not None:
            modelRestaurant['foodType'] = foodTypeTag.text

        if programTag is not None:
            modelRestaurant['program'] = programTag.text
            # print(programTag.text)

        if imgTag is not None:
            modelRestaurant['img'] = imgTag['src']
            modelRestaurant['description'] = imgTag['alt']
            # print(imgTag['src'])
            # print(imgTag['alt'])

        if titleTag is not None:    
            dataList.append(modelRestaurant)

```

## Geocoding API를 이용해 위경도 구하기
```
import googlemaps
    for data in dataList:
        gmaps = googlemaps.Client(key='키값')
        geocode_result = gmaps.geocode(data['address'])
        lat = geocode_result[0]['geometry']['location']['lat']
        lng = geocode_result[0]['geometry']['location']['lng']
        data['lat'] = lat
        data['lng'] = lng
```        

## 기존 데이터와 비교하여 신규 추가된 목록만 저장하고 최신 업데이트 내역 갱신

1. firestore 에 저장된 리스트 : saveFirebaseRestaurants
2. 웹 크롤링된 리스트 : loadWebRestaurants
3. saveFirebaseRestaurants 리스트와 loadWebRestaurants 비교하여 신규추가된 내역만 위경도값 조회하여 
4. firestore에 저장
5. 최신 업데이트 내역 관리 : collection(update) - document(2021-01-21) - list:{documentId,documentId,documentId} 

## 데이터 저장
firebase firestore를 이용하여 데이터 저장

## mac mini를 이용해서 하루에 한번씩 웹크롤링 자동화 처리


# iOS 
프로젝트 생성
초기설정
- pod install, config

## 사용라이브러리
```
  $firebase_version = '7.2-M1'
  # Pods for tvRestaurant
  pod 'Firebase/Analytics', $firebase_version
  pod 'Firebase/Messaging', $firebase_version
  pod 'Firebase/Firestore', $firebase_version
  
  pod 'RxSwift'
  pod 'RxCocoa'
  pod 'lottie-ios'
  pod 'UIGradient'
  
  pod 'FSPagerView'
  pod 'Hero'
  pod 'appstore-card-transition'
  pod 'RealmSwift'
```

## 코딩 스타일 가이드
코딩 스타일 가이드를 작성하기 위해 아래 링크들을 참조

스위프트 기본 코드 컨벤션 : https://swift.org/documentation/api-design-guidelines/50
airbnb : https://github.com/airbnb/swift
linkedin : https://github.com/linkedin/swift-style-guide

