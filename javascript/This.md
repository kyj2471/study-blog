---

# JavaScript에서 This란 뭔가요?

this에 대해 이해하기위해 여러 책을 읽고 내린 this에 대한 결론은 `this는 현재 실행중인 함수의 대상 객체를 참조하는 키워드이다. 함수로 호출, 메서드로서 호출이 됨에 따라 대상 객체가 달라지게 됨으로 this의 값은 달라진다.`

# This?

this는 실행 컨텍스트가 생성 될 때 결정된다. 그리고 실행 컨텍스트는 함수를 호출할 때 생성된다. 즉, this는 일반적으로 실행 컨텍스트가 생성 될 때 결정된다. 그리고 this는 어디에나 있다. 그렇다면 전역 공간에서 this가 무엇일지 브라우져 콘솔창을 통해 확인해보면 this는 전역 객체(window)를 가르킨다.

![스크린샷 2024-03-28 오후 8.33.21.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/ad79c095-c62e-4268-8fe9-c9d202ae92f5/13b9f7be-4274-4baf-b056-5094b67c827a/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-03-28_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_8.33.21.png)

```jsx
var a = 1;
console.log(this.a) // 1
console.log(window.a) // 1
console.log(a) // 1
```

위의 상황을 보면 모든 log의 값이 1입니다. 이유는 전역 변수를 선언하면 자바스크립트 엔진이 이를 전역 객체의 프로퍼티에 할당하기 때문입니다. 마지막 console.log(a)만 호출해도 1이 결과값인 이유는 `식별자 a를 조회할 때 스코프 체인상 a를 검색하다 도달하는 전역 스코프의 렉시컬 환경인 전역 객체에 도달`하기 때문입니다.

반면 위의 상황에서 이전 실행 컨텍스트 포스팅에서 설명했던 내용인 let/const 키워드를 사용해 선언한 경우 this, window를 통해 접근하면 undefined가 호출됩니다. 이전 포스팅에 있지만 간단하게 설명하면 var는 전역 스코프에서 전역 객체의 프로퍼티로 추가되나 `let/const는 전역 스코프에 있을 때 전역객체의 프로퍼티로 추가되지 않고 블록 스코프를 가지며 그 스코프 내에서만 유지`되기 때문입니다.

### 메서드로 호출할 때 this vs 함수로 호출될 때의 this

앞서 함수와 메서드의 차이를 알아야한다. 이 둘의 차이는 `독립성`으로 함수는 그 자체로 독립적으로 기능을 수행하는 반면 메서드는 자신을 호출한 대상객체에 관한 동작을 수행한다

간단하게 아래의 코드를 보자

```jsx
var func = function(x){
  console.log(this,x)
}

func(1) // window {...} 1

var obj = {
  method: func
}

obj.method(2) // {method:f func()} 2
```

서로 다른 this의 결과가 확인되었습니다. this는 본인을 호출한 주체에 대한 정보가 담기게 되는데 메서드로 호출한 경우 점 표기법에서 확인 할 수있듯 obj가 주체가 되고 this는 obj가 됩니다.

반면 함수로 호출된 경우 this는 전역 객체입니다. 어떤 함수를 함수로서 호출 할 때 this가 지정되지 않습니다. this는 호출한 주체에 대한 정보가 담긴다고 했었는데 함수로서 호출한 경우는호출 주체가 정해지지 않았습니다. 실행 컨텍스트가 활성화 될 때 this가 지정되지 않은 경우 this는 전역객체를 바라봅니다. 이로 인해 실행 컨텍스트의 스코프 체인에 의해 this는 전역객체를 바라보게 되는 것 입니다.

그렇다면 함수와 메서드의 구분이 명확하지 않은 JavaScript에서 이 둘을 구별하는 방법은 뭘까? 간단하게 `점`이다. 도트 체이닝(물론 대괄호를 통해서 실행해도 같다.)을 통해 실행된 함수의 경우는 이 함수를 실행시킨 대상객체가 도트체이닝 전 지정되어있다. 이런 경우에는 `메서드` 그렇지 않은 경우는 `함수`로 실행된 것이며 this는 전역객체를 바라본다.

### this 우회하는 방법

그렇다면 이 this를 우회하려면 어떻게 해야할까 궁금해 집니다. 이런 경우(호출 주체가 없는 경우) 실행 컨텍스트의 변수가 스코프체인되는 점을 이용해 함수 호출시 주변 this를 상속받는 게 좋을것 같습니다.

