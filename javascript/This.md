# This

---

자바스크립트를 공부하면서 가장 혼란스러운 개념중 하나인 this를 다뤄 보겠다. 왜 자바스크립트에서의 this가 혼란스러울까 생각해보면 이 this를 어디에서나 사용할 수있고 상황에 따라 this가 바라보는 대상이 달라지는게 큰 이유인 것 같다.
# This?

this에 대해 공부하다 내린 결론은 단 두줄이다.

- this는 함수가 호출될 때 결정된다.
- 화살표 함수에서 this는 함수가 속한 상위 this를 받는다.

this는 함수가 호출될 때 결정된다고 했다. 즉, this는 일반적으로 실행 컨텍스트가 생성될 때 결정된다. 그리고 this는 어디에나 있다. 그렇다면 전역 공간에서 this가 무엇일지 브라우져 콘솔창을 통해 확인해보면 this는 전역 객체(window)를 가르킨다.

### 메서드로 호출할 때 this vs 함수로 호출될 때의 this

앞서 함수와 메서드의 차이를 알아야한다. 이 둘의 차이는 독립성으로 함수는 그 자체로 독립적으로 기능을 수행하는 반면 메서드는 자신을 호출한 대상객체에 관한 동작을 수행한다

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

`obj.outer()` 점 표기법을 통해 3번째 줄의 this는 obj로 출력됩니다. 그리고 함수로 호출된 innerFunc함수 내의 this는 전역객체가 됩니다. 이제 이 경우를 위에서 말한 실행 컨텍스트에서 변수가 스코프체인이 된다는 점을 활용해보겠습니다.

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

이처럼 같은 함수로서의 호출이지만 innerFunc2는 self라는 변수에 this를 담아서 사용해서 간단하게 우회하였습니다.

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

화살표 함수는 실행 컨텍스트 생성시 this바인딩 과정 자체가 빠져 상위 스코프의 this를 그대로 활용할 수 있습니다. 위에서 말한 “화살표 함수에서 this는 함수가 속한 상위 this를 받는다”의 말과 동일합니다.

### 콜백 함수, 생성자 함수 내부에서의 this

콜백 함수에서는 제어권을 가지는 함수가 this를 무엇으로 할지 결정됩니다. 정해지지 않은경우에는 역시 전역객체를 바라봅니다.

생성자 함수에서의 this는 생성된 인스턴스를 가르킵니다.

### 명시적으로 this를 바인딩

call, apply 메서드를 통해 this를 명시적으로 지정하며 함수나 메서드를 실행할 수있습니다.
bind메서드는 call과 비슷하지만 넘겨받은 this 및 인수를 바탕으로 새로운 함수를 반환합니다.