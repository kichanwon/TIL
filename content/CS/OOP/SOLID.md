> 로버트 마틴(Robert C. Martin)이 정립한 **유연하고 유지보수 가능한 객체지향 설계 지침**
---
### SRP (Single Responsibility Principle)
**단일 책임 원칙**
- 클래스는 **오직 하나의 책임(변경 이유)**만을 가져야 함
- 하나의 클래스가 **하나의 기능**만을 수행하도록 분리
> 유지보수성 향상 / 테스트 용이
```Java
public class UserService {
    public void registerUser(String email) {
        // 사용자 등록 로직
    }
    public void sendEmail(String email) {
        // 이메일 발송 로직 (책임 분리 안 됨)
    }
}
```

```Java
public class UserService {
    public void registerUser(String email) {
        // 사용자 등록 로직
    }
}
public class EmailService {
    public void sendEmail(String email) {
        // 이메일 발송 로직
    }
}
```
---
### OCP (Open-Closed Principle)
**개방-폐쇄 원칙**
- **확장에는 열려 있고**, **변경에는 닫혀 있어야 함**
- 기존 코드를 건드리지 않고 기능을 확장할 수 있어야 함
> 리스크 없이 기능 확장 / 기존 코드 안정성 확보
```Java
public class DiscountService {
    public int getDiscount(String grade) {
        if (grade.equals("VIP")) return 20;
        else if (grade.equals("BASIC")) return 10;
        return 0;
    }
}
```

```Java
public interface DiscountPolicy {
    int getDiscount();
}

public class VIPDiscount implements DiscountPolicy {
    public int getDiscount() { return 20; }
}

public class BasicDiscount implements DiscountPolicy {
    public int getDiscount() { return 10; }
}

public class DiscountService {
    public int calculateDiscount(DiscountPolicy policy) {
        return policy.getDiscount();
    }
}
```
---
### LSP (Liskov Substitution Principle)
**리스코프 치환 원칙**
- 부모 클래스 객체를 사용하는 곳에 **자식 클래스 객체를 대체**해도 **정상 작동**해야 함
- 하위 클래스는 부모 클래스의 행위를 **깨뜨리지 않아야 함**
> 상속 구조의 일관성 / 신뢰성 보장
```Java
class Bird {
    public void fly() {}
}
class Ostrich extends Bird {
	// 날 수 없음
    public void fly() { throw new UnsupportedOperationException(); } 
}
```

```java
interface Flyable {
    void fly();
}
class Sparrow implements Flyable {
    public void fly() { /* 날기 구현 */ }
}
class Ostrich {
    // fly() 없음
}
```
---
### ISP (Interface Segregation Principle)
**인터페이스 분리 원칙**
- **하나의 일반적인 인터페이스보다는 여러 개의 구체적인 인터페이스**로 나누어야 함
- 클라이언트는 **필요하지 않은 인터페이스에 의존해서는 안 됨**
> 불필요한 의존 제거 / 모듈화 향상
```java
interface Worker {
    void work();
    void eat();
}
class Robot implements Worker {
    public void work() { /* 작업 */ }
    // 먹지 않음
    public void eat() { throw new UnsupportedOperationException(); } 
}
```

```java
interface Workable {
    void work();
}
interface Eatable {
    void eat();
}
class Human implements Workable, Eatable {
    public void work() {}
    public void eat() {}
}
class Robot implements Workable {
    public void work() {}
}
```
---
### DIP (Dependency Inversion Principle)
**의존 역전 원칙**
- 고수준 모듈(정책)은 저수준 모듈(세부사항)에 의존하면 안 됨
- **추상화에 의존해야 하며**, 세부 구현에 의존하지 않아야 함
> 유연한 구조 / 테스트 및 유지보수 용이
```java
public class OrderService {
    private MySQLDatabase db = new MySQLDatabase(); // 구체 클래스에 의존
}
```

```java
public interface Database {
    void saveOrder(String order);
}

public class MySQLDatabase implements Database {
    public void saveOrder(String order) { /* 저장 */ }
}

public class OrderService {
    private Database db;

    public OrderService(Database db) {
        this.db = db;
    }
}
```
---