```jsx
var obj = {
  outer: function(){
    console.log(this) // {outer:f}
    var innerFunc = function(){
      console.log(this) // window
    }
    
    innerFunc()
  }
}
obj.outer()
```

`obj.outer()` 점 표기법을 통해 3번째 줄의 this는 obj로 출력됩니다. 그리고 함수로 호출된 innerFunc함수 내의 this는 전역객체가 됩니다. 이제 이 경우를 위에서 말한 실행 컨텍스트에서 변수가 스코프체인이 된다는 점을 활용해 this를 우회 해보겠습니다.

```jsx
var obj = {
  outer: function(){
    console.log(this) // {outer:f}
    var innerFunc = function(){
      console.log(this) // window
    }
    innerFunc()
    
    var self = this
    var innerFunc2 = function(){
      console.log(self) // {outer:f}
    }
    innerFunc2()
    
  }
}
obj.outer()
```

이처럼 같은 함수로서의 호출이지만 self 식별자가 해당 스코프에 정의되어 있지 않으니 스코프 체인을 통해 한단계위의 스코프를 조회할 것입니다. 상위 스코프에서의 this는 obj객체를 가르키고있고 innerFunc2의 로그값 역시 해당 값을 참조하니 this가 obj가 됩니다.

### this를 바인딩 하지 않는 화살표 함수

ES6에서 this가 전역객체를 바라보는 문제를 해결하기위해 화살표 함수가 나왔습니다.

```jsx
var obj = {
  outer: function(){
    console.log(this) // {outer: f}
    var innerFunc = () => {
      console.log(this) // {outer: f}
    }
    innerFunc()
  }
}
obj.outer()
```

화살표 함수는 실행 컨텍스트 생성시 this바인딩 과정 자체가 빠져 `상위 스코프의 this`를 그대로 활용할 수 있습니다. 위에서 말한 ***“화살표 함수에서 this는 함수가 속한 상위 this를 받는다”***의 말과 동일합니다.

### 콜백 함수, 생성자 함수 내부에서의 this

콜백 함수에서는 제어권을 가지는 함수가 this를 무엇으로 할지 결정됩니다. 정해지지 않은경우에는 역시 전역객체를 바라봅니다. 콜백 함수 역시 함수임으로 this는 전역객체가 될 것 입니다. 하지만 콜백 함수도 제어권을 받은 함수에서 콜백 함수에 별도로 this의 대상을 지정한 경우는 그 대상을 참조합니다.

### 생성자 함수 내부 this

공통된 속성의 객체를 생성할 떄 생성자 함수를 사용한다(프로토타입 포스팅 보러가기). 어떤 함수가 생성자 함수로 호출된 경우 내부에서의 this는 새로 만들어진 구체적인 instance가 된다. new 명령어를 통해 생성자 함수를 생성하면 생성자의 prototype 프로퍼티를 참조하는 __proto__프로퍼티가 있는 객체인 인스턴스가 생성되며 이 때 공통 속성을 this에 할당한다.

```jsx
const Person = function(name, age){
  this.meet = 'hi';
  this.name = name;
  this.age = age
}

const instance = new Person('tony',30)
console.log(instance)

-> instance log결과

Person {
  meet: 'hi',
  name: 'tony',
  age: 30,
  __proto__: { constructor: ƒ Person() }
}
```

이와 같이 instance의 로그 결과는 Person 클래스의 인스턴스 객체가 출력이됨을 확인할 수 있다.

### 명시적으로 this를 바인딩

**call 메서드** call 메서드는 호출 주체 함수를 즉시 실행하도록 한다. 이때 첫 번째 인자를 this바인딩한다. 이후 인자는 매개변수이다.

**apply 메서드** call 메서드와 기능적으로 동일하나 call은 첫번째 인자를 thisbinding 이후 인자를 매개변수로 받으나 apply메서드는 두번째 인자를 배열로 받아 호출할 함수의 매개변수로 지정한다.

bind 메서드

bind 메서드는 call과 비슷하나 즉시 호출이 아닌 넘겨받은 this와 인수를 새로운 함수로 반환하기만 하는 함수이다.

### 화살표 함수

화살표 함수는 실행 컨텍스트 생성시 thisbinding하는 과정이 제외되었다. 이로 인해 함수 내부에 this가 없으며 그렇기에 this에 접근하고자 하면 스코프체인에서 가장 가까운 this에 접근하게된다.