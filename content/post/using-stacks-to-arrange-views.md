---
title: "Using Stacks to Arrange Views"
date: 2021-05-06T00:56:26+09:00
draft: false
tags:
  - iOS App Dev Tutorials
---

[Using Stacks to Arrange Views](https://developer.apple.com/tutorials/app-dev-training/using-stacks-to-arrange-views) 에서는 프로젝트 생성 한 후, `MeetingView` 화면을 만든다.
<!--more--> 


# 프로젝트 구조
`Scrumdinger` 프로젝트 구조는 다음과 같다.

- Scrumdinger
  - Scrumdinger
    - ScrumdingerApp.swift
    - ContentView.swift
    - Assets.xcassets
    - Info.plist
    - Preview Content
      - Preview Assets.xcassets


# `ContentView.swift` 살펴보기
`ContentView.swift` 에는 두 개의 구조체가 있는데, 하나는 [`View`](https://developer.apple.com/documentation/swiftui/view) 프로토콜을 구현한 `ContentView`고, 다른 하나는 canvas 에 나타내는데 필요한 `ContentView_Previews`다.

SwiftUI 에서 UI를 구현하기 위해서는 [`View`](https://developer.apple.com/documentation/swiftui/view) 프로토콜 구현체가 필요하며, 구현체는 [`body`](https://developer.apple.com/documentation/swiftui/view/body-swift.property) 라는 `computed property` 를 구현해야 한다. 결국 [`body`](https://developer.apple.com/documentation/swiftui/view/body-swift.property)에 UI 에 필요한 모든 구성요소를 추가해 원하는 화면을 만들게 된다.


# `ProgressView` 추가 시 canvas issue
예제 초반에 [ProgressView](https://developer.apple.com/documentation/swiftui/progressview) 를 화면에 추가하게 되는데, 추가하는 것 자체는 큰 어려움이 없지만, Xcode12.5 기준으로 `ProgressView(value:total)` 선언 시, `value` 값이 `Int` 이면 canvas 에서 아래와 같은 에러가 발생한다.

```sh
initializer 'init(value:total:)' requires that 'Int' conform to 'BinaryFloatingPoint'

----------------------------------------

CompileDylibError: Failed to build MettingView.swift

Compiling failed: initializer 'init(value:total:)' requires that 'Int' conform to 'BinaryFloatingPoint'

/Users/kyungkoo/works/SwiftUI-App-Dev/Scrumdinger/Scrumdinger/MettingView.swift:12:17: error: initializer 'init(value:total:)' requires that 'Int' conform to 'BinaryFloatingPoint'
        AnyView(ProgressView(value: __designTimeInteger("#5838.[1].[0].property.[0].[0].arg[0].value", fallback: 5), total: __designTimeInteger("#5838.[1].[0].property.[0].[0].arg[1].value", fallback: 15)))
                ^
SwiftUI.ProgressView:3:12: note: where 'V' = 'Int'
    public init<V>(value: V?, total: V = 1.0) where Label == EmptyView, CurrentValueLabel == EmptyView, V : BinaryFloatingPoint
           ^
==================================
```

재미있는점은 이 에러는 canvas 에서만 발생하고, 실제 빌드 및 시뮬레이터 에서는 정상적으로 동작한다는 점이다.

위 문제를 해결하기 위해 `value` 에는 `5.0` 와 같이 `Float` 타입으로 값을 입력해 주도록 하자.


# 컨포넌트 살펴보기
프로젝트 초반이다보니 여러 컴포넌트를 사용하게 된다.
각 컴포넌트에 대해 간략하게 살펴보자.

## HStack 과 VStack
SwiftUI 에서는 [`HStack`](https://developer.apple.com/documentation/swiftui/hstack) 과 [`VStack`](https://developer.apple.com/documentation/swiftui/vstack) 을 사용하여 컴포넌트를 배치한다. 이름에서 알 수 있듯이 `HStack` 은 가로, `VStack` 은 세로로 컴포넌트를 배치한다.

`UIKit` 에서 constraints 를 사용하여 컴포넌트 간 제약을 정의하여 배치를 결정했던것에 비해 훨씬 편리한 방식으로 배치할 수 있다는 것을 알 수 있다.

개인적으로 Android 의 [`LinearLayout`](https://developer.android.com/guide/topics/ui/layout/linear?hl=ko)이 떠올랐는데 기능도 흡사하다는 생각이 들었다. 
Android 에서는 constraints 개념을 갖는 [`ConstraintLayout`](https://developer.android.com/training/constraint-layout?hl=ko) 이 활발하게 사용되고 있으니 두 플랫폼이 서로 주고 받는게 하나 더 늘었구나 하는 생각도 들었다.


## Text 와 Label
예제에서는 [`Text`](https://developer.apple.com/documentation/swiftui/text) 와 [`Label`](https://developer.apple.com/documentation/swiftui/label) 을 사용하여 문자를 표시한다.

### 1) [`Text`](https://developer.apple.com/documentation/swiftui/text)
[`Text`](https://developer.apple.com/documentation/swiftui/text)는 한 줄 이상의 문자열을 표시할 때 사용한다. 
[`UILabel`](https://developer.apple.com/documentation/uikit/uilabel)을 대신할 컴포넌트라고 할 수 있다.

### 2) [`Label`](https://developer.apple.com/documentation/swiftui/label)
[`Label`](https://developer.apple.com/documentation/swiftui/label)은 텍스트와 이미지를 갖는 컴포넌트다. 기본적으로 <이미지>-<텍스트> 순서로 나타나며, [`SFSymbol`](https://developer.apple.com/sf-symbols/) 을 사용하여 손쉽게 이미지와 텍스트를 가변할 수도 있다.

`Label` 생성자는 텍스트, 이미지 순서로 입력받지만, 화면에는 이미지, 텍스트 순서로 출력된다.
입력 받는 순서대로 출력 되었으면 더 직관적이지 않을까 하는 생각이 든다.


## `Spacer`
[`Spacer`](https://developer.apple.com/documentation/swiftui/spacer)를 사용하면 컴포넌트간 간격을 손쉽게 조절할 수 있다. 말 그대로 공간을 차자하는 컴포넌트다. `UIKit` 에서는 constraints 으로 컴포넌트 간 간격을 설정했다면, `SwiftUI` 에서는 컴포넌트 사이에 `Spacer` 를 추가하여 원하는 간격을 손쉽게 만들어낼 수 있다. 


## `Circle`
[`Circle`](https://developer.apple.com/documentation/swiftui/circle)을 사용하면 손 쉽게 원형 UI 를 구현할 수 있다. 부모 뷰 가운데에 원을 그려준다.


## `Button`
[`Button`](https://developer.apple.com/documentation/swiftui/button)은 액션(클로저)과 `Label` 로 이루어진다. 예제에서는 `Button` 을 다음과 같이 선언했다.

```swift
Button(action: {}) {
  Image(systemName: "forward.fill")
}
```

`Button`은 액션과 `Label` 로 이루어지는데, 예제에서는 `Label` 대신 `Image` 를 선언했음에도 잘 동작한다. 사실, `Button` 의 `Label` 은 위에 언급한 [`Label`](https://developer.apple.com/documentation/swiftui/label) 이 아니라 [`View`](https://developer.apple.com/documentation/swiftui/view)다. 

`Button` 구조체 선언을 보면 아래와 같은데, generic 으로 선언한 타입이 `Label` 이라 [`Label`](https://developer.apple.com/documentation/swiftui/label) 과 혼동을 일으킬 수도 있다.

```swift
public struct Button<Label> : View where Label : View {
  //
}
```

위 선언에서 보다시피 `Label` 은 결국 [`View`](https://developer.apple.com/documentation/swiftui/view) 구현체이기 때문에 SwiftUI 에서 사용하는 모든 컴포넌트를 `Button` 에 추가할 수 있다.
