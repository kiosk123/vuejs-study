## nextTick
가상 DOM이 비동기적으로 DOM을 변경하는 형태라서 매우 짧은 랙이 발생  
따라서 데이터를 변경하고 DOM에 접근해도 변경이 일어나기 전이라 DOM을 못찾거나 에러가 발생할 수 있음  

반영된 DOM에 접근하려면 DOM 변경을 잠시 기다리기 위한 기능으로 nextTick을 사용해야한다.

### 사용방법
```
this.$nextTick(function() {
  // DOM 변경 후 데이터 조작
})
```

### DOM 변경 후 높이 확인하기
```
<button v-on:click="list.push(list.length+1)">추가</button>
<ul ref="list">
  <li v-for="item in list">{{ item }}</li>
</ul>
new Vue({
  el: '#app',
  data: {
    list: []
  },
  watch: {
    list: function () {
      // 이렇게 해서는 변경 후 ul 태그의 높이를 추출할 수 없음
      console.log('기본 출력:', this.$refs.list.offsetHeight)
      // nextTick을 사용하면 할 수 있어요!
      this.$nextTick(function () {
        console.log('nextTick:', this.$refs.list.offsetHeight)
      })
    }
  }
})
```

주의점은 웹 폰트 또는 이미지가 포함된 경우 이러한 로드는 대기하지 않는다.  
이미지 로드를 가진 콜백 함수 내부에서 nextTick을 사용하거나, 이미지의 대략적인 높이를 지정하는 형태로 사용해야한다.  