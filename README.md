# toy_todo-list
해당 토이 프로젝트는 유튜브 채널 <Dev Ed> 의 Beginner Vanilla Javascript Project Tutorial를 보고 따라한 것입니다. 현재는 JS로만 구성되어 있으나 코드 리팩토링을 통해 Reat로 리팩토링할 예정입니다.
해당 토이를 유튜브에서 보고 싶다면 https://www.youtube.com/watch?v=Ttf3CEsEwMQ

![ezgif com-gif-maker](https://user-images.githubusercontent.com/87353284/138810042-07e73073-467c-4cb8-b443-6ca3a4731d77.gif)

## function developed
  1. todo 항목 추가, 삭제
  2. todo check 기능(text-decoration: line-through, fall down)
  3. filter - All, completed, uncompleted
  4. 크롬 개발자 도구 localstorgae 활용한 todo list 저장, 삭제



## highlight codes
  
1. JS
  
  (1) ele.classList.toggle
  
  일반적으로 ele.classList.add 가 가장 많이 사용
  이번에 사용된 것은 toggle이다.
  모던 JS 튜토리얼의 정의에 따르면
  **elem.classList.toggle("class") – class가 존재할 경우 class를 제거하고, 그렇지 않은 경우엔 추가**
  이다.
  
  아래 코드는 todo 객체의 class에 completed가 있으면 제거하고 없으면 completed class를 추가하라는 의미이다.
  
  ```   //CHEKCH MARK
    if(item.classList[0] === 'complete-btn') {
        const todo = item.parentElement;
        todo.classList.toggle("completed")
    }
  ```
  
  (2) 조건문 switch case
  
  if 대신 깔끔한 switch case 문을 사용하였다. todos에 todoList의 자식 노드를 넣어주고 그 자식 노드의 값에 따라 CSS 설정을 바꾸어 준 것이다.
  
  ```function filterTodo(e) {
    const todos = todoList.childNodes;
    todos.forEach(function(todo) {
        switch(e.target.value) {
            case 'all':
                todo.style.display = 'flex';
                break;
            case 'completed':
                if(todo.classList.contains('completed')) {
                    todo.style.display = 'flex';
                } else {
                    todo.style.display = 'none';
                }
                break;
            case 'uncompleted':
                if(!todo.classList.contains('completed')) {
                    todo.style.display = 'flex';
                } else {
                    todo.style.display = 'none';
                }
                break;
        }
    });
  ```
  (3) classList에 있는 인덱스로 요소 선택하기
  
  이벤트 리스너에 의해 전달된 이벤트 객체 e의 classList의 0번째 인덱스는 className이다. 아래 코드에서는 classList[0]을 통해 className 'trash-btn'을 
  ```function deleteCheck(e) {
    const item = e.target;
    //DELETE TODO
    if(item.classList[0] === 'trash-btn') {
        const todo = item.parentElement;
        //Animation
        todo.classList.add('fall');
        removeLocalTodos(todo);
        todo.addEventListener('transitionend', function() {
            todo.remove();
        })
    }
  ```
