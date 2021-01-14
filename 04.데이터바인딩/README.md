# 04. 데이터 바인딩

컴포넌트의 data 옵션에 문자열 또는 객체 등의 데이터를 정의하면, 인스턴스 생성때 모두 리액티브 데이터로 변환된다.  
```
    <div id="app">
       {{message}}
    </div>

    let app = new Vue({
        el: '#app',
        data: {
            message: 'Hello Vue.js' //message 변화 감지
        }
    })

```

옵션에서 외부 데이터를 정의해도 Vue 인스턴스의 데이터로 등록하면 리액티브 데이터로 변경을 감지할 수 있다.

```
    <div id="app">
       {{state.count}}
    </div>

    const state = { count: 0 }; //Vue 인스턴스 외부에 state 객체 선언
    let app = new Vue({
        el: '#app',
        data: {
            state: state
        }
    })

    setInterval(function() {
        state.count++;
    }, 2000);

```

data 옵션 바로 아래의 속성은 인스턴스 생성 이후에 따로 추가 할 수 없기애, 값이 결정되지 않은 경우라도 초깃값을 넣어서 정의한다.
```
    let app = new Vue({
        el: '#app',
        data: {
            newTodoText: '',
            visitCount: 0,
            hideCompletedTodos: false,
            todos: [],
            error: null
        }
    })
```