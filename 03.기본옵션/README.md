# 03. 기본 옵션

## 기본 옵션 구성

el : 마운트할 엘리먼트의 셀렉터  
data : 어플리케이션에서 사용할 데이터. - 객체, 배열 등..  
computed : 함수로 인해 산출되는 데이터  
created : 라이프 사이클 hook - Vue 인스턴스 라이프 사이클 중에 등록되어 특정 시점에 실행되는 콜백  
|  **메서드** | **실행시점**  |
|---|---|
| beforeCreate  | 인스턴스가 생성되고, 리액티브 초기화가 일어나기 전 - created와의 차이는 리액티브 데이터의 초기화 전에 호출  |
| created  | 인스턴스가 생성되고, 리액티브 초기화가 일어난 후 - 인스턴스 자신을 나타내는 this에는 접근가능 하지만 DOM이 구축되기 전이기 때문에 DOM접근은 불가 (내부에서 $el, getElementById()등 사용 불가)   |
| beforeMount  | 인스턴스가 마운트되기 전 - DOM에 접근 불가능  |
| mounted  | 인스턴스가 마운트된 후 - 인스턴스의 상태를 사용해서 DOM을 만든 직후에 호출 (내부에서 $el, getElementById()등 사용 가능), **그렇다고 모든 자식 컴포넌트가 마운되었다는 보증은 하지 않음**  |
| beforeUpdate  |  데이터가 변경되어 DOM에 적용되기 전 |
| updated  |  데이터가 변경되어 DOM에 적용된 후 |
| beforeDestory  | Vue 인스턴스가 제거되기 전  |
| destroyed  | Vue 인스턴스가 제거된 후  |
| errorCaptured  | 임의의 자식 컴포넌트에서 오류가 발생했을 때  |

methods : 애플리케이션에서 사용할 메서드 데이터처리나, 이벤트 핸들러 구현등에 사용

```
const app = new Vue({
    el: '#app',
    data: {
        message: 'hello world!'
    },
    computed: {
        computedMessage: function() {
            return this.message + '!';
        }
    },
    created: function() {
        //...
    },
    methods: {
        myMethod: function() {
            //...
        }
    }
});
```

