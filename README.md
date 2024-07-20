# source-code-project
projects

#todo list project
#html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>TODO LIST</title>
  <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/font-awesome/4.5.0/css/font-awesome.min.css">
  <link rel="stylesheet" href="./style.css">
</head>
<body>
  <div class="container">
    <h1>Todo List</h1>
    <div class="input-container">
      <input class="todo-input" placeholder="Add a new task...">
      <button class="add-button">
        <i class="fa fa-plus-circle"></i>
      </button>
    </div>
    <div class="filters">
      <div class="filter" data-filter="completed">Complete</div>
      <div class="filter" data-filter="pending">Incomplete</div>
      <div class="delete-all">Delete All</div>
    </div>
    <div class="todos-container">
      <ul class="todos"></ul>
      <img class="empty-image" src="./empty.svg">
    </div>
  </div>
  <script src="./script.js"></script>
</body>
</html>

#css
*,
*::before,
*::after {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Roboto', sans-serif;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  background: url(./background.jpg) no-repeat;
  background-position: center;
  background-size: cover;
}

.container {
  width: 400px;
  height: auto;
  min-height: 400px;
  padding: 30px;
  background: transparent;
  border: 2px solid #e6b7eca1;
  border-radius: 10px;
  backdrop-filter: blur(15px);
}

h1 {
  color: #eee;
  text-align: center;
  margin-bottom: 36px;
}

.input-container {
  display: flex;
  justify-content: space-between;
  margin-bottom: 25px;
}

.todo-input {
  flex: 1;
  outline: none;
  padding: 10px 10px 10px 20px;
  background-color: transparent;
  border: 2px solid #e6b7eca1;
  border-radius: 30px;
  color: #eee;
  font-size: 16px;
  margin-right: 10px;
}

.todo-input::placeholder {
  color: #bfbfbf;
}

.add-button {
  border: none;
  outline: none;
  background: #e6b7eca1;
  color: #fff;
  font-size: 35px;
  cursor: pointer;
  border-radius: 40px;
  width: 40px;
  height: 40px;
}

.empty-image {
  margin: 55px auto 0;
  display: block;
}

.todo {
  list-style: none;
  display: flex;
  align-items: center;
  justify-content: space-between;
  background-color: #463c7b;
  border-radius: 5px;
  margin: 10px 0;
  padding: 10px 12px;
  border: 2px solid #e6b7eca1;
  transition: all 0.2s ease;
}

.todo:first-child {
  margin-top: 0;
}

.todo:last-child {
  margin-bottom: 0;
}

.todo:hover {
  background-color:#e6b7eca1;
}

.todo label {
  cursor: pointer;
  width: fit-content;
  display: flex;
  align-items: center;
  font-family: 'Roboto', sans-serif;
  color: #eee;
}

.todo span {
  padding-left: 20px;
  position: relative;
  cursor: pointer;
}

span::before {
  content: "";
  height: 20px;
  width: 20px;
  position: absolute;
  margin-left: -30px;
  border-radius: 100px;
  border: 2px solid #e6b7eca1;
}

input[type='checkbox'] {
  visibility: hidden;
}

input:checked + span {
  text-decoration: line-through
}

.todo:hover input:checked + span::before, input:checked + span::before {
  background: url(./check.svg) 50% 50% no-repeat #09bb21;
  border-color: #09bb21;
}

.todo:hover span::before {
  border-color: #eee;
}

.todo .delete-btn  {
  background-color: transparent;
  border: none;
  cursor: pointer;
  color: #eee;
  font-size: 24px;
}

.todos-container  {
  height: 300px;
  overflow: overlay;
}

.todos-container::-webkit-scrollbar-track  {
  background: rgb(247, 247, 247);
  border-radius: 20px
}

.todos-container::-webkit-scrollbar  {
  width: 0;
}

.todos-container:hover::-webkit-scrollbar  {
  width: 7px;
}

.todos-container::-webkit-scrollbar-thumb  {
  background: #d5d5d5;
  border-radius: 20px;
}

.filters {
  display: flex;
  justify-content: space-between;
  margin-bottom: 25px;
}

.filter {
  color: #eee;
  padding: 5px 15px;
  border-radius: 100px;
  border: 2px solid #e6b7eca1;
  transition: all 0.2s ease;
  cursor: pointer;
}

.filter.active, .filter:hover {
  background-color: #e6b7eca1;
}

.delete-all {
  display: flex;
  color: #eee;
  padding: 7px 15px;
  cursor: pointer;
  transition: all 0.2s ease;
}

.delete-all:hover {
  border-radius: 5px;
  background-color: #e6b7eca1;
}

#javascrpit
const input = document.querySelector("input");
const addButton = document.querySelector(".add-button");
const todosHtml = document.querySelector(".todos");
const emptyImage = document.querySelector(".empty-image");
let todosJson = JSON.parse(localStorage.getItem("todos")) || [];
const deleteAllButton = document.querySelector(".delete-all");
const filters = document.querySelectorAll(".filter");
let filter = '';

showTodos();

function getTodoHtml(todo, index) {
  if (filter && filter != todo.status) {
    return '';
  }
  let checked = todo.status == "completed" ? "checked" : "";
  return /* html */ `
    <li class="todo">
      <label for="${index}">
        <input id="${index}" onclick="updateStatus(this)" type="checkbox" ${checked}>
        <span class="${checked}">${todo.name}</span>
      </label>
      <button class="delete-btn" data-index="${index}" onclick="remove(this)"><i class="fa fa-times"></i></button>
    </li>
  `; 
}

function showTodos() {
  if (todosJson.length == 0) {
    todosHtml.innerHTML = '';
    emptyImage.style.display = 'block';
  } else {
    todosHtml.innerHTML = todosJson.map(getTodoHtml).join('');
    emptyImage.style.display = 'none';
  }
}

function addTodo(todo)  {
  input.value = "";
  todosJson.unshift({ name: todo, status: "pending" });
  localStorage.setItem("todos", JSON.stringify(todosJson));
  showTodos();
}

input.addEventListener("keyup", e => {
  let todo = input.value.trim();
  if (!todo || e.key != "Enter") {
    return;
  }
  addTodo(todo);
});

addButton.addEventListener("click", () => {
  let todo = input.value.trim();
  if (!todo) {
    return;
  }
  addTodo(todo);
});

function updateStatus(todo) {
  let todoName = todo.parentElement.lastElementChild;
  if (todo.checked) {
    todoName.classList.add("checked");
    todosJson[todo.id].status = "completed";
  } else {
    todoName.classList.remove("checked");
    todosJson[todo.id].status = "pending";
  }
  localStorage.setItem("todos", JSON.stringify(todosJson));
}

function remove(todo) {
  const index = todo.dataset.index;
  todosJson.splice(index, 1);
  showTodos();
  localStorage.setItem("todos", JSON.stringify(todosJson));
}

filters.forEach(function (el) {
  el.addEventListener("click", (e) => {
    if (el.classList.contains('active')) {
      el.classList.remove('active');
      filter = '';
    } else {
      filters.forEach(tag => tag.classList.remove('active'));
      el.classList.add('active');
      filter = e.target.dataset.filter;
    }
    showTodos();
  });
});

deleteAllButton.addEventListener("click", () => {
  todosJson = [];
  localStorage.setItem("todos", JSON.stringify(todosJson));
  showTodos();
});
