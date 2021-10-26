# toy_todo-list
해당 토이 프로젝트는 유튜브 채널 <Dev Ed> 의 Beginner Vanilla Javascript Project Tutorial를 보고 따라한 것입니다. 현재는 JS로만 구성되어 있으나 코드 리팩토링을 통해 Reat로 리팩토링할 예정입니다.
해당 토이를 유튜브에서 보고 싶다면 https://www.youtube.com/watch?v=Ttf3CEsEwMQ

![ezgif com-gif-maker](https://user-images.githubusercontent.com/87353284/138810042-07e73073-467c-4cb8-b443-6ca3a4731d77.gif)

## function developed
  1. todo 항목 추가, 삭제
  2. todo check 기능(text-decoration: line-through, fall down)
  3. filter - All, completed, uncompleted
  4. 크롬 개발자 도구 Localstorgae 활용한 todo list 저장, 삭제

## highlight codes
  
### 1. JS
  
  #### (1) ele.classList.toggle
  
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
  
  #### (2) 조건문 switch case
  
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
  #### (3) classList에 있는 인덱스로 요소 선택하기
  
  이벤트 리스너에 의해 전달된 이벤트 객체 e의 classList의 0번째 인덱스는 className이다. 아래 코드에서는 classList[0]을 통해 className 'trash-btn'을 불러왔다.
  
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
  
  #### (4) LocalStorage 활용
  
  크롬 개발자 도구의 LocalStorage를 활용하여 todoList의 데이터를 저장하고 HTML DOM이 초기화 되었을 때 저장된 localStorage의 value를 렌더링해주었다.
  
  ```function saveLocalTodos(todo) {
    //todo가 이미 list에 존재하는가?
    let todos;
    if(localStorage.getItem("todos") === null) {
        todos = [];
    } else {
        todos = JSON.parse(localStorage.getItem("todos"))
    }
    todos.push(todo);
    localStorage.setItem("todos", JSON.stringify(todos));
  ```
  
  #### (5) 이벤트 리스너 DOMContentLoaded
  
  HTML 문서의 생명 주기는 3가지 이벤트가 관여한다. DOMContentLoaded, load, beforeunload/unload.
  그중 DOMContentLoaded는  브라우저가 HTML을 모두 읽고 DOM 트리를 완성하는 즉시 발생한다.
  아래의 코드는 HTML 문서 초기에 getTodos를 읽어와서 Local Storage에 저장된 todo를 화면에 뿌려준다.
  
  ```//Event Listeners
     document.addEventListener('DOMContentLoaded', getTodos)
  ```
  
### 2. CSS
  
  #### (1) background-image: linear-gradient
  gradient 기능을 처음 접해 보았다. linear- gradient는 직선으로 진행하는 색상무늬이고 degree 속성으로 직선의 기울어짐 정도를 표현할 수 있다.
  
  ```
  background-image: linear-gradient(120deg, #f6d365, #b18d83);
  color: #fff;
  font-family: 'Nanum Gothic', sans-serif;
  min-height: 100vh;
  ```
  
  #### (2) transition
  CSS에서 transition은 애니메이션의 속도를 조절하는 방법을 제공한다. 아래의 코드의 all 은 모든 애니메이션 요소, 0.3s 애니메이션 작동 지속 시간ease 는 타이밍 펑션으로 전환 효과의 시간당 속도를 설정한다.
  ease는 기본값으로 효과가 천천히 시작되어 그 다음에는 빨라지고 마지막에 다시 느려진다.
  
  ```
  color: #d88771;
  background:#fff;
  cursor: pointer;
  transition: all 0.3s ease; }
  ```
  
  ### (3) transform
  CSS transfor을 사용하면 요소의 회전, 크기조절, 기울기, 이동 효과를 부여할 수 있다. translateY는 브라우저의 Y축을 기준 8rem만큼 요소를 이동한다는 의미다. 
  rotateZCSS좌표체계가 궁금하다면 https://jobcoding.tistory.com/130. rotateZ는 회전속과를 주는데 degree가 양수면 요소가 시계방향으로 음수는 반시계방향으로 회전하게 된다.
  
  ```
  transform: translateY(8rem) rotateZ(20deg);
  opacity: 0.5;
  ```
  
  ## (4) after:, hoover:after
  요소의 작동 순서를 지정해 주었다. CSS가 진행될 때 순차적으로 다른 이벤트를 진행할 수 있게 하였다.
  ```
  select::after {
    content: "\25BC";
    position: absolute;
    background: #ff6f47;
    top: 0;
    right: 0;
    padding: 1rem;
    pointer-events: none;

  select:hover::after {
    background: white;
    color: #ff6f47;
  ```
  
