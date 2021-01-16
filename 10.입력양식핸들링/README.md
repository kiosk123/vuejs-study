## input
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