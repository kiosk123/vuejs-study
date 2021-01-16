## 이벤트 장식자 - DOM 이벤트의 기본적인 동작을 변경
|  **장식자** | **내용**  |
|---|---|
| **.stop**  | event.stopPropagation() |
| **.prevent**  | event.preventDefault() |
| **.capture**  | 캡처모드로 DOM 이벤트를 핸들 |
| **.self**  | 이벤트가 해당 요소에서 직접 발생할 때만 핸들러를 호출 |
| **.native**  | 컴포넌트의 루트 요소 위에 있는 네이티브 이벤트를 핸들 |
| **.once**  | 한 번만 핸들 |
| **.passive**  | {passive:true} 로 DOM 이벤트를 핸들|

### .capture
이벤트를 갭처 모드로 발생. 루트 요소에서 이벤트 타겟 요소까지 DOM 트리를 찾아 내려가는 캡처를 할 때에 이벤트 발생  
이벤트 버블링 보다 먼저 이벤트 발생  

```
<div @click.capture="handler('div1')">
    div1
    <div @click="handler('div2')">
        div2
        <div @click="handler('div3')">
            div3
        </div>
    </div>
</div>

<!-- div3 를 클릭했을 때 출력 결과-->
div1
div3
div2
```

### .self
event.target 요소가 자기 자신일 때만 핸들러가 호출  
주로 모달 대화 상자의 배경 부분을 클릭해서 닫는 경우 등에 사용

```
<div class="overlay" @click.self="close">...</div>
```

### .native
DOM 이벤트라도 $emit를 사용하지 않으면 호출되지 않도록 함  
이벤트를 직접 호출하고 싶은 경우에 사용

```
<!--컴포넌트를 클릭하면 핸들러가 호출됨 -->
<my-component @click.native="handler"></my-component>

<!--컴포넌트를 클릭하더라도 핸들러가 호출되지 않음 -->
<my-component @click="handler"></my-component>
```

### .passive
event.preventDefault()를 사용하지 않겠다고 명시. 따라서 .prevent 장식자와 함께 사용할 수 없음.  
모바일 환경에서 비용이 높은 처리를 할때 

## 클릭이벤트 

|  **장식자** | **내용**  |
|---|---|
| **.left**  | 마우스 왼쪽 버튼으로 눌렀을 때 |
| **.right**  | 마우스 오른쪽 버튼 눌렀을때 |
| **.middle**  | 마우스 중간 버튼 눌렀을때 |

```
<!-- 클릭이벤트 예시 -->
<div @click.right="handler"> example </div>
```

## 키이벤트
|  **장식자** | **내용**  |
|---|---|
| **.enter**  | Enter 키를 눌렀을때 |
| **.tab**  | tab 키를 눌렀을때 |
| **.delete**  | delete키를 눌렀을 때 |
| **.esc**  | esc키를 눌렀을때 |
| **.space**  | 스페이스 키를 눌렀을때 |
| **.up**  | 위쪽 화살표 키를 눌렀을때 |
| **.down**  | 아래쪽 화살표 키를 눌렀을때|
| **.left**  | 왼쪽 화살표 키를 눌렀을때|
| **.right**  | 오른쪽 화살표 키를 눌렀을때|

## 시스템 장식자
시스템 장식자는 대응키가 눌린 경우에만 핸들러를 호출함

| **.ctrl**  | ctrl 키를 눌린경우 |
| **.alt**  | alt 키를 눌린경우 |
| **.shift**  | shift키를 눌린경우 |
| **.meta**  | meta키를 눌린경우 |