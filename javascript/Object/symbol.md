# 심볼형

자바스크립트에서 변수에 이름을 붙여주는 데, 이는 중복을 할 수 없다는 제약이 있습니다. 혹여나 중복이 되을경우를 방지 하기 위해서 인거죠. 하지만 가끔은 중복이 필요할 때도 있지 않을까요? 그때 사용하는것이 Symbol형입니다. 심볼형은 고유한 식별자로 사용이 되는데 같은 이름을 짓더라도 같은 이름이 아닙니다. 예제를 통해서 알아보죠

<br>

심볼형의 사용법을 한번 알아보죠

```js
// id는 새로운 심볼이 됩니다.
let id = Symbol();

// 심볼 id에는 "id"라는 설명이 붙습니다.
let id = Symbol("id");

// 심볼형에 같은 이름을 사용해도 같지 않다는 내용입니다.
let id1 = Symbol("id");
let id2 = Symbol("id");

console.log(id1 == id2); // false
```

헷갈릴수 있는데. 변수에 할당한 id1은 심볼 id1이 되는 것이고, 심볼 괄호 안에있는 "id"는 심볼 이름이라고 합니다. 이는 이름표같은 역할만 할 뿐 다른 어떤것에는 아무 영향을 주지 않습니다.

만약 심볼 이름을 알고 싶다면 다음과 같이 호출할 수 있습니다.

```js
let id = Symbol("id");
console.log(id.description); // id
```

<br>
<br>
<br>

# 숨김 프로퍼티

그렇다면 이 심볼을 이용해서 뭘 할 수 있을까요? 먼저 객체에 숨김 프로퍼티를 할 수 있습니다.
숨김 프로퍼티란 원본 object에 밀수해서 들어간 프로퍼티라고 생각하면 될 거 같습니다.
이게 뭔 개소리냐? 하겠지만, 이게 현재로써의 제가 이해한 최대입니다. 적힌바로는 심볼을 이용하면 서드파티에서 객체에 심볼을 추가한 점을 찾지 못한다. 심볼은 유일성이 보장되어 있으므로, 식별자(키 이름)이 충돌하지 않습니다.

그러니 다음과 같이 스토리를 적어보겠다. 라이브러리(제국)에 밀수(심볼)해서 들어가면 경비(호완성)에 걸리지 않는다.

<br>

다음은 객체에 심볼을 적용하는 방법이다.

```js
let user = {
  name: "John",
};

let id = Symbol("id");

user[id] = 1; // [id]는 심볼형으로 되어 있는 id를 접근하기 위한 형식입니다.

user[id] = "new symbol text"; // 심볼 id값을 변경했습니다.

//심볼형 접근과 다르게 이러한 방식은 user객체에 id프로퍼티를 추가해 주는 겁니다.
user["id"] = 1;

console.log(user[id]); // 심볼을 키로 사용해 데이터에 접근할 수 있습니다.
console.log(user["id"]); // user의 id프로퍼티를 가져옵니다.
```

<br>

또는 객체에 리터럴방식으로 다음과 같이 값을 할당할 수 있습니다.

```js
let id = Symbol("id");

let user = {
  name: "John",
  [id]: 123, // "id": 123은 안됨
};
```

심볼은 for...in에서 출력되지 않습니다. 이게 숨김 프로퍼티의 특성중 하나라고 볼 수도 있습니다.
다만 객체를 복사하는 매서드인 Object.assign은 심볼 프로퍼티도 예외없이 다 같이 복사합니다.

```js
let id = Symbol("id");
let user = {
  [id]: 123,
};

let clone = Object.assign({}, user);

console.log(clone[id]); // 123
```

# 전역 심볼

가끔은 고유한 값인 심볼에서도 규칙적인 심볼 이름을 불러올 때도 있을겁니다. 그럴때 사용한는 것이, 전역 심볼입니다. 전역심볼은 전역 심볼 레지스트리에 저장되고 불러와집니다. 고로 위의 심볼과 같이, 같은 심볼 이름을 사용하더라도 1가지만 가리키게 되어 있습니다.

전역 심볼을 할당하려면 `Symbol.for(key)` 반대로 할당된 전역 심볼 이름을 불러오려면 `Symbol.keyFor` 을 사용하면 됩니다.

```js
// 전역 레지스트리에서 심볼을 읽습니다.
let id = Symbol.for("id"); // 심볼이 존재하지 않으면 새로운 심볼을 만듭니다.

// 동일한 이름을 이용해 심볼을 다시 읽습니다.
let idAgain = Symbol.for("id");

// 두 심볼은 같습니다.
console.log(id === idAgain); // true

// 이름을 이용해 심볼을 찾음
let sym = Symbol.for("name");
let sym2 = Symbol.for("id");

// 심볼을 이용해 이름을 얻음
console.log(Symbol.keyFor(sym)); // name
console.log(Symbol.keyFor(sym2)); // id

let globalSymbol = Symbol.for("name");
let localSymbol = Symbol("name");

alert(Symbol.keyFor(globalSymbol)); // name, 전역 심볼
alert(Symbol.keyFor(localSymbol)); // undefined, 전역 심볼이 아님

alert(localSymbol.description); // name
```
