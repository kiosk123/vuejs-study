## 사용자 정의 디렉티브
v-bind 처럼 디렉티브를 직접 만들 수 있게 해주는 기능  
요소 또는 속성은 데이터 바인딩으로 조작할 수 있지만, DOM API를 사용하고 싶을 수도 있음   
이런 경우에 사용자 정의 디렉티브를 사용하면, 상태를 감시하면서 직접 DOM을 조작할 수 있음  
  
**주의**
사용자 정의 디렉티브에서 받게 되는 요소는 $el 또는 $refs와 같은 가상 DOM이 아니므로, 렌더링 최적화가 따로 들어가지 않음  
기본적인 데이터 바인딩만으로 표현할 수 없는 부분에만 사용 권장

### 사용자 디렉티브 정의
사용자 정의 디렉티브로 등록할 메서드 이름에 v-프리픽스를 붙여서 사용
```
<div v-directive>example</div>
```

사용자 정의 디렉티브의 트리거가 될 데이터를 바인딩 할 수도 있음
```
<!-- value가 변할 때 호출 됨 -->
<div v-directive="value"> example </div>
```

### 로컬에 등록
컴포넌트의 directives 옵션에 등록하면 특정 컴포넌트 내부에서만 사용할 수 있게됨
```
new Vue({
  el: '#app',
  directives: {
    focus: {
      // 연결되어 있는 요소가 DOM에 추가될 때
      inserted: function (el) {
        el.focus() // 요소에 초점을 맞춤
      }
    }
  }
})
<input type="text" v-focus>
```

### 전역에 등록
전역 메서드 Vue.directive를 사용해서 전역에 등록하면, 모든 컴포넌트에서 사용가능
```
Vue.directive('focus', {
  inserted: function(el) {
    el.focus();
  }
})
```

### 사용할 수 있는 훅
|  **메서드** | **실행시점**  |
|---|---|
| **bind**  | 디렉티브가 처음 요소와 연결될때  |
| **inserted**  | 연결된 요소가 부모 노드에 삽일될때 |
| **update**  | 연결된 요소를 내포하고 있는 컴포넌트의 VNode(가상돔)가 변경되었을때  |
| **componentUpdated**  | 내포하고 있는 컴포넌트와 자식 컴포넌트의 VNode(가상돔)가 변경되었을 때  |
| **unbind**  | 연결되어 있는 요소로부터 디렉티브가 제거될 때

update 또는 componentUpdated는 컴포넌트의 가상 DOM이 변경될 때 호출된다.  
워처를 사용한다고 해도 데이터 바인딩이 되지 않은 경우라면 데이터가 변화해도 호출되지 않는다.

```
Vue.directive('example', {
  bind: function (el, binding) {
    console.log('v-example bind')
  },
  inserted: function (el, binding) {
    console.log('v-example inserted')
  },
  update: function (el, binding) {
    console.log('v-example update')
  },
  componentUpdated: function (el, binding) {
    console.log('v-example componentUpdated')
  },
  unbind: function (el, binding) {
    console.log('v-example unbind')
  }
})
```

### 훅 매개변수
각각의 훅에는 다음과 같은 매개변수를 전달할 수 있다.
|  **메서드** | **실행시점**  |
|---|---|
| **el**  | 디렉티브가 연결되어 있는 요소 |
| **binding**  | 바인드된 값, 매개변수, 장식자가 들어 있는 객체 |
| **vnode**  | 요소에 대응되는 VNODE  |
| **oldVnode**  | 변경 이전의 VNode(update 또는 componentUpdated에서만 사용가능)  |

두번째 매개변수에 함수를 전달하면, bind와 update에 훅을 실시한다.
```
Vue.directive('example', function(el, binding, vnode, oldVnode) {
  //bind와 update로 호출
})
```

### 동영상 조작

```
<div id="app">
  <!--동영상1 -->
  <button v-on:click="video1=true">재생</button>
  <button v-on:click="video1=false">중지</button>
  <video src="movie1.mp4" v-video="video1"></video>

  <!--동영상2 -->
  <button v-on:click="video2=true">재생</button>
  <button v-on:click="video2=false">중지</button>
  <video src="movie2.mp4" v-video="video2"></video>
</div>

new Vue({
  el: '#app',
  data: {
    video1: false,
    video2: false,
  },
  // bind와 update일때 훅실시
  directives: {
    video(el, binding) {
      binding.value ? el.play() : el.pause()
    }
  }
})
```
update 훅은 바인드하고 있는 속성의 변화뿐만 아니라 해당 요소를 포함하고 있는 컴포넌트의 가상 DOM에 변화가 있을 때도 호출된다  
따라서 video1 속성이 변화하면 데이터 변화도 발생하므로, 컴포넌트가 다시 렌더링 되어 video2 속성을 바인드하고 있는 요소의 디렉티브도 함께 호출됨 -> 비효율적

### 이전 상태와 비교해서 처리
메서드의 매개변수로 이전 VNode와 이전 바인드 데이터를 받을 수 있다.  
이를 사용해서 이전 데이터와 현재 데이터를 비교하면, 자신이 바인드하고 있는 데이터가 변경되었다고 판단 될 경우에만 렌더링하도록 할 수 있다.

binding 매개변수는 다음과 같은 속성을 가진다.
|  **속성** | **실행시점**  |
|---|---|
| **arg**  | 매개변수 |
| **modifiers**  | 장식자 객체 |
| **value**  | 새로운 값 |
| **oldValue**  | 이전 값(update 또는 componentUpdated에서만 사용 가능) |

동영상 재생 조작하기 코드를 다음과 같이 수정하면 다음과 같다
```
new Vue({
  el: '#app',
  data: {
    video1: false,
    video2: false,
  },
  // bind와 update일때 훅실시
  directives: {
    video(el, binding) {
      if (binding.value !== binding.oldValue) {
        binding.value ? el.play() : el.pause()
      }
    }
  }
})
```