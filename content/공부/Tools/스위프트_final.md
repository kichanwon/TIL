# Swift 프로그래밍 기말고사 정리


```swift
// Optional 변수 선언
// 값이 있을 수도 있고 없을 수도 있는 타입
var name: String? = nil

// Closure 선언
// 이름이 없는 함수 형태의 코드 블록
let greet = { (name: String) -> String in
    return "Hello \(name)"
}

print(greet("Swift"))


// Protocol 선언
// 클래스, 구조체, 열거형이 따라야 할 규칙
protocol Flyable {
    func fly()
}

// Struct가 Protocol 채택
struct Bird: Flyable {
    func fly() {
        print("Fly")
    }
}

let bird = Bird()
bird.fly()


// Class 선언
// class는 참조 타입
class Rectangle {
    // 인스턴스 프로퍼티
    var width: Double
    var height: Double

    // 생성자
    init(width: Double, height: Double) {
        self.width = width
        self.height = height
    }

    // 메서드
    func area() -> Double {
        return width * height
    }
}

// 인스턴스 생성 후 인스턴스 프로퍼티와 메서드 접근
let rect = Rectangle(width: 10, height: 5)
print(rect.area())   // 50.0


// Type Property
// 인스턴스 생성 없이 타입 이름으로 접근
class Counter {
    static var count = 0
}

Counter.count += 1
print(Counter.count)


// Collection 선언
let arr = [1, 2, 3]
let set: Set<Int> = [1, 2, 3]
let dict = ["A": 100, "B": 90]

print(arr[0])        // 1
print(set.count)     // 3
print(dict["A"]!)    // 100


// if문
let age = 20

if age >= 20 {
    print("성인")
} else {
    print("미성년자")
}


// guard문
// 조건이 거짓이면 현재 코드 블록을 빠져나감
func checkAge(_ age: Int) {
    guard age >= 20 else {
        print("미성년자")
        return
    }

    print("성인")
}

checkAge(18)


// 2의 배수 판단
let num = 8

if num % 2 == 0 {
    print("2의 배수")
} else {
    print("2의 배수 아님")
}


// 쇼핑몰 총 판매액 계산
let quantity = 10
let price = 1000

var total = quantity * price

// 수량이 10개 이상이면 10% 할인
if quantity >= 10 {
    total = Int(Double(total) * 0.9)
}

// 할인 후 금액이 5000원 이상이면 배송비 무료
if total >= 5000 {
    print("배송비 무료")
} else {
    total += 2500
}

print(total)
```

## 1. Closure란?

Closure는 이름이 없는 함수이다.  
변수나 상수에 저장할 수 있고, 함수의 인자로 전달할 수 있다.

```swift
let greet = { (name: String) -> String in
    return "Hello \(name)"
}
```

## 2. 객체 지향 프로그래밍 언어의 3요소

1. 캡슐화 [[Encapsulation]]
2. 상속 [[Inheritance]]
3. 다형성 [[polymorphism]]

## 3. 인스턴스 프로퍼티와 타입 프로퍼티의 차이

인스턴스 프로퍼티는 객체마다 각각 가지는 프로퍼티이다.

```swift
class Person {
    var name = ""
}
```

타입 프로퍼티는 타입 자체가 가지는 프로퍼티이며, 모든 인스턴스가 공유한다.

```swift
class Person {
    static var count = 0
}
```

| 인스턴스 프로퍼티 | 타입 프로퍼티 |
|---|---|
| 객체마다 따로 존재 | 타입에 하나만 존재 |
| 인스턴스 생성 후 접근 | 타입 이름으로 접근 |
| `person.name` | `Person.count` |

## 4. 프로토콜이란?

프로토콜은 클래스, 구조체, 열거형이 따라야 할 규칙 또는 설계도이다.

```swift
protocol Flyable {
    func fly()
}
```

```swift
struct Bird: Flyable {
    func fly() {
        print("Fly")
    }
}
```

## 5. `struct MyApp: App { ... }`의 의미

```swift
@main
struct MyApp: App {
    var body: some Scene {
        WindowGroup {
            ContentView()
        }
    }
}
```

의미:

- `MyApp`이라는 구조체를 정의한다.
- `App` 프로토콜을 채택한다.
- SwiftUI 앱의 시작점이다.
- `ContentView()`가 첫 화면으로 실행된다.

## 6. Rectangle 클래스 정의

```swift
class Rectangle {
    var width: Double
    var height: Double

    init(width: Double, height: Double) {
        self.width = width
        self.height = height
    }

    func area() -> Double {
        return width * height
    }
}
```

## 7. Rectangle 클래스 활용

