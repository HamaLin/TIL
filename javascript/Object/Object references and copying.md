# ****참조에 의한 객체 복사****


객체는 복사하는 방식이 일반 원시 타입의 방식과 다르다.

```jsx
const message = "hello";
const copyMessage = message;

message === copyMessage
```

```jsx
const user = {
	name: 'KDY'
}
const admin = user;

admin === user
```

위 코드를 실행 했을 때 전부 당연히 같다고 나올 것이다. 근데 무엇을 기준으로 같다고 하는걸까?

결론부터 말하면, 일반적인 원시타입의 경우 값의 내용을 기준으로 판단한다. 하지만 객체의 경우는 객체가 저장되어 있는 메모리 주소를 기준으로 판단한다. 자바스크립트에서는 메모리 주소를 "참조"라고 표현한다.

아마 C언어를 했아면, 포인터라고 들어봤을 것이다. 그거와 비슷하다고 생각하면 쉽다. 그런데 여기서 재미있는 점은, C언어의 포인터는 일반 자료형이나 배열같은 모든 자료형을 메모리 주소를 기준으로 비교하지만, 자바스크립트의 경우 Object(객체)만 해당된다.

다음 그림이 정말 훌륭한 예이다.

<!-- ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b0d36333-b1fe-46b2-acfd-889ee4fa95fc/Untitled.png) -->
<img src="./images/Object references and copying.png" width="100%" height="auto" title="" ></img>

위 코드에서 선언한 user객체는 결국 user라는 이름을 가지고 있을 뿐, 실제로 가지고 있는 값은 메모리 주소이다. 그리고 admin에 값을 할당한 행위도 admin에 메모리 주소값을 넣어줬을 뿐이다.

그럼 실제 값은 어디있나? 당연히 메모리 주소를 가리키는 곳에 위치하는 것이다. 이게 생각보다 중요하다. 다음 코드에서 생각대로 풀어보겠다.

```jsx
let a = {};  // a: Ox0001 -> Ox0001: {} 
let b = a;   // b: Ox0001

// Tip: 객체는 ==, === 똑같이 인식합니다.
alert( a == b );  // true
alert( a === b ); // true
```

```jsx
let a = {};  // a: Ox0001 -> Ox0001: {} 
let b = {};   // b: Ox0002 -> Ox0002: {}

alert( a == b ); // false
```

물론 뇌피셜 일수도 있지만, 대충 동작구조가 저런거 같다. 그래서 어떻게 보면 일반적인 자료형의 비교방식과 똑같다고 생각할 수 있다. 왜냐? a의 값 Ox0001과 b의 값 Ox0001이 일치하니까 True인 것이고다.


<br>
<br>
<br>


# 객체 복사


그렇다면 이 객체특성을 모르고 다음과 같이 진행했을 때, 어떻게 되는지 보자.

```jsx
let user = {
  name: "apple",
};
let admin = { power: "root" };

// user에게 admin대입
user = {
  ...user,
  authority: admin,
};

admin; // { power: 'root' }
user.authority.power = "user";
admin; // { power: 'user' }
```

저렇게 극단적으로 코드를 작성하는 일은 없겠지만, 만약 저렇게 작성됬다면, 유저는 authority를 통해서 관리자의 power에 접근 할 수 있게 된다. 이러면 안 될 것이다. 그러면 어떻게 복사해야 할까?

2가지 방법이 있다.

1. 반복문을 이용한 무식한 복사 - **깉은 복사(deep cloning)**라고 부른다.
    
    ```jsx
    let user = {
      name: "John",
      age: 30
    };
    
    let clone = {}; // 새로운 빈 객체
    
    // 빈 객체에 user 프로퍼티 전부를 복사해 넣습니다.
    for (let key in user) {
      clone[key] = user[key];
    }
    
    clone.name = "Pete"; // clone의 데이터를 변경합니다.
    
    alert( user.name ); // 기존 객체에는 여전히 John이 있습니다.
    ```
    
2. Object.assign함수를 이용한 복사 - **얕은 복사(shallow copy)**라고 부른다.
    
    ```jsx
    Object.assign(dest, [src1, src2, src3...])
    ```

그러면 스프레드 연산자(Spread Operator)은 어떤 방식의 복사일까?
결론부터 말하면, 얕은 복사이다. 

MDN에 기재되어 있는 말이다.
>참고: Spread 문법은 배열을 복사할 때 1 레벨 깊이에서 효과적으로 동작합니다. 그러므로, 다음 예제와 같이 다차원 배열을 복사하는것에는 적합하지 않을 수 있습니다. (Object.assign() 과 전개 구문이 동일합니다)


<br>
<br>
<br>

# 객체 병합


병합하고 싶을때는 어떻게 할까?

```jsx
let user = { name: "John" };

let permissions1 = { canView: true };
let permissions2 = { canEdit: true };

// permissions1과 permissions2의 프로퍼티를 user로 복사합니다.
Object.assign(user, permissions1, permissions2);

// now user = { name: "John", canView: true, canEdit: true }
```

이렇게도 할 수 있다.

```jsx
let user = { name: "John" };

Object.assign(user, { name: "Pete" });

alert(user.name); // user = { name: "Pete" }
```

또는 이렇게

```jsx
let user = {
  name: "John",
  age: 30
};

let clone = Object.assign({}, user);
```

<br>
<br>
<br>


# 중첩 객체 복사

하지만 이런 경우는 어떻게 할까?

```jsx
let user = {
  name: "John",
  sizes: {
    height: 182,
    width: 50
  }
};

alert( user.sizes.height ); // 182
```

이러한 경우 Object.assign()을 사용하면 sizes 프로퍼티가 위에 적었던것 처럼 같은 참조값을 공유하기 때문에 반복문을 사용해서 객체의 프로퍼티 전부 복사해줘야 한다.