# Iterabel 객체

iterable(반복 가능)하다는 뜻인 영어 단어입니다. 자바스크립트에서는 배열과 이터러블 객체는 상당히 유사합니다. 물론 배열말고도 문자열 역시도 상당히 유사하죠. 즉 어떤 것들의 집합, 목록을 반복성을 나타내는 경우 이터러블 객체가 포함되어 있다고 생각하시면 될 거 같습니다.

위에서 객체라고 했는데, 객체 또한 이터러블로 만들 수 있습니다. 실제로 자바스크립트에서는 배열과 객체는 상당히 유사합니다. 그렇다고 두 개가 완벽하게 똑같지는 않습니다. 깊은 얘기는 다음에 하고 일단 객체를 이터러블 객체로 만들어 보겠습니다.

<br>
<br>
<br>

# Symbol.iterator

다음은 이터러블 객체로 만들기 쉬운 예제입니다.

```js
let range = {
  from: 1,
  to: 5,
};

// 우리의 목표는 다음과 같이 동작하는 것입니다.
// for(let num of range) ... num = 1, 2, 3, 4, 5
```

객체를 이터러블로 만드려면 Symbol.iterator(특수 내장 심볼)메서드를 추가해야 합니다. 다음과 같은 사항은 꼭 지켜져야 합니다.

1. 반복문(for...of)는 `Symbol.iterator`를 찾습니다. 만약 `Symbol.iterator`가 존재하지 않는다면, 에러를 발생합니다. 그리고 `Symbol.iterator`는 반드시 `next` 함수를 반환해야 합니다

2. 위 조건을 만족한다면 반복문은 문제 없다고 판단하여 동작합니다.

3. `next()` 매서드의 반환값은 반드시 다음과 같아야 합니다. `{done: Boolean, value: any}` 여기에서 done은 반복문 종료를 의마합니다.

아래는 예제입니다.

```js
let range = {
  from: 1,
  to: 5,
};

// Symbol.iterator는 이터레이터 객체를 반환합니다.
range[Symbol.iterator] = function () {
  return {
    current: this.from,
    last: this.to,

    // for문과 같이 계속 반복하는 잡업을 정의하는 곳입니다.
    next() {
      // 현재 current값이 last보다 작을 경우 if안의 내용을 실행한다.
      if (this.current <= this.last) {
        return { done: false, value: this.current++ };
        // 만약 위 내용이 성립하지 않는다면 else문을 실행한다.
      } else {
        // done은 반복문을 종료하는 의미입니다.
        return { done: true };
      }
    },
  };
};

// 이제 의도한 대로 동작합니다!
for (let num of range) {
  console.log(num); // 1, then 2, 3, 4, 5
}

// 간략하게 나타내면 다음과 같이 사용할 수 있습니다.
let range = {
  from: 1,
  to: 5,

  [Symbol.iterator]() {
    this.current = this.from;
    return this;
  },

  next() {
    if (this.current <= this.to) {
      return { done: false, value: this.current++ };
    } else {
      return { done: true };
    }
  },
};

for (let num of range) {
  alert(num); // 1, then 2, 3, 4, 5
}
```

<br>
<br>
<br>

# 문자열도 이터러블입니다.

다음을 보면 문자열도 이터러블이 내장되어 있다는 것을 알 수 있습니다.

```js
for (let char of "test") {
  // 글자 하나당 한 번 실행됩니다(4회 호출).
  alert(char); // t, e, s, t가 차례대로 출력됨
}

//이터레이터 명시적 호출
let str = "Hello";

// for..of를 사용한 것과 동일한 작업을 합니다.
// for (let char of str) alert(char);

let iterator = str[Symbol.iterator]();

while (true) {
  let result = iterator.next();
  if (result.done) break;
  alert(result.value); // 글자가 하나씩 출력됩니다.
}
```

<br>
<br>
<br>

# 이터러블과 유사 배열

- 이터러블은 위에서 설명한 것과 같이 매서드 Symbol.iterator가 구현된 객체입니다.
- 유사배열은 인덱스와 `length`프로퍼티가 있어, 배열 내장객체가 있는 것 처럼 보이는 객체입니다.

다음과 같은 예 보겠습니다.

```js
// 유사 배열입니다. 이름만 유사 배열이지, 배열과 같은 동작을 할 수 있는 건 아닙니다. 또 이터러블과 같이 반복문을 수행할 수도 없습니다.
let arrayLike = {
  0: "Hello",
  1: "World",
  length: 2,
};

// Symbol.iterator가 없으므로 에러 발생
for (let item of arrayLike) {
}
```

<br>
<br>
<br>

# Array.from

위에서 설명했던 이터러블과 유사 배열을 연장입니다.
만약 이터러블이나, 유사 배열을 진짜 배열과 같이 사용하고 싶다면, Array.from을 사용하면 됩니다.

```js
let arrayLike = {
  0: "Hello",
  1: "World",
  length: 2,
};

// arrayLike객체를 잘 봐야 합니다. 이터러블 매서드를 추가해주지 않은 '유사배열'입니다.
let arr = Array.from(arrayLike);
console.log(Array.isArray(arr)); // true

arr.pop(); // 이제 array매서드를 사용할 수 있습니다.

// 위에서 사용했던 range 객체(이터러블 매서드 추가)를 똑같이 Array.from해도 잘 바뀌는 걸 알 수 있습니다.

let arr = Array.from(range);
console.log(Array.isArray(arr)); // true
```

<br>

참고로 for .. in과 for ...of는 다른 동작을 수행합니다.

- for ... in 은 객체의 프로퍼티를 반복하여 출력하는 것이 가능합니다. 이는 꼭 이터러블 매서드가 필요하지 않고 출력할 수 있습니다.
- for ... of 는 ES6에 새로 추가된 문법이며, 객체에 이터러블 객체가 꼭 존재해야 반복문을 수행할 수 있습니다.
