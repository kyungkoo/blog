---
title: "Using Stacks to Arrange Views"
date: 2021-05-06T00:56:26+09:00
draft: false
---

## [ProgressView](https://developer.apple.com/documentation/swiftui/progressview) 추가하기
[ProgressView](https://developer.apple.com/documentation/swiftui/progressview) 를 추가하는 것 자체는 큰 어려움이 없지만, Xcode12.5 기준으로 `ProgressView(value:total)` 선언 시, `value` 값이 `Int` 이면 canvas 에서 아래와 같은 에러가 발생한다.

```
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

위 문제를 해결하기 위해서는 `value` 에 `5.0` 와 같이 `Float` 타입을 입력해주어야 한다.