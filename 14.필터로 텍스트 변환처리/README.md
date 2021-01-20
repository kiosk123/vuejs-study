## 필터
문자 수를 반올림 하거나 쉼표를 넣는 등의 텍스트 기반 변환 처리에 특화된 기능  
필터는 로컬 또는 전역에 등록해서 사용.  
**로컬에 등록한 경우라도 this는 사용하지 않으므로 주의**

### 사용방법
등록한 필터는 {{}} 또는 v-bind 값 뒤에 | 로 연결해서 사용

```
{{ 대상 데이터 | 필터이름}}

<div v-bind:id="대상 데이터 | 필터 이름"></div>
```

### 로컬 필터
컴포넌트의 filters 옵션에 등록하면 특정 컴포넌트 내부에서만 사용가능

```
new Vue({
  el: '#app',
  data: {
    price: 19800
  },
  filters: {
    localeNum: function (val) {
      return val.toLocaleString()
    }
  }
})

{{ price | localeNum}} 원   <!--결과 값 19,800 -->
```

### 전역 필터 
범용적인 필터는 전역 메서드 Vue.filter를 사용해서 등록  
모든 컴포넌트에서 참조 가능

```
Vue.filter('localeNum', function(val) {
    return val.toLocaleString()
})
```

### 필터에 매개변수 전달

다음과 같이 정의할 경우 filter의 첫번째 파라미터는 message속성값 두번째는 foo속성값 마지막은 100이다
```
{{message | filter(foo, 100)}}

filters: {
    filter: function(message, foo, num) {
        console.log(message, foo, num)
    }
}
```

### 필터 여러개 연결하기

```
new Vue({
  el: '#app',
  filters: {
    // 소수점 이하 두 번째 자리까지 끊는 필터
    round: function (val) {
      return Math.round(val * 100) / 100
    },
    // 도 단위를 라디안 단위로 변환하는 필터
    radian: function (val) {
      return val * Math.PI / 180
    }
  }
})
180도는 {{ 180 | radian | round }} 라디안입니다 .
```