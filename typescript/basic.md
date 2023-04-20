# 기본 타입

1. 불리언(Boolean)
2. 숫자(Number)
3. 문자열(String)
4. 배열(Array)
5. 튜플(Tuple)
6. 열거(Enum)
7. Any
8. Void
9. Null, Undefined
10. Never
11. 객체(Object)
12. 타입 단언(Type assertions)


<br>
<br>

## 배열
---

TypeScript에서 배열을 선언하는 방법은 두 가지입니다.

1. 타입[]
2. Array<타입>

이때 2번째 방법을 제너릭 타입 선언이라고 합니다.


<br>
<br>

## 튜플
---

튜플은 고정된 배열의 요소를 지정할 때 사용합니다.

```tsx
// 튜플 타입으로 선언
let x: [string, number];
// 초기화
x = ["hello", 10]; // 성공
// 잘못된 초기화
x = [10, "hello"]; // 오류
```


<br>
<br>

## 열거
---

열거형은 타입에 값을 불러오는 용도로 사용하기 적합합니다.

```tsx
enum Color {Red, Green, Blue}
let c: Color = Color.Green;
```

 다음과 같이 실전에 사용할 수 있을 거 같습니다.

```tsx
enum Color {
  Red = "#ff1111",
  Green = "#11ff11",
  Blue = "#1111ff",
}
let hexColor: Color = Color.Green;
```

또는 다음과 같이 인덱스로 접근해서 값을 가져올 수도 있습니다.

```tsx
enum Color {Red = 1, Green, Blue}
let colorName: string = Color[2];

console.log(colorName); // 값이 2인 'Green'이 출력됩니다.
```


<br>
<br>

## Any
---

어떤 타입이든 쓸 수 있는 타입입니다. 받는 값을 예상할 수 없을 때 사용을 하며, 가능하면 난발하는 것을 자제해야 하는 타입이기도 합니다.

어떤 측면으로 보면 Object와 비슷한 기능을 할 수 있을 거 같다고 생각이 듭니다만,
Object는 Object 타입의 속성을 명확하게 알려주는 반면, Any는 아무것도 알려줄 수 없습니다.

```tsx
let notSure: any = 4;
notSure.ifItExists(); // 성공, ifItExists 는 런타임엔 존재할 것입니다.
notSure.toFixed(); // 성공, toFixed는 존재합니다. (하지만 컴파일러는 검사하지 않음)

let prettySure: Object = 4;
prettySure.toFixed(); // 오류: 프로퍼티 'toFixed'는 'Object'에 존재하지 않습니다.
```


<br>
<br>

## void
---

주로 함수의 반환문을 정의할 때 많이 쓰입니다. 그 이유가, void는 undefined와 null 두 가지 타입을 다 수용해 줄 수 있기 때문입니다. 

```tsx
let unusable: void = undefined;
unusable = null; // 성공  `--strictNullChecks` 을 사용하지 않을때만
```


<br>
<br>

## Null and Undefined
---

두 타입 다 모르는 값을 표기할 때 사용하는 타입입니다.

조금 다른 점이 void의 경우 null, undefined 모두 수용 가능하지만, null의 경우는 void를 수용하지 못합니다. 이는 아마 javascript의 null 구조 때문에 그런 거 같습니다. 기회가 되면 자세하게 풀어보겠습니다.


<br>
<br>

## Never
---

Never은 주로 Error에 관하거나 값을 도출할 수 없는 경우 쓰이는 타입입니다.
Never은 특이 케이스로 void, null, undefined 전부 수용할 수 없습니다.

```tsx
// never를 반환하는 함수는 함수의 마지막에 도달할 수 없다.
function error(message: string): never {
    throw new Error(message);
}

// 반환 타입이 never로 추론된다.
function fail() {
    return error("Something failed");
}

// never를 반환하는 함수는 함수의 마지막에 도달할 수 없다.
function infiniteLoop(): never {
    while (true) {
    }
}
```

<br>
<br>

## 타입 단언(Type assertions)
---

타입을 조작하는 방식인 거 같습니다. 자세한 건 알아봐야 할 거 같습니다.

타입 단언은 2가지 형태로 구사할 수 있습니다.

1. angle-bracket
    
    ```tsx
    let someValue: any = "this is a string";
    
    let strLength: number = (<string>someValue).length;
    ```
    
2. as 문법
    
    ```tsx
    let someValue: any = "this is a string";
    
    let strLength: number = (someValue as string).length;
    ```
    

두 가지 다 사용가능하지만, React같은 jsx와 같이 사용하는 경우는 as 스타일만 가능합니다.