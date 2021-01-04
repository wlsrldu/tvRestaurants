# tvRestaurants
tv에 나온 맛집 정보 어플리케이션 개발과정 기록


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

html 파싱이용하여 수요미식회 정보 취득



## 데이터 저장
firebase firestore를 이용하여 데이터 저장



