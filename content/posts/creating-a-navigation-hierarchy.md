---
title: "Creating a Navigation Hierarchy"
date: 2021-09-02T23:54:13+09:00
draft: false
tags:
  - iOS App Dev Tutorials
---

[Creating a Navigation Hierarchy](https://developer.apple.com/tutorials/app-dev-training/creating-a-navigation-hierarchy)에서는 [`NavigationView`](https://developer.apple.com/documentation/swiftui/navigationview) 와 [`NavigationLink`](https://developer.apple.com/documentation/swiftui/navigationlink) 를 사용하여 SwiftUI 에서 화면간 이동 방법에 대해 알아본다.
<!--more-->

# Set Up Navigation
[`NavigationView`](https://developer.apple.com/documentation/swiftui/navigationview)는 UIKit 의  `UINavigationController` 의 기능을 제공한다. `NavigationView` 의 컨텐츠로 뷰를 지정하면 해당 뷰는 Navigation 의 root 가 된다. 예제에서는 `ScrumsView` 를 root 로 설정했다.
<br/>
`ScrumsView` 에서는 `List` 아이템을 `NavigationLink` 로 감싸고, Navigation 에 title 과 barItem 을 추가한다. title 과 barItem 을 NavigationView 가 아닌 `ScrumsView` 에서 지정한다는 것에 주의하자.

[`NavigationLink`](https://developer.apple.com/documentation/swiftui/navigationlink)는 `UINavigationController` 에서 ViewController를 push 하는 역할을 담당한다. 예제에서는 아래 생성자를 사용하여 NavigationLink 를 생성했다. 여기서 destination 는 link 를 선택했을때 보여주어야 하는 뷰를, label 은 destination 을 설명할 뷰를 의미한다. 예제에서는 List 에 나타나는 뷰를 label 로 지정했다.

```swift
public init(destination: Destination, @ViewBuilder label: () -> Label
```

# Create the Detail View, Add Visual Components to the Detail View
다음으로 `ScrumsView` List 로 나타나는 항목을 선택 시 나타날 뷰를 만들어본다.
DetailView 에서는 Grouped Style의 UITableView 와 유사한 화면을 만들게 된다.
`List` 내부에서 `Section` 별로 그룹을 만들고 `.listStyle(InsetGroupedListStyle())` 을 추가하면 손쉽게 Grouped Style 을 구현할 수 있다.