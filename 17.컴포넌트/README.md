## 컴포넌트
### 컴포넌트 정의
Vue.component 를 사용해서 전역에 등록하면 자동으로 모든 컴포넌트에서 사용할 수 있다.
```
Vue.component('my-component', {
  template: '<p>MyComponent</p>'
})
```