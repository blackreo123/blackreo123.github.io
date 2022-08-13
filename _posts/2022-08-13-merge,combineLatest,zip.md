---
layout: post
# published: true
title:  "[rxSwift] merge, combineLatest, zip"
summary: "how to use merge, combineLatest, zip"
author: jiha
category: [swift, rxswift]
tags: swift
keywords: swift, rxswift
thumbnail: /assets/img/posts/swift.png
usemathjax: false
permalink: /blog/swift-merge-combineLatest-zip/
---

## merge

### merge exmple1
```swift
print("---------- merge1 ----------")
let kanto = Observable.from(["東京","千葉","埼玉","神奈川","群馬","栃木","茨城","山梨"])
let tohoku = Observable.from(["青森","秋田","岩手","山形","宮城","福島"])

Observable.of(kanto, tohoku)
    .merge()
    .subscribe(onNext: {
        print($0)
    })
    .disposed(by: disposeBag)
```
### 特徴
1. mergeされたobservableの中で一つでもエラーが発生したら全体エラー
2. 放出される順番はきまってない

### result
```swift
---------- merge1 ----------
東京
千葉
青森
埼玉
秋田
神奈川
岩手
群馬
山形
栃木
宮城
茨城
福島
山梨
```

### merge exmple2
```swift
print("---------- merge2 ----------")
Observable.of(kanto, tohoku)
    .merge(maxConcurrent: 1) // 一回に処理するobervableの数
    .subscribe(onNext: {
        print($0)
    })
    .disposed(by: disposeBag)
```
### result
```swift
---------- merge2 ----------
東京
千葉
埼玉
神奈川
群馬
栃木
茨城
山梨
青森
秋田
岩手
山形
宮城
福島
```

## combineLatest

### combineLatest example1
```swift
print("---------- combineLatest1 ----------")
let firstName = PublishSubject<String>()
let lastName = PublishSubject<String>()

let fullName = Observable
    .combineLatest(firstName, lastName) { firstName, lastName in
        firstName + lastName
    }

fullName
    .subscribe(onNext: {
        print($0)
    })
    .disposed(by: disposeBag)

firstName.onNext("Yoon")
lastName.onNext("jiha")
lastName.onNext("jihye")
firstName.onNext("Bae")
firstName.onNext("Kim")

```
### result
```swift
---------- combineLatest1 ----------
Yoonjiha
Yoonjihye
Baejihye
Kimjihye
```

### combineLatest example2
```swift
print("---------- combineLatest2 ----------")
let firstName = PublishSubject<String>()
let lastName = PublishSubject<String>()

let fullName = Observable
    .combineLatest([firstName, lastName]) { name in
        name.joined(separator: " ")
    }
fullName
    .subscribe(onNext: {
        print($0)
    })
    .disposed(by: disposeBag)

firstName.onNext("Yoon")
lastName.onNext("jiha")
lastName.onNext("jihye")
firstName.onNext("Bae")
firstName.onNext("Kim")
```

### result
```swift
---------- combineLatest2 ----------
Yoon jiha
Yoon jihye
Bae jihye
Kim jihye
```

### combineLatest example3
```swift
print("---------- combineLatest3 ----------")
let dateStyle = Observable<DateFormatter.Style>.of(.short, .long)
let today = Observable<Date>.of(Date())

let makeToday = Observable
    .combineLatest(dateStyle, today) { dateStyle, today -> String in
        let dateFormatter = DateFormatter()
        dateFormatter.dateStyle = dateStyle
        return dateFormatter.string(from: today)
    }
    
makeToday
    .subscribe(onNext: {
        print($0)
    })
    .disposed(by: disposeBag)
```
### result
```swift
---------- combineLatest3 ----------
8/13/22
August 13, 2022
```

## zip

### zip example
```swift
print("---------- zip ----------")
enum WinOrLose {
    case win
    case lose
}

let match = Observable<WinOrLose>.of(.lose, .lose, .win, .win, .win)
let player = Observable<String>.of("korea", "japan", "USA", "UK", "brazil", "india")

let matchResult = Observable
    .zip(match, player) { result, player in
        return player + " \(result)"
    }

matchResult
    .subscribe(onNext: {
        print($0)
    })
    .disposed(by: disposeBag)
```
### 特徴
zipの中の一つでも完了したら全体が終了


### result
```swift
---------- zip ----------
korea lose
japan lose
USA win
UK win
brazil win
```