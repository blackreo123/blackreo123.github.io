---
layout: post
title:  "startWith and concat"
summary: "how to use startWith and concat"
author: jiha
category: [swift, rxswift]
tags: swift
keywords: swift, rxswift
usemathjax: false
permalink: /blog/swift-startWith-and-concat/
---
<h2>startWith</h2>

```swift
print("---------- startWith ----------")

let childGroup = Observable<String>.of("👦🏻", "🧒🏼", "👦🏽")

childGroup
    .enumerated()
    .map({ index, element in
        return element + "child" + "\(index)"
    })
    .startWith("👨🏻teacher") // String type
    .subscribe(onNext: {
        print($0)
    })
    .disposed(by: disposeBag)
```
<h2>startWith result</h2>
```swift
---------- startWith ----------
👨🏻teacher
👦🏻child0
🧒🏼child1
👦🏽child2
```

<h2>concat</h2>
```swift
print("---------- concat1 ----------")
let childGroup2 = Observable<String>.of("👦🏻", "🧒🏼", "👦🏽")
let teacher = Observable<String>.of("👨🏻")
let lineUp = Observable
    .concat([teacher, childGroup2])

lineUp
    .subscribe(onNext: {
        print($0)
    })
    .disposed(by: disposeBag)

print("---------- same result concat1 ----------")
teacher
    .concat(childGroup2)
    .subscribe(onNext: {
        print($0)
    })
    .disposed(by: disposeBag)
```
<h2>concat result</h2>
```swift
---------- concat1 ----------
👨🏻
👦🏻
🧒🏼
👦🏽
---------- same result concat1 ----------
👨🏻
👦🏻
🧒🏼
👦🏽

```

<h2>concatMap</h2>
```swift
print("---------- concatMap ----------")
let careCenter: [String: Observable<String>] = [
    "childGroup": Observable<String>.of("👦🏻", "🧒🏼", "👦🏽"),
    "babyGroup": Observable<String>.of("👶🏻", "👶🏿")
]

Observable<String>.of("childGroup", "babyGroup")
    .concatMap { group in
        careCenter[group] ?? .empty()
    }
    .subscribe(onNext: {
        print($0)
    })
    .disposed(by: disposeBag)
```
<h2>concatMap result</h2>
```swift
---------- concatMap ----------
👦🏻
🧒🏼
👦🏽
👶🏻
👶🏿
```