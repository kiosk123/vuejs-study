## input
### v-model 적용 예외
<input type="file"> 경우 v-model을 사용할 수 없기 때문에.  
데이터 동기화를 할때는 change 이벤트를 바인딩해서 사용한다.

```
<input type="file" @change="handleChange">
<div v-if="preview"><img v-bind:src="preview"></div>

<script>
    const app = new Vue({
        el: '#app',
        data: {
            preview=''
        },
        methods: {
            handleChange: function(event) {
                const file = event.target.files[0]
                if (file && file.type.match(/^image\/(png|jpeg)$/)) [
                    this.preview = window.URL.createObjectURL(file);
                ]
            }
        }
    });
</script>
```

### 장식자
v-model 디렉티브에는 다음과 같은 장식자를 사용할 수 있다.
|  **장식자** | **내용**  |
|---|---|
| **.lazy**  | input 대신 change 이벤트 핸드링하기 |
| **.number**  | 값을 숫자로 변환 |
| **.trim**  | 값 양쪽에 있는 쓸데없는 공백 제거 |

#### **.lazy**  
입력 양식은 입력이 되는 시점에 동기화되지만, .lazy 사용시 change 이벤트 발생시점 변경  
change 이벤트는 텍스트 입력 양식의 경우, 초점(focus)이 제거되거나 엔터키등을 눌렀을 때 발생

#### **.number**
숫자값으로 변환

```
<input type="text" v-model.number="price"> {{price}}
```
