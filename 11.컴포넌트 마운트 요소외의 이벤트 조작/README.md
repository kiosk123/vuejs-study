컴포넌트를 마운트한 요소 내부에서 발생하는 이벤트는 v-on을 사용해서 핸들링 할 수 있다.  
하지만 window와 body에는 v-on을 사용할 수 없다.  
  
그렇기에 **window 객체는 addEventListener 메서드를 사용해야 하지만, v-on과 다르게**  
**필요가 없어지는 경우에 핸들러가 자동으로 제거되지 않는다**.  
  
따라서 불필요해지는 경우가 발생하면 사전에 훅을 해서 핸들러를 제거해야한다.

## 스크롤 이벤트 추출
스크롤 처럼 발생 빈도가 높은 이벤트는 타이머를 이용해서 처리 실행 빈도를 확인하는 형태로 사용하는 것을 권장.  
다음 코드는 window의 스크롤 이벤트를 핸들해서 200ms 간격으로 scrollY 속성을 변경한다.
```
// 스크롤 수치를 추출
new Vue({
    el: '#app',
    data: {
        scrollY: 0,
        timer: null
    },
    created: function() {
        // 핸들러 등록
        window.addEventListener('scroll', this.handleScroll);
    },
    beforeDestroy: function() {
        // 핸들러 제거
        window.removeEventListener('scroll, this.handleScroll)
    },
    methods: {
        handleScroll: function() {
            if (this.timer === null) {
                // setTimeout 내부의 this가 Vue인스턴스를 참고할수 있도록 bind한다
                this.timer = setTimeout(function() {
                    this.scrollY = window.scrollY;
                    clearTimeout(this.timer);
                    this.timer = null;
                }.bind(this), 200);
            }
        }
    }
})

//...
<div id="app">
    <header :class="{compact: scrollY > 200}">
        200px 이상 스크롤했으면, .compact 클래스 추가
    </header>
</div>
```

## 스무스 스크롤 구현하기
페이지 가장 위로 부드럽게 이동하는 스무스 스크롤을 구현할 때는 window 객체를 조작해야한다.  
다음은 [라이브러리](https://github.com/cferdinandi/smooth-scroll)를 이용해서 스무스 스크롤을 사용하는 예제다.

```
<script src="https://cdn.jsdelivr.net/gh/cferdinandi/smooth-scroll/dist/smooth-scroll.polyfills.min.js"></script>
<div id="app">
    <div class="content">...</div>
    <div v-on:click="scrollTop">
        페이지 상단으로 이동하기
    </div>
</div>

//...
const scroll = new SmoothScroll();
new Vue({
    el: '#app',
    methods: {
        scrollTop: function() {
            scroll.animateScroll(0);
        }
    }
})
```

