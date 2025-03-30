## 19장. 프로토타입

### 19.1 객체지향 프로그래밍

객체의 집합으로 프로그램을 표현하려는 프로그래밍 패러다임을 말한다.

실세계의 실체는 **속성**을 가지고 있다.

예를 들어 사람은 이름, 주소, 성별 등등.. 다양한 속성을 가진다.

속성을 통해 여러 개의 값을 하나의 단위로 구성한 복합적인 자료구조를 객체라한다.

### 19.2 상속과 프로토타입

상속은 객체지향 프로그래밍의 핵심 개념으로, 어떤 객체의 프로퍼티 또는 메서드를 다른 객체가 상속받아 그대로 사용할 수 있는 것을 말한다.

프로토타입 기반으로 상속을 구현하여 불필요한 중복을 제거할 수 있다.

[예제]
```
// Circle 생성자 함수
function Circle(radius) {
    // 반지름 프로퍼티
    this.radius = radius;
    
    // 원의 면적을 계산하는 메서드
    this.getArea = function() {
        return Math.PI * this.radius * this.radius;
    };
}

// 사용 예시
const circle1 = new Circle(5);
const circle2 = new Circle(10);

console.log(circle1.getArea === circle2.getArea);  //false

```

위의 예제처럼 getArea 메서드를 만들면 모든 인스턴스에서 각각의 getArea 메서드를 갖게 되어 메모리를 낭비한다.

```
// Circle 생성자 함수
function Circle(radius) {
    // 반지름 프로퍼티
    this.radius = radius;
}

// getArea 메서드를 프로토타입에 추가
Circle.prototype.getArea = function() {
    return Math.PI * this.radius * this.radius;
};

const circle1 = new Circle(5);
const circle2 = new Circle(10);

console.log(circle1.getArea === circle2.getArea);  //true
```

위와 같이 프로토타입으로 메서드를 할당하여 모든 인스턴스가 프로토타입으로 부터 getArea메서드를 상속받아 사용하도록 하자.
