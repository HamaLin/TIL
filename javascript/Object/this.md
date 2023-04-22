# This

This를 이해하기 전에 먼저 메서드에 대해서 알아야 한다.
메서드는 말이 어렵지 그냥 함수이다. 그런데 왜 메서드라는 이름으로 불려지냐 하면, 객체 안에 존재하는 함수이기 때문이다.

매서드도 2가지의 형태를 가지고 있다.

```jsx
// 일반적인 메서드
user = {
  sayHi: function () {
    alert("Hello");
  },
};

// 단축 구문
user = {
  sayHi() {
    alert("Hello");
  },
};
```

그럼 This가 왜 필요한지 다음과 같은 가정이 있다고 생각해보자

```jsx
const vocaloid = {
  name: "miku",
  vocaloidGenerate: function () {
    const name = "kahu";

    console.log(name); // kahu
    console.log(this.name); // miku
  },
};

vocaloid.vocaloidGenerate();
```

보다 싶히 객체 안에 name이 두 가지 선언되어 있다. 만약 `vocaloidGenerate` 를 사용해서 현재 객체의 name을 불러올 수도 있을 것이다. 이때 사용하는게 this다.

그러면 다음과 같은 사용할 수도 있지않을까? 생각할 수 있다.

```jsx
const vocaloid = {
  name: "miku",
  vocaloidGenerate: function () {
    const name = "kahu";
    console.log(vocaloid.name); // miku
  },
};
```

ㅇ이렇게 사용해도 되긴 하지만 다음과 같은 문제도 발생할 수 있다.

```jsx
let vocaloid = {
  name: "miku",
  vocaloidGenerate: function () {
    const name = "kahu";
    console.log(vocaloid.name); // miku
  },
};

let newVocaloid = vocaloid;
vocaloid = null;

newVocaloid.vocaloidGenerate(); // Error
```

저번에 적었던 거 처럼 객체를 복사해서 넣는게 아닌 주소값을 넣는거라고 했었다.
즉 `newVocaloid` 에는 주소값이 들어갔고, 실제 객체가 존재하는`vocaloid` 에는 null을 넣었으니, `newVocaloid` 을 불러도 아무것도 존재하지 않게되는 것이다.

<br>
<br>
<br>

## This의 다른 사용 용도

---

앞에서 This는 객체의 프로퍼티를 가져오는 용도로 예를 들었지만, 이번에는 다른 방면으로 사용해보도록 하자

```jsx
function sayHi() {
  alert(this.name);
}
```

객체가 아닌 이번에는 일반 함수에서 this를 호출할수도 있다. 물론 저렇게 실행하면 undefined만 출력한다.

이걸 다음과 같이 사용할 수 있다.

```jsx
let user = { name: "John" };
let admin = { name: "Admin" };

function sayHi() {
  alert(this.name);
}

user.f = sayHi;
admin.f = sayHi;

user.f(); // John  (this == user)
admin.f(); // Admin  (this == admin)

admin["f"](); // Admin
```

ㅇ함수를 사용해, 객체의 메서드를 정의하는 방법으로 사용할 수는 있다.

개인적으로는 이렇게 사용하는 방면이 많을까? 의문이 든다. 그냥 Class사용하는게 더 좋지 않을까 생각이 들지만, Javascript실제 응용편에 정리하도록 해봐야 겠다.

<br>
<br>
<br>

## 화살표 함수의 This

---

화살표 함수에서는 This를 사용할 수 없다.
사이트에서 설명하는 말이 잘 이해가 안되는데(예재도 이해가 안간다.) 다음과 같이 보면 될 거 같다.

```jsx
let vocaloid = {
  name: "miku",
  vocaloidGenerate: function () {
    console.log(this.name);
  },

  vocaloidGenerate2: () => {
    console.log(this.name);
  },
};

vocaloid.vocaloidGenerate(); // miku
vocaloid.vocaloidGenerate2(); // undefined
```

다음과 같이 실행하면 undefined가 나온다.
즉 this로 접근하지 못하게 하려면 화살표 함수를 사용하면 될 거 같다.
