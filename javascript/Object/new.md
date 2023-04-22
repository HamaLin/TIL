# new 연산자, 생성자 함수

생성자 함수는 간단하게 보면 Class이다.
생성자를 만들기 위해서는 2가지 조건을 충족해주면 된다.

1. 함수 이름의 첫글자는 대문자이여야 한다.
2. 반드시 new를 붙여준다.

```jsx
function User(name) {
  this.name = name;
  this.isAdmin = false;
}

let user = new User("보라");

alert(user.name); // 보라
alert(user.isAdmin); // false
```

위와 같이 아무것도 return하지 않아도 new를 붙이면 암시적으로 빈 객체와 반환문을 만들어줍니다. 기억하기로는 이러한 문법이 나왔던 이유는 초기 자바스크립트에서는 class가 존재하지 않아 class와 비슷하게 구현하려고 나온 것으로 기억합니다. 하지만 현재 ES6문법에는 class가 존재하니, 이런게 있구나 하고 넘어가는게 좋을 거 같습니다.

<br>
<br>
<br>

# 생성자 return

위에서 생성자는 암시적으로 return을 해줍니다.
하지만 용도에 따라서 커스텀하게 반화하고 싶을 때도 있을겁니다. 다음코드를 예로 생각해봅니다

```jsx
function BigUser() {
  this.name = "원숭이";

  return { name: "고릴라" }; // <-- this가 아닌 새로운 객체를 반환함
}

alert(new BigUser().name); // 고릴라
```

특별한 경우에 사용하면 유용할지 모릅니다만, 많이 사용될지는 잘 모르겠습니다.
