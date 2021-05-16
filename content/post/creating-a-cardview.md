---
title: "Creating a Cardview"
date: 2021-05-16T23:11:23+09:00
draft: false
tags:
  - iOS App Dev Tutorials
---
[Creating a Cardview](https://developer.apple.com/tutorials/app-dev-training/creating-a-cardview) 에서는 데이터 모델과 목록 셀에서 사용할 CardView 를 만든다.

<!--more-->


# 시작 전 유의사항
예제에서는 프로젝트에 필요한 `Color` 관련 기능을 `extension` 으로 미리 구현해 놓았다. 그렇기 때문에 이전 예제에서 이어서 하다보면 `Color` 관련 기능 사용시 에러가 발생하기 때문에 사이트에서 제공하는 `Project files` 를 다운받아서 예제를 진행하도록 하자.


# DailyScrum Model
`DailyScrum` 모델은 `struct` 로 생성한다. 또한, preview 에서 사용할 데이터를 `extension` 으로 함께 구현한다.


# CardView
`CardView` 는 `DailyScrum` 정보를 보여주는 뷰다. 구현 자체는 특별한 부분은 없지만, 몇 가지 재미난 부분이 있다.

## `previewLayout`
[`previewLayout`](https://developer.apple.com/documentation/swiftui/view/previewlayout(_:))을 사용하면 프리뷰 화면에 나타나는 컨테이너의 크기를 변경할 수 있다. 기본적으로 프리뷰 화면은 디바이스 전체 화면이 나오지만, 이 메소드를 사용하여 원하는 크기로 변경할 수 있다. 
여기서는 `.fixed(width: 400, height: 60)` 로 설정하였다.
이를 통해 목록 화면을 만들지 않더라도 목록 화면에 나타날 형태를 쉽게 확인할 수 있었다.

## `foregroundColor(scrum.color.accessibleFontColor)`
`accessibleFontColor` 는 `Color+Codable.swift` 에서 확인 할 수 있다. `scrum` 배경색에 기반하여 가독성이 좋은 색상을 반환한다. 이를 화면 전체에 해당하는 `VStack` 에 `foregroundColor` 로 적용하여 모든 텍스트에 일괄 적용되도록 처리하였다. 
단순히 예제만 따라하면 어떤 차이가 있는지 모를 수도 있지만, `preview` 에서 scrum 데이터를 변경하거나 [`background`](https://developer.apple.com/documentation/swiftui/view/background(_:alignment:)) 값을 제거해보면 차이를 쉽게 알 수 있을 것이다.

##  Accessibility
예제 전반적으로 `Accessibility` 를 상당히 강조한다는 것을 알 수 있다.
여기서는 `Label` 의 `Accessibility` 를 재정의하는데, 먼저 [`accessibilityElement(children: .ignore)`](https://developer.apple.com/documentation/swiftui/text/accessibilityelement(children:)) 로 기존 정보는 무시하고, [`accessibilityLabel`](https://developer.apple.com/documentation/swiftui/text/accessibilitylabel(_:)-7wzu8) 와 [`accessibilityValue`](https://developer.apple.com/documentation/swiftui/text/accessibilityvalue(_:)-7vn9b) 를 통해 새로운 이름과 값을 설정 한다.

# 마침
이번 예제에서는 `accessibleFontColor` 를 사용한 가독성 처리가 인상깊었다.
배경색이 가변하는 화면에서는 유용하게 사용할 수 있지 않을까 하는 생각이 들었다.