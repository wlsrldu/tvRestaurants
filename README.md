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

## 데이터 저장
firebase firestore를 이용하여 데이터 저장



