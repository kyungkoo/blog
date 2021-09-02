---
title: "Displaying Data in a List"
date: 2021-08-25T23:57:24+09:00
draft: false
tags:
  - iOS App Dev Tutorials
---

[Displaying Data in a List](https://developer.apple.com/tutorials/app-dev-training/displaying-data-in-a-list)에서는 `List` 뷰를 사용하여 스크럼 데이터를 출력하고, `ScrumsView` 를 앱의 RootView 로 설정한다.
<!--more-->

# `ScrumsView`

## `List`
[`List`](https://developer.apple.com/documentation/swiftui/list) 는 데이터를 목록화 하여 출력하는 컨테이너 `View` 다.

List 문서를 살펴보면 수 많은 생성자가 있는데, 예제에서는 [`@ViewBuilder`](https://developer.apple.com/documentation/swiftui/viewbuilder) 하나만 받는 [`init(content:)`](https://developer.apple.com/documentation/swiftui/list/init(content:)) 생성자를 사용한다.


```swift
@available(iOS 13.0, macOS 10.15, tvOS 13.0, watchOS 6.0, *)
extension List where SelectionValue == Never {

    /// Creates a list with the given content.
    ///
    /// - Parameter content: The content of the list.
    public init(@ViewBuilder content: () -> Content)

/// 이하 생략
}
```

## `ForEach`
[`ForEach`](https://developer.apple.com/documentation/swiftui/foreach) 는 식별 가능한 데이터의 컬랙션을 사용하여 뷰를 생성하는 `구조체`다. 예제에서는 `List` 의 클로저에서 `ForEach` 를 사용하여 `CardView` 를 생성한다.

`ForEach` 를 사용할 때 중요한점은 데이터가 **식별가능** 해야 한다는 점인데, 예제 에서는 처음에는 `DailyScrum` 의 `title` 프로퍼티를 사용하여 데이터를 식별하도록 했고, 이어서 `DailyScrum` 에 [`Identifiable`](https://developer.apple.com/documentation/swift/identifiable) 을 구현하여 `id` 값으로 데이터를 식별하도록 변경했다.


# `Make Scrums Identifiable`


## `Identifiable`
[`Identifiable`](https://developer.apple.com/documentation/swift/identifiable)
`ForEach` 로 전달하는 컬랙션 내부 데이터는 **식별가능** 해야 한다. 이를 위해 내부에서는 `Identifiable` 프로토콜을 사용하여 유니크 값 여부를 판별한다.
예제에서는 id 타입으로 `UUID` 를 사용하였으나 다른 객체와 구분이 가능한 값이면 id 타입으로 사용 가능하다.

## `WindowGroup`

예제에서는 ScrumsView 를 root view 로 설정하기 위해 [`WindowGroup`](https://developer.apple.com/documentation/swiftui/windowgroup) 에 ScrumsView 를 추가했다. 

아래는 SwiftUI 프로젝트의 엔트리 포인트다. `App` 과 `Scene` 그리고 `WindowGroup` 으로 구성된 것을 확인할 수 있다. 이들 관계는 [App essentials in SwiftUI](https://developer.apple.com/videos/play/wwdc2020/10037/) 를 참고하자.
 
```swift
@main
struct MyApp: App {
    var body: some Scene {
        WindowGroup {
          // root view 설정
        }
    }
}
```


# 더 알아보기
- List 의 closure 에서 ForEach 가 동작하는 방식
- ForEach 를 사용하지 않고 List 만 사용했을 경우 차이점
- `App`, `Scene`, `WindowGroup` 관계