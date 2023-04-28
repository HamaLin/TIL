# Map

맵은 객체와 상당히 유사합니다. 만약 객체와 똑같은 일을 수행한다면, 맵이라는 개념이 나오지 않았겠죠? 맵은 객체와 다르게 키(프로퍼티)에 다양한 자료형을 선언할 수 있다.

맵에는 다음과 같은 특징들이 있습니다.

- `new Map()` 을 통해 map을 만들수 있습니다.
- `map.set(key, value)` - `Key`를 이용해서 value를 저장할 수 있습니다.
- `map.get(key)` - `Key`에 해당하는 값을 반환합니다. `key`가 존재하지 않으면 undefined를 반환합니다.
- `map.has(key)` - `Key`가 존재하면 true, 없으면 false를 반환합니다.
- `map.delete(key)` - `Key`에 해당하는 값을 삭제합니다.
- `map.clear()` - 모든 요소를 제거합니다.
- `map.size` - 요소의 갯수를 반환합니다.

```js
let map = new Map();

map.set("1", "str1");
map.set(1, "num1");
map.set(true, "bool1");

console.log(map); // Map(3) { '1' => 'str1', 1 => 'num1', true => 'bool1' }

console.log(map.get(1)); // num1
console.log(map.get("1")); // str1

console.log(map.size); // 3
```

`map[key]`를 사용해서 값을 가져올 수는 있습니다만, 권장하는 방법은 아닙니다. 위와 같은 방법을 이용해서 불러오면 일반 객체처림 취급하여 map의 장점을 읽어버립니다. 가능하면 `set`, `get`을 이용해야 합니다.

**맵은 객체를 키로 사용할 수 있습니다.**

```js
let miku = { name: "miku" };

let vocaloidMap = new Map();

vocaloidMap.set(miku, 123);

console.log(vocaloidMap.get(miku)); // 123
```

객체를 키로 사용할 수 있는점은 맵의 최대 장점중 하나입니다.

이를 이용하여 map 체이닝을 할 수 있습니다.

```js
map.set("1", "str1").set(1, "num1").set(true, "bool1");
```

<br>
<br>
<br>

# 맵의 반복문

다음과 같은 요소를 사용하여 반복문을 수행할 수 있습니다.

- `map.keys()` – 각 요소의 키를 모은 **반복 가능한(iterable, 이터러블)** 객체를 반환합니다.
- `map.values()` – 각 요소의 값을 모은 이터러블 객체를 반환합니다.
- `map.entries()` – 요소의 [키, 값]을 한 쌍으로 하는 이터러블 객체를 반환합니다. 이 이터러블 객체는 for..of반복문의 기초로 쓰입니다.

```js
let recipeMap = new Map();
recipeMap.set("cucumber", 500);
recipeMap.set("tomatoes", 350);
recipeMap.set("onion", 50);

// 이렇게 선언 할 수도 있습니다.
// let recipeMap = new Map([
//   ['cucumber', 500],
//   ['tomatoes', 350],
//   ['onion',    50]
// ]);

// 키(vegetable)를 대상으로 순회합니다.
for (let vegetable of recipeMap.keys()) {
  alert(vegetable); // cucumber, tomatoes, onion
}

// 값(amount)을 대상으로 순회합니다.
for (let amount of recipeMap.values()) {
  alert(amount); // 500, 350, 50
}

// [키, 값] 쌍을 대상으로 순회합니다.
for (let entry of recipeMap) {
  // recipeMap.entries()와 동일합니다.
  alert(entry); // cucumber,500 ...
}

// forEach를 다음과 같이 사용할 수 있습니다.
recipeMap.forEach((value, key, map) => {
  alert(`${key}: ${value}`); // cucumber: 500 ...
});
```

맵은 일반 객체와 달리 삽인 순서를 기억합니다. 그래서 반복문을 수행할 때 넣은 순서대로 출력이 되는 것입니다.

<br>
<br>
<br>

# 객체를 맵으로 바꾸기

평범한 객체를 맵으로 만들고 싶다면 내장 메서드 `Object.entries(obj)`를 활용하면 됩니다. 이 메서드는 객체의 키-값로 가지는 배열로 반환합니다.

```js
let obj = {
  name: "John",
  age: 30,
};

console.log(Object.entries(obj)); // [ [ 'name', 'John' ], [ 'age', 30 ] ]

let map = new Map(Object.entries(obj));

alert(map.get("name")); // John
```

<br>
<br>
<br>

# 맵을 객체로 바꾸기

위의 행동과 반대로 해보겠습니다.
맵을 객체로 바꾸는 방법은 `Object.fromEntries`를 사용하면 됩니다. 이 메서드는 [키, 값]인 배열을 일반 객체로 바꿔줍니다.

```js
let map = new Map();
map.set("banana", 1);
map.set("orange", 2);
map.set("meat", 4);

// 이렇게도 할 수 있습니다.
// let map = new Map([
//     ['banana', 1]
//     ['orange', 2]
//     ['meat', 4]
// ])

let obj = Object.fromEntries(map.entries()); // 맵을 일반 객체로 변환 (*)

// 맵이 객체가 되었습니다!
// obj = { banana: 1, orange: 2, meat: 4 }

alert(obj.orange); // 2
```

<br>
<br>
<br>

# Set

셋은 중복을 허용하지 않는 값을 모아놓은 컬렉션입니다. 셋에는 키 없이 저장됩니다.

메서드는 다음과 같습니다.

- `new Set(iterable)` – 셋을 만듭니다. 이터러블 객체를 전달받으면(대개 배열을 전달받음) 그 안의 값을 복사해 셋에 넣어줍니다.
- `set.add(value)` – 값을 추가하고 셋 자신을 반환합니다.
- `set.delete(value)` – 값을 제거합니다. 호출 시점에 셋 내에 값이 있어서 제거에 성공하면 true, 아니면 false를 반환합니다.
- `set.has(value)` – 셋 내에 값이 존재하면 true, 아니면 false를 반환합니다.
- `set.clear()` – 셋을 비웁니다.
- `set.size` – 셋에 몇 개의 값이 있는지 세줍니다.

```js
let set = new Set();

let john = { name: "John" };
let pete = { name: "Pete" };
let mary = { name: "Mary" };

set.add(john);
set.add(pete);
set.add(mary);
set.add(john);
set.add(mary);

console.log(set.size); // 3

for (let user of set) {
  console.log(user.name); // // John, Pete, Mary 순으로 출력됩니다.
}
```

arr.find를 통해서 중복값을 사용할 수도 있습니다. 하지만 arr.find는 모든 배열에 걸쳐서 중복을 검사하기 때문에, 성능면으로 Set에 비해 떨어집니다. 즉 중복되지 않는 값을 사용하고자 할때는 Set이 최적이 될 수 있습니다.

<br>
<br>
<br>

# 셋의 반복문

Map과 마찬가지로 for ...of나 forEach를 사용하여 반복문을 사용할 수 있습니다.

```js
let set = new Set(["oranges", "apples", "bananas"]);

for (let value of set) alert(value);

// forEach를 사용해도 동일하게 동작합니다.
set.forEach((value, valueAgain, set) => {
  alert(value);
});
```

forEach에 valueAgain, set이라는 매개변수가 눈에 띄입니다. valueAgain은 value와 동일한 값, set은 객체(셋)입니다. 특별한 기능을 위해 존재하는 것이 아닌, 맵과의 호환성 때문에 이렇게 나옵니다.
