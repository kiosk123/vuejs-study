# 03. 기본 옵션

## 기본 옵션 구성

el : 마운트할 엘리먼트의 셀렉터  
data : 어플리케이션에서 사용할 데이터. - 객체, 배열 등..  
computed : 함수로 인해 산출되는 데이터  
created : 라이프 사이클 hook - Vue 인스턴스 라이프 사이클 중에 등록되어 특정 시점에 실행되는 콜백  
|  메서드 | 시점  |
|---|---|
|   |   |
|   |   |
|   |   |

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

