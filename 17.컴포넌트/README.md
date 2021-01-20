## 컴포넌트
### 전역 컴포넌트 등록

**컴포넌트 정의**
Vue.component 를 사용해서 전역에 등록하면 자동으로 모든 컴포넌트에서 사용할 수 있다.
```
Vue.component('button-counter', {
  data: function () {
    return {
      count: 0
    }
  },
  template: '<button v-on:click="count++">You clicked me {{ count }} times.</button>'
})

new Vue({ el: '#components-demo' })
```

**컴포넌트 사용**
```
<div id="components-demo">
  <button-counter></button-counter>
</div>
```

**결과**
```
<div id="components-demo">
  <button v-on:click="count++">You clicked me {{ count }} times.</button>
</div>
```

### 로컬 컴포넌트 등록
컴포넌트 정의를 특정 컴포넌트의 components 옵션에 등록하면,  
로컬로 등록된 해당 컴포너트 스코프 내부에서만 해당 컴포넌트를 사용할 수 있도록 제한할 수 있다.  

**컴포넌트는 루트 인스턴스가 정의되기 전에 정의해야 한다 **
```
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.min.js"></script>

<div id="app">
  <my-component></my-component>
</div>


<script>

//컴포넌트 정의
const myComponent = {
  template: '<p>MyComponent</p>'
}

const app = new Vue({ 
  el: '#app',
  components: {
    'my-component': myComponent
  }
})

</script>

```

### 컴포넌트의 옵션

루트 생성자 new Vue() 옵션과 마찬가지로 컴포넌트 전용 템플릿 이외에데이터와 메서드도 정의 할 수 있다.

```
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/lodash.js/4.17.20/lodash.min.js"></script>


<div id="app">
  <my-component></my-component>
</div>


<script>

Vue.component('my-component', {
  //data는 객체를 리턴하는 함수로 지정
  //여러개의 컴포넌트 인스턴스들이 같은 객체를 참조해서 상태 공유 방지
  data: function () {
    return {
      message: 'Hello Vue js'
    }
  },
  //템플릿의 루트요소는 반드시하나 <p><div></div></p>는 가능하지만 <p></p><div></div>는 불가
  template: '<p>{{message}}</p>',
  mathods: {
    //메서드, 산출 속성, 워처의 정의 방법은
    //루트 생성자 객체와 같음
  }
})

const app = new Vue({ el: '#app' })

</script>

```

### 컴포넌트 인스턴스
다음과 같이 같은 컴포넌트를 여러 번 사용했을 경우, 이러한 것들은 my-component를 기반으로 만들어진 완전히 다른 인스턴스로 취급됨
```
<div id="app">
  <my-component></my-component>
  <my-component></my-component>
</div>
```