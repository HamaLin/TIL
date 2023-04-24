# Interface

타입스크립트에서의 Interface는 타입에 이름을 짓는 행위같습니다.
인터페이스의 이름은 대부분 첫 글자를 대문자로 짓는것이 형식적입니다.

타입스크립트에서 옵셔널 체이닝을 이용해 프로퍼티를 선언할 경우, 그 프로퍼티가 존재하지 않아도 문제 없다고 판단하여 완전히 똑같지 않는 객체를 전달하더라도 에러를 발생시키지 않습니다. 이는 유연하지만, 가끔 위험할 수도 있으니, 정확한 값은 옵셔널 체이닝을 하지않도록 합시다.

```jsx
interface Vocaloid {
  name?: string;
  number?: number;
}

function createVocaloid(config: Vocaloid): {
  name: string,
  generation: number,
} {
  let newSquare = { name: "", generation: 0 };

  switch (config.number) {
    case 1000:
      newSquare.name = "miku";
      newSquare.generation = 1;
      break;
    case 2000:
      newSquare.name = "kahu";
      newSquare.generation = 3;
      break;
    case 3000:
      newSquare.name = "rin";
      newSquare.generation = 2;
      break;
    default:
      newSquare.name = "unknown";
      break;
  }

  return newSquare;
}

let myVocaloid = createVocaloid({ number: 1000 }); // {name: 'miku', generation: 1}
```

<br>
<br>
<br>

# 읽기전용

가끔 프로퍼티를 읽기전용으로 사용하고 싶을 때도 있을겁니다. 그럴때는 읽기전용으로 선언하면 프로퍼티를 읽기전용으로 사용할 수 있습니다.

읽기전용은 다음과 같이 사용할 수 있습니다.

```tsx
interface coordinate {
  readonly x: number;
  readonly y: number;
}

let space: coordinate = { x: 12, y: 21 };

space.x = 20;
```

배열같은 경우

```tsx
let arr: ReadonlyArray<number> = [1, 2, 3];
let newArr = [];

arr[0] = 0; // Error
arr.push(4); // Error

newArr = arr; // Error
```

하지만 다음과 같이 타입 단언을 이용하면 재 할당은 할 수 있습니다.

```tsx
newArr = arr as number[];
```

하지만 다음과 같이 완전히 새로운 타입으로는 선언할 수 없습니다.

```tsx
newArr = arr as string[]; // Error
```

<br>
<br>
<br>

# 프로퍼티 확장

정적 타입외에도 가끔은 유연하게 타입을 받아야 할 때가 존재합니다.
특히 그런 타입의 경우 기존 타입에서 1~2개씩만 붙는 경우가 있고, 다른곳에서 재활용 하지 않을 때도 있습니다. 그런 경우 어떻게 타입을 정의해서 사용해야 할까요?

```tsx
interface Vocaloid {
  name?: string;
  number?: number;
}

function createVocaloid(config: Vocaloid): {
  name: string;
  generation: number;
} {
  ...
}

// 기존 타입에 generType을 추가 매개변수로 전달하려고 한다.
let myVocaloid = createVocaloid({ name: 'Rin', genderType: 'F' });
```

1.  타입 단언

    ```tsx
    let myVocaloid = createVocaloid({ genderType: "F" } as Vocaloid);
    ```

2.  타입 확장 선언 (이런경우 추가 될 프로퍼티가 있는 경우를 확신할 때 사용합니다.)

    ```tsx
    interface Vocaloid {
      name?: string;
      number?: number;
      [propName: string]: any;
    }
    ```

3.  새로운 변수 선언
    (이 방법은 TS컴파일러를 회피하는 방식인거 같습니다, 다만 일치하는 프로퍼티가 한개라도 있어야 가능한 방법입니다.)
    ```tsx
    let newObj = {name: '', genderType: 'F'}
    let myVocaloid = createVocaloid(newObj);

        let newObj = {genderType: 'F'}
        let myVocaloid = createVocaloid(newObj); // Error
        ```

<br>
<br>
<br>

# 함수타입 선언

함수 타입의 경우 다음과 같이 사용할 수 있습니다.

```tsx
interface SearchFunc {
  (source: string, subString: string): boolean;
}

let mySearch: SearchFunc;
mySearch = function (source, subString) {
  let result = source.search(subString);
  return result > -1;
};
```

<br>
<br>
<br>

# 인덱시블 타입

인덱시블 타입이라는 건 배열에서 `arr[1]` 이나 `obj['id']` 와 같이 값을 가져오는 것처럼, '인덱싱'으로 타입을 정의하는 것을 말합니다.

다음과 같은 예제를 보시죠

```ts
interface ArrInterface {
  [index: number]: string;
}

let arr: ArrInterface = ["bob", "jin"];
```

다음과 같이 타입을 정의할 수 있습니다. 다만 인덱싱 타입은 `string`과 `number`만 가능하다는 점만 기억하면 될 거 같습니다.

그리고 인덱시블은 인터페이스안에 1가지만 정의할 수 있는 것으로 보입니다(추측) 해당 인터페이스의 규칙을 정립하는 방식이라 2가지가 공존하기 어렵다고 생각이 듭니다. 다음과 같이 보시죠

```ts
interface NumberDictionary {
  [index: string]: number;
  [index: number]: string; // X 두 가지의 인덱시블은 에러를 유발합니다.
  length: number; // 성공, length는 숫자입니다
  name: string; // 오류, `name`의 타입은 인덱서의 하위타입이 아닙니다
}

// 위의 오류를 고치고 싶다면 다음과 같이 하시면 됩니다.
interface NumberDictionary {
  [index: string]: number | string;
  length: number; // 성공, length는 숫자입니다
  name: string; // 성공, name은 string입니다.
}

// 인덱시블도 readonly타입을 선언 할 수 있습니다.
interface ReadonlyStringArray {
  readonly [index: number]: string;
}
let myArray: ReadonlyStringArray = ["Alice", "Bob"];
myArray[2] = "Mallory"; // 오류!
```

<br>
<br>
<br>

# 클래스 타입