```swift
let rect = Rectangle(width: 10, height: 5)
print(rect.area())
```

실행 결과:

```swift
50.0
```

## 8. 옵셔널 변수 정의

옵셔널은 값이 있을 수도 있고, 없을 수도 있는 타입이다.  
값이 없을 때는 `nil`을 가진다.

```swift
var name: String?
name = nil
name = "Swift"
```

## 9. Swift 컬렉션 정의와 종류

컬렉션은 여러 개의 데이터를 저장하는 자료구조이다.

### Array

순서가 있고, 중복을 허용한다.

```swift
let arr = [1, 2, 3]
```

### Set

순서가 없고, 중복을 허용하지 않는다.

```swift
let set: Set<Int> = [1, 2, 3]
```

### Dictionary

키와 값의 쌍으로 데이터를 저장한다.

```swift
let dict = ["A": 100, "B": 90]
```

## 10. guard문과 if문의 차이

if문은 조건이 참일 때 특정 코드를 실행한다.

```swift
if age >= 20 {
    print("성인")
}
```

guard문은 조건이 거짓이면 즉시 종료한다.

```swift
guard age >= 20 else {
    return
}
```

| if문 | guard문 |
|---|---|
| 조건 분기 | 조기 종료 |
| 중첩 구조가 생기기 쉬움 | 코드 흐름이 단순해짐 |
| 조건이 참일 때 실행 | 조건이 거짓이면 종료 |

## 11. Set 실행 결과 문제

문제:

```swift
let oddSet: Set<Int> = [1,3,5,7,9]
let evenSet: Set<Int> = [0,2,4,6]
let primeSet: Set<Int> = [2,3,5,7]

print(oddSet.subtracting(primeSet).sorted())
```

풀이:

```swift
oddSet = [1, 3, 5, 7, 9]
primeSet = [2, 3, 5, 7]
```

`oddSet`에서 `primeSet`에 포함된 값을 제거한다.

```swift
[1, 9]
```

정답:

```swift
[1, 9]
```

## 12. 반복문 실행 결과 문제

문제:

```swift
for index in 1..<5 {
    print("\(index) times 5 is \(index*5)")
}
```

`1..<5`는 1 이상 5 미만이므로 다음 값이 반복된다.

```swift
1, 2, 3, 4
```

실행 결과:

```swift
1 times 5 is 5
2 times 5 is 10
3 times 5 is 15
4 times 5 is 20
```

## 13. 쇼핑몰 총 판매액 프로그램

조건:

- 수량과 가격을 입력받는다.
- 총 판매액은 `수량 × 가격`
- 수량이 10개 이상이면 10% 할인
- 할인 후 금액이 5000원 이상이면 배송비 무료
- 그렇지 않으면 배송비 2500원 추가

예시 코드:

```swift
let quantity = 10
let price = 1000

var total = quantity * price

if quantity >= 10 {
    total = Int(Double(total) * 0.9)
}

if total >= 5000 {
    print("배송비 무료")
} else {
    total += 2500
}

print(total)
```

## 14. 입력받은 정수가 2의 배수인지 판단

```swift
while true {
    let num = Int(readLine()!)!

    if num % 2 == 0 {
        print("2의 배수")
    } else {
        print("2의 배수 아님")
    }
}
```

핵심 조건:

```swift
num % 2 == 0
```

## 15. 조건문 실행 결과 문제

문제:

```swift
let a = 10
let b = 3
let c = a % b

if (a < b && c == 1) {
    print("Hello")
} else {
    print("World")
}
```

풀이:

```swift
c = 10 % 3
c = 1
```

조건식:

```swift
a < b && c == 1
10 < 3 && 1 == 1
false && true
false
```

따라서 `else`가 실행된다.

정답:

```swift
World
```

# 시험 직전 암기 요약

## Closure

이름 없는 함수

## Optional

값이 있을 수도 있고 없을 수도 있는 타입

```swift
var x: Int?
```

## Protocol

구현해야 할 규칙 또는 설계도

## OOP 3요소

- 캡슐화
- 상속
- 다형성

## Collection

| 종류 | 특징 |
|---|---|
| Array | 순서 있음, 중복 허용 |
| Set | 순서 없음, 중복 불가 |
| Dictionary | Key-Value 저장 |

## guard

조건이 거짓이면 즉시 종료

## 자주 나오는 실행 결과

```swift
oddSet.subtracting(primeSet).sorted()
```

결과:

```swift
[1, 9]
```

```swift
for index in 1..<5
```

반복 값:

```swift
1, 2, 3, 4
```

```swift
10 < 3 && 10 % 3 == 1
```

결과:

```swift
false
```

출력:

```swift
World
```