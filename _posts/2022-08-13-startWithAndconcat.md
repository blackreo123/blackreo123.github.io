---
layout: post
published: true
title:  "startWith and concat"
summary: "how to use startWith and concat"
author: jiha
date: "`r format(Sys.time(), '%Y/%m/%d')`"
category: [swift, rxswift]
tags: swift
keywords: swift, rxswift
usemathjax: false
permalink: /blog/swift-startWith-and-concat/
---
## startWith

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
## startWith result
```swift
---------- startWith ----------
👨🏻teacher
👦🏻child0
🧒🏼child1
👦🏽child2
```

## concat
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
## concat result
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

## concatMap
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
## concatMap result
```swift
---------- concatMap ----------
👦🏻
🧒🏼
👦🏽
👶🏻
👶🏿
```