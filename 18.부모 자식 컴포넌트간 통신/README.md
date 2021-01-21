## 컴포넌트간 통신
컴포넌트 인스턴스는 각자의 스코프를 가짐.  
스코프 내부의 데이터와 메서드는 this를 이용하여 접근
```
this.message
this.update()
```

## 컴포넌트간 데이터 공유법
1. props와 사용자 정의 이벤트를 사용해서 부모 자식끼리 통신  
2. 부모 자식 관계가 아닌 경우 이벤트 버스 사용  
3. Vuex를 사용한 상태 관리

## 부모 자식 컴포넌트

템플릿에서 다른 컴포넌트를 사용하면 부모 자식 관계가 생성된다.  
루트 인스턴스를 부모로 갖는 경우에도 루트 컴포넌트를 부모 컴포넌트라고 한다.

```
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/lodash.js/4.17.20/lodash.min.js"></script>


<!-- my-component의 부모는 루트 인스턴스 -->
<div id="app">
  <my-component></my-component>
</div>


<script>
Vue.component('comp-child', {
  data: function() {
    return {
      message: "child"
    }
  },
  template: '<p>{{message}}</p>',
})

Vue.component('my-component', {
  //data는 객체를 리턴하는 함수로 지정
  data: function () {
    return {
      message: 'parent'
    }
  },
  //comp-child의 부모는 my-component
  template: '<p>{{message}}<comp-child></comp-child></p>',
  mathods: {
    //메서드, 산출 속성, 워처의 정의 방법은
    //루트 생성자 객체와 같음
  }
})

const app = new Vue({ el: '#app' })

</script>

```

## 부모 자식끼리의 데이터 흐름

- 부모에서 자식으로 데이터 전달
  - 부모[속성] -> 자식[props]
- 자식에서 부모로 데이터 전달
  - 자식[$emit] -> 부모[on]

## 부모에서 자식으로 데이터 흐름
### 부모에서 자식으로 데이터 전달
부모 컴포넌트의 템플릿에서 자식 컴포넌트를 사용할 때,  
속성으로 컴포넌트에 데이터를 가지도록 할 수 있음

```
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/lodash.js/4.17.20/lodash.min.js"></script>


<!--
  자식 A
  자식 B 출력
-->
<div id="app">
  <my-component></my-component>
</div>


<script>
//자식 컴포넌트
Vue.component('comp-child', {
  data: function() {
    return {
      message: "child"
    }
  },
  template: '<p>{{val}}</p>',
  props: ['val']
})

//부모 컴포넌트
Vue.component('my-component', {
  data: function () {
    return {
      message: 'parent'
    }
  },
  // val이라는 속성으로 단순한 문자열을 가지게함
  template: '<p>'+
            '<comp-child val="자식A"></comp-child>'+
            '<comp-child val="자식B"></comp-child>'+
            '</p>',
  mathods: {
    //메서드, 산출 속성, 워처의 정의 방법은
    //루트 생성자 객체와 같음
  }
})

const app = new Vue({ el: '#app' })

</script>
```

### 부모컴포넌트의 리액티브 데이터를 자식 컴포넌트에 전달
```
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/lodash.js/4.17.20/lodash.min.js"></script>

<div id="app">
  <my-component></my-component>
</div>


<script>
//자식 컴포넌트
Vue.component('comp-child', {
  data: function() {
    return {
      message: "child"
    }
  },
  template: '<p>{{val}}</p>',
  props: ['val']
})

//부모 컴포넌트
Vue.component('my-component', {
  data: function () {
    return {
      message: 'parent'
    }
  },
  // v-bind:속성="리액티브 속성"으로 자식에게 리액티브 데이터 전달
  template: '<p>'+
            '<comp-child v-bind:val="message"></comp-child>'+
            '</p>',
  mathods: {
  }
})

const app = new Vue({ el: '#app' })

</script>

```

v-for를 이용해 리스트를 반복해서 렌더링도 가능
```
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/lodash.js/4.17.20/lodash.min.js"></script>


<div id="app">
  <ul>
    <comp-child v-for="item in list"
      v-bind:key="item.id"
      v-bind:name="item.name"
      v-bind:hp="item.hp"></comp-child>
  </ul>
</div>


<script>
//자식 컴포넌트
Vue.component('comp-child', {
  template: '<li>{{ name }} HP.{{ hp }}</li>',
  props: ['name', 'hp']
})

//부모 컴포넌트
new Vue({
  el: '#app',
  data: {
    list: [
      { id: 1, name: '슬라임', hp: 100 },
      { id: 2, name: '고블린', hp: 200 },
      { id: 3, name: '드래곤', hp: 500 }
    ]
  }
})

</script>

```

**주의!**  
**props는 리액티브 상태이므로 부모 쪽에서 데이터를 변경하면 자식 쪽의 상태도 변경된다.**  
메서드 내부에서는 this를 사용해서 자기 자신의 데이터처럼 사용할 수 있다.

```
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/lodash.js/4.17.20/lodash.min.js"></script>


<div id="app">
  <ul>
    <comp-child v-for="item in list"
      v-bind:key="item.id"
      v-bind:name="item.name"
      v-bind:hp="item.hp"></comp-child>
  </ul>
</div>


<script>
//자식 컴포넌트
Vue.component('comp-child', {
  template: '<li>{{ name }} HP.{{ hp }}<button @click="doAttack">공격</button></li>',
  props: ['name', 'hp'],
  methods: {
    doAttack: function() {
      //props 접근
      alert(this.name + "이 타격을 입었습니다");
      this.hp -= 10;
    }
  }
})

//부모 컴포넌트
new Vue({
  el: '#app',
  data: {
    list: [
      { id: 1, name: '슬라임', hp: 100 },
      { id: 2, name: '고블린', hp: 200 },
      { id: 3, name: '드래곤', hp: 500 }
    ]
  }
})

</script>

```

props로 받을 자료형을 지정할 수 있다.  
지정한 자료형 이외의 값이 들어올 경우, 경고도 출력되므로 문제가 발생했을 경우,  
쉽게 찾을 수 있다.

|자료형|	설명|	예|
|---|---|---|
|String|	문자열|	'1'|
|Number|	숫자|	1|
|Boolean|	불|	true, false|
|Function|	함수|	function() {}|
|Object|	객체|	{ name: 'foo' }|
|Array|	배열|	[1, 2, 3], [{ id: 1 }, { id: 2 }]|
|생성자| 함수	인스턴스|	new Cat()|
|null|	모든 자료형|	1, '1', [1]|

```
//자료형 확인을 생략하는 경우
Vue.component('example', {
  props: ['value']
})

//자료형만 하는 경우
Vue.component('example', {
  props: {
    value1: String,
    value2: [String, Number],  //멀티 타입으로 자료형을 받음
    value3: {
        type: String,
        required: true  //반드시 포함해야함
    },
    value4: {
        type: Number,
        default: 1000 //디폴트값
    },
    //객체와 배열의 디폴트 값
    //팩토리 함소를 이용하여 반환하는 형태를 사용
    value5: { 
      type: Object,
      default: function() {
        return {message: 'hello'}
      }
    },
    //사용자 정의 유효성 검사함수 - value6 프로퍼티에 대한 유효성 검사
    value6: {
      validator: function(value) {
        return value > 10
      }
    }
  }
})

//인스턴스 확인
function Cat(name) {
  this.name = name
}
Vue.component('example', {
  props: {
    value: Cat // 고양이 데이터만 허가
  }
})
new Vue({
  data: {
    value: new Cat('구름') // value는 고양이 데이터
  }
})

<example v-bind:value="value"></example>
```

## 자식에서 부모로 데이터흐름

자식 컴포넌트의 상태에 따라서 부모 컴포넌트에서 어떤 액션을 실시하도록 처리하거나,  
자식 컴포넌트가 가진 데이터를 부모 컴포넌트에 전달하고 싶을 때는 사용자 정의 이벤트와 $emit이라는 인스턴스 메서드를 사용  
  
자식[$emit] -> 부모[on]
```
//부모 코드
<child-comp v-on:childs-event="parentsMethod">

//자식 코드
this.$emit('childs-event')
```

### 자식 이벤트를 부모에서 캐치
```
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/lodash.js/4.17.20/lodash.min.js"></script>


<div id="app">
    <comp-parent></comp-parent>
</div>


<script>
//자식 컴포넌트
Vue.component('comp-child', {
  template: '<button v-on:click="handleClick">이벤트 호출하기</button>',
  methods: {
    handleClick: function() {
      this.$emit('childs-event') //comp-child의 v-on:childs-event
    }
  }
})

//부모 컴포넌트
Vue.component('comp-parent', {
  template: '<comp-child v-on:childs-event="parentsMethod"></comp-child>',
  methods: {
    parentsMethod: function() {
        alert('이벤트를 받았습니다')
    }
  }
})


new Vue({
  el: '#app'
})

</script>

```

### 부모가 가진 데이터 조작하기
$emit의 두번째 파라미터 부터 부모 이벤트 함수의 파라미터로 전달 
```
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/lodash.js/4.17.20/lodash.min.js"></script>


<div id="app">
    <comp-parent></comp-parent>
</div>


<script>
//자식 컴포넌트
Vue.component('comp-child', {
  template: '<li>{{name}} HP.{{hp}} <button v-on:click="doAttack">공격하기</button></li>',
  props: {id:Number, name:String, hp:Number},
  methods: {
    doAttack: function() {
      //$emit의 파라미터가 v-on:attack="handleAttack" 파라미터로 바인딩
      this.$emit('attack', this.id)
    }
  }
})

//부모 컴포넌트
Vue.component('comp-parent', {
  template: '<ul>'+
            '<comp-child v-for="item in list"'+
            'v-bind:key="item.id" v-bind="item"'+
            'v-on:attack="handleAttack">'+
            '</comp-child>'+
            '</ul>',
  data: function() {
    return {
      list: [
        { id: 1, name: '슬라임', hp: 100 },
        { id: 2, name: '고블린', hp: 200 },
        { id: 3, name: '드래곤', hp: 500 }
      ]
    }
  },
  methods: {
    handleAttack: function(id) {
      const item = this.list.find(function(el) {
        return el.id === id;
      })

      if (item !== undefined && item.hp > 0) {
        alert(`${item.name}이 공격받았습니다.`)
        item.hp -= 10
      }
    }
  }
})


new Vue({
  el: '#app'
})

</script>

```