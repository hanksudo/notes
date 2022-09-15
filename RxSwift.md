# RxSwift note

- Composable, Reusable
- Declarative
- Immutable

## Observable

Fundamental part of RxSwift, Read-only

### Create and subscribe Observable

```swift
let disposeBag = DisposeBag()

// Create
let observable = Observable.of(1, 3, 7)

// Subscribe
observable
    .subscribe(onNext: { element in 
        print("Element:", element)
    }, onError: { error in 
        print("Error:", error)
    }, onCompleted: {
        print("Completed")
    }, onDisposed: {
        print("Disposed")
    })
    .disposed(by: disposeBag)
```

### Other examples

```swift
let observable = Observable.of(1, 3, 7)
let observable = Observable.just(true)
let observable = Observable.from([1, 3, 7])
let observable = Observable.from(p[tion[1, 3, 7])
// never stop
let observable: Observable<Int> = .never()
// only completed
let observable: Observable<Int> = .empty()
// only error
let error = NSError(domain: "test", code: -1, userInfo: nil)
let observable: Observable<Int> = .error(error)
```

### Custom Observable

```swift
// Custom Observable
let disposeBag = DisposeBag()
  
let myJust = { (element: String) -> Observable<String> in
    return Observable.create { observer in
        observer.onNext(element)
        observer.onCompleted()
        return Disposables.create()
    }
}
    
myJust("🔴")
    .subscribe { print($0) }
    .disposed(by: disposeBag)
```

### Observable vs Driver

**Driver** automatically adds `shareReplay(1)`

```swift
// By Observable
var username = Observable<String>
validatedUsername = input.username
    .flatMapLatest { username in
        return validationService.validateUsername(username)
            .observeOn(MainScheduler.instance)
            .catchErrorJustReturn(.failed(message: "Error contacting server"))
    }
    .share(replay: 1)
// By Driver
validatedUsername = input.username
    .flatMapLatest { username in
        return validationService.validateUsername(username)
            .asDriver(onErrorJustReturn: .failed(message: "Error contacting server"))
    }
.observeOn(MainScheduler.instance)
.catchErrorJustReturn(.Failed(message: "Error contacting server"))
         
// are squashed into single 

.asDriver(onErrorJustReturn: .Failed(message: "Error contacting server"))
```

---

## Subjects

Can act as both **observable** and as **observer**

[[Link](http://reactivex.io/documentation/subject.html)]

- **PublishSubject**: Starts `empty` and only emits new elements to subscribers.
- **BehaviorSubject**: Starts with an initial value and replays it or the latest element to new subscribers.
- **ReplaySubject**: Initialized with buffer size and will remain a buffer of elements up to that size and replay it to new subscribers.
- **AsyncSubject**: Emits only the last `next` event in the sequence, and only where subject receives a `completed` event. This is a seldom used kind of subject.

---

## Relays

**Relay** is wrappers around **Subject,** Accept and relay `next` events

- **PublishRelay**
- **BehaviorRelay**: Wraps a BehaviorSubject, preserves its current value as a state, and replays only the latest/initial value to new subscribers.

## Learning resource

- [ReactiveX - Intro](http://reactivex.io/intro.html)
- [ReactiveX - Links to More Information](http://reactivex.io/tutorials.html)
- [RxMarbles: Interactive diagrams of Rx Observables](https://rxmarbles.com/)
- [ReactiveX/RxSwift](https://github.com/ReactiveX/RxSwift/blob/master/Documentation/GettingStarted.md)
- [RxExample](https://github.com/ReactiveX/RxSwift#manual)
- [RxSwift/Traits.md at main · ReactiveX/RxSwift · GitHub](https://github.com/ReactiveX/RxSwift/blob/main/Documentation/Traits.md)
- [RxSwift 中文文档](https://beeth0ven.github.io/RxSwift-Chinese-Documentation/)
- [RxSwift / 30天探索之旅 :: 第 12 屆 iT 邦幫忙鐵人賽](https://ithelp.ithome.com.tw/users/20115751/ironman/2936)
