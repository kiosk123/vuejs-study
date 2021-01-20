## 워처
특정 데이터 또는 산출 속성의 상태를 감시해서 변화가 있을 때 등록한 처리를 자동으로 실행해 주는 기능
```
new Vue({
    //...
    watch: {
        <감시할 데이터>: function(<새로운 값>, <이전 값>) {
            //value가 변화했을 때 하고 싶은 처리
        },
        'item.value': function(newVal, oldVal) {
            // 객체 속성의 변화를 감지하고 처리
        }
    }
})
```

### deep과 immediate 옵션 활용
```
// deep: true, // 네스트된 객체도 감시할지 여부 - 필요시 설정
// immediate: true //처음 값을 읽어 들이는 시점에서 호출할지 여부 - 필요시 설정
watch: {
    list: {
        handler: function(newVal, oldVal) {
            //list가 변경될 때 하고 싶은 처리
        },
        deep: true,
        immediate: true
    }
}

// 배열 내부의 속성이 바뀌면 호출됨
this.list[0].name = 'NewValue!' 
```

### 인스턴스 메서드로 워처 등록
this.$watch를 사용하여 워처 등록가능

```
created: fucntion() {
    this.$watch('value', function(newVal, oldVal) {
        //...
    })
}
```
인스턴스 메서드를 사용해서 등록할 때는 watch 옵션을 사용해서 등록할 때와 다르게 다음과 같은 정보가 매개변수로 전달됨

```
{
    <감시할 데이터>,
    <핸들러>,
    <옵션 객체>
}
```

created 훅에서 워처 등록하기

```
created: function() {
    this.$watch('value', function() {
        //...
    }, {
        immediate: true
    })
}
```

### 워처 제거
인스턴스 메서드로 등록한 경우, 리턴 값을 사용해서 워처를 제거할 수 있음

```
const unwatch = this.$watch('value', handler);
unwatch() //value 감시 제거
```

### 한번만 동작하는 워처
unwatch를 사용하면 한번만 동작하는 워처를 정의할 수 있다
```
const unwatch = this.$watch('list', function() {
    this.edited = true //리스트가 편집되었는지 기록
    unwatch() 감시제거
}, {deep: true})
```

### 실행 빈도 제어

```
watch: {
    value: function() {
        setInterval(function() {
            // 처리 작성
        }, 1000)
    }
}
```

### 여러 값 감시

```
created: function() {
    this.$watch(function() {
        return [this.width, this.height]
    }, function() {
        // when width or height value is changed
    })
}
```

```
//산출 속성을 감시 - 동작은 위 코드와 동일
computed: {
    watchTarget: function() {
        return [this.width, this.height]
    }
},
watch: {
    watchTarget: function() {...}
}
```

### 객체 자료형의 이전 값을 워처로 이용해서 비교하기
감시 대상이 객체일 경우, 참조로 비교가 일어나므로 oldVal과 newVal이 항상 같아 제대로된 감시를 할 수 없다.
예를 들어서 다음 코드는 배열의 길이는 비교하는 예이다. 하지만 언제나 같은 값을 가진다.

```
watch: {
    list: function(newVal, oldVal) {
        console.log(newVal.length, oldVal.length)
    }
}
```

다음과 같이 감시 대상을 복사 또는 깊은 복사를 하거나 속성을 포함시켜 두면, 배열의 길이를 비교할 수 있다.

```
// 복사 (깊은 복사)
this.$watch(function() {
    return Object.assign([], this.list)
}, function(newVal, oldVal) {
    console.log(newVal.length, oldVal.length)
})
```

```
//대상에 속성 포함시키기
this.$watch(function() {
    return {value: this.list, length:this.list.length}
}, function(newVal, oldVal) {
    console.log(newVal.length, oldVal.length)
})
```

### 입력 양식을 감시하고 API로 데이터 가져오기
토픽 선택 값에 있는 current를 감시하여 선택 값이 바뀔때 워처 처리를 하게 된다.
```
<div id="app">
  <select v-model="current">
    <option v-for="topic in topics" v-bind:value="topic.value">
      {{ topic.name }}
    </option>
  </select>
  <div v-for="item in list">{{ item.full_name }}</div>
</div>

new Vue({
  el: '#app',
  data: {
    list: [],
    current: '',
    topics: [
      { value: 'vue', name: 'Vue.js' },
      { value: 'jQuery', name: 'jQuery' }
    ]
  },
  watch: {
    current: function (val) {
      // 깃허브 API에서 토픽 리포지토리 검색하기
      axios.get('https://api.github.com/search/repositories', {
        params: {
          q: 'topic:' + val
        }
      }).then(function (response) {
        this.list = response.data.items
      }.bind(this))
    }
  },
})
```