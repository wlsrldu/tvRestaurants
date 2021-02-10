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

## 퍼포먼스 향상을 위해 Texture 도입 
기존 storyboard로 작성된 뷰를 Texture Layout API로 변경 
