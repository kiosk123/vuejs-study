## $el과 $refs
$el과 $refs는 DOM의 위치와 높이를 알려면 DOM에 직접 접근해야하는데 이럴때 사용  
DOM을 참조해야 사용할 수 있기 때문에, 라이프 사이클 중 mounted 이후 부터 사용 가능  

$el과 $refs는 가상DOM을 사용해서 접근하는 것이 아니기 때문에 이것을 이용해서 텍스트를 변경해도  
가상DOM에서 변경이 일어나면 가상DOM 값으로 덮어쓰게됨