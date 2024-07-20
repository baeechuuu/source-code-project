# source-code-project
projects

#todo list project using html,css, and js
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

#todo list porject using php
#add.php
<?php

if(isset($_POST['title'])){
    require '../db_conn.php';

    $title = $_POST['title'];

    if(empty($title)){
        header("Location: ../index.php?mess=error");
    }else {
        $stmt = $conn->prepare("INSERT INTO todos(title) VALUE(?)");
        $res = $stmt->execute([$title]);

        if($res){
            header("Location: ../index.php?mess=success"); 
        }else {
            header("Location: ../index.php");
        }
        $conn = null;
        exit();
    }
}else {
    header("Location: ../index.php?mess=error");
}

#remove.php
<?php

if(isset($_POST['id'])){
    require '../db_conn.php';

    $id = $_POST['id'];

    if(empty($id)){
       echo 0;
    }else {
        $stmt = $conn->prepare("DELETE FROM todos WHERE id=?");
        $res = $stmt->execute([$id]);

        if($res){
            echo 1;
        }else {
            echo 0;
        }
        $conn = null;
        exit();
    }
}else {
    header("Location: ../index.php?mess=error");
}

#check.php
<?php

if(isset($_POST['id'])){
    require '../db_conn.php';

    $id = $_POST['id'];

    if(empty($id)){
       echo 'error';
    }else {
        $todos = $conn->prepare("SELECT id, checked FROM todos WHERE id=?");
        $todos->execute([$id]);

        $todo = $todos->fetch();
        $uId = $todo['id'];
        $checked = $todo['checked'];

        $uChecked = $checked ? 0 : 1;

        $res = $conn->query("UPDATE todos SET checked=$uChecked WHERE id=$uId");

        if($res){
            echo $checked;
        }else {
            echo "error";
        }
        $conn = null;
        exit();
    }
}else {
    header("Location: ../index.php?mess=error");
}

#index.php
<?php 
require 'db_conn.php';
?>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>To-Do List</title>
    <link rel="stylesheet" href="css/style.css">
</head>
<body>
    <div class="main-section">
       <div class="add-section">
          <form action="app/add.php" method="POST" autocomplete="off">
             <?php if(isset($_GET['mess']) && $_GET['mess'] == 'error'){ ?>
                <input type="text" 
                     name="title" 
                     style="border-color: #ff6666"
                     placeholder="This field is required" />
              <button type="submit">Add &nbsp; <span>&#43;</span></button>

             <?php }else{ ?>
              <input type="text" 
                     name="title" 
                     placeholder="What do you need to do?" />
              <button type="submit">Add &nbsp; <span>&#43;</span></button>
             <?php } ?>
          </form>
       </div>
       <?php 
          $todos = $conn->query("SELECT * FROM todos ORDER BY id DESC");
       ?>
       <div class="show-todo-section">
            <?php if($todos->rowCount() <= 0){ ?>
                <div class="todo-item">
                    <div class="empty">
                        <img src="img/f.png" width="100%" />
                        <img src="img/Ellipsis.gif" width="80px">
                    </div>
                </div>
            <?php } ?>

            <?php while($todo = $todos->fetch(PDO::FETCH_ASSOC)) { ?>
                <div class="todo-item">
                    <span id="<?php echo $todo['id']; ?>"
                          class="remove-to-do">x</span>
                    <?php if($todo['checked']){ ?> 
                        <input type="checkbox"
                               class="check-box"
                               data-todo-id ="<?php echo $todo['id']; ?>"
                               checked />
                        <h2 class="checked"><?php echo $todo['title'] ?></h2>
                    <?php }else { ?>
                        <input type="checkbox"
                               data-todo-id ="<?php echo $todo['id']; ?>"
                               class="check-box" />
                        <h2><?php echo $todo['title'] ?></h2>
                    <?php } ?>
                    <br>
                    <small>created: <?php echo $todo['date_time'] ?></small> 
                </div>
            <?php } ?>
       </div>
    </div>

    <script src="js/jquery-3.2.1.min.js"></script>

    <script>
        $(document).ready(function(){
            $('.remove-to-do').click(function(){
                const id = $(this).attr('id');
                
                $.post("app/remove.php", 
                      {
                          id: id
                      },
                      (data)  => {
                         if(data){
                             $(this).parent().hide(600);
                         }
                      }
                );
            });

            $(".check-box").click(function(e){
                const id = $(this).attr('data-todo-id');
                
                $.post('app/check.php', 
                      {
                          id: id
                      },
                      (data) => {
                          if(data != 'error'){
                              const h2 = $(this).next();
                              if(data === '1'){
                                  h2.removeClass('checked');
                              }else {
                                  h2.addClass('checked');
                              }
                          }
                      }
                );
            });
        });
    </script>
</body>
</html>

#DBconn.php
<?php 

$sName = "localhost";
$uName = "root";
$pass = "";
$db_name = "to_do_list";

try {
    $conn = new PDO("mysql:host=$sName;dbname=$db_name", 
                    $uName, $pass);
    $conn->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
}catch(PDOException $e){
  echo "Connection failed : ". $e->getMessage();
}

#style.css
body {
    background: #34495e;
  }
  
  * {
    padding: 0px;
    margin: 0px;
    box-sizing: border-box;
  }
  
  .main-section {
    background: transparent;
    max-width: 500px;
    width: 90%;
    height: 500px;
    margin: 30px auto;
    border-radius: 10px;
  }
  
  .add-section {
    width: 100%;
    background: #fff;
    margin: 0px auto;
    padding: 10px;
    border-radius: 5px;
  }
  
  .add-section input {
    display: block;
    width: 95%;
    height: 40px;
    margin: 10px auto;
    border: 2px solid #ccc;
    font-size: 16px;
    border-radius: 5px;
    padding: 0px 5px;
  }
  
  .add-section button {
    display: block;
    width: 95%;
    height: 40px;
    margin: 0px auto;
    border: none;
    outline: none;
    background: #0088FF;
    color: #fff;
    font-family: sans-serif;
    font-size: 16px;
    border-radius: 5px;
    cursor: pointer;
  }
  
  .add-section button:hover {
    box-shadow: 0 2px 2px 0 #ccc, 0 2px 3px 0 #ccc;
    opacity: 0.7;
  }
  
  .add-section button span {
    border: 1px solid #fff;
    border-radius: 50%;
    display: inline-block;
    width: 18px;
    height: 18px;
  }
  
  #errorMes {
    display: block;
    background: #f2dede;
    width: 95%;
    margin: 0px auto;
    color: rgb(139, 19, 19);
    padding: 10px;
    height: 35px;
  }
  
  .show-todo-section {
    width: 100%;
    background: #fff;
    margin: 30px auto;
    padding: 10px;
    border-radius: 5px;
  }
  
  .todo-item {
    width: 95%;
    margin: 10px auto;
    padding: 20px 10px;
    box-shadow: 0 4px 8px 0 #ccc, 0 6px 20px 0 #ccc;
    border-radius: 5px;
  }
  
  .todo-item h2 {
    display: inline-block;
    padding: 5px 0px;
    font-size: 17px;
    font-family: sans-serif;
    color: #555;
  }
  
  .todo-item small {
    display: block;
    width: 100%;
    padding: 5px 0px;
    color: #888;
    padding-left: 30px;
    font-size: 14px;
    font-family: sans-serif;
  }
  
  .remove-to-do {
    display: block;
    float: right;
    width: 20px;
    height: 20px;
    font-family: sans-serif;
    color: rgb(139, 97, 93);
    text-decoration: none;
    text-align: right;
    padding: 0px 5px 8px 0px;
    border-radius: 50%;
    transition: background 1s;
    cursor: pointer;
  }
  
  .remove-to-do:hover {
    background: rgb(139, 97, 93);
    color: #fff;
  }
  
  .checked {
    color: #999 !important;
    text-decoration: line-through;
  }
  
  .todo-item input {
    margin: 0px 5px;
  }
  
  .empty {
    font-family: sans-serif;
    font-size: 16px;
    text-align: center;
    color: #cccc;
  }

  #DB_todo.sql
  

SET SQL_MODE = "NO_AUTO_VALUE_ON_ZERO";
SET AUTOCOMMIT = 0;
START TRANSACTION;
SET time_zone = "+00:00";


/*!40101 SET @OLD_CHARACTER_SET_CLIENT=@@CHARACTER_SET_CLIENT */;
/*!40101 SET @OLD_CHARACTER_SET_RESULTS=@@CHARACTER_SET_RESULTS */;
/*!40101 SET @OLD_COLLATION_CONNECTION=@@COLLATION_CONNECTION */;
/*!40101 SET NAMES utf8mb4 */;

--
-- Database: `to_do_list`
--

-- --------------------------------------------------------

--
-- Table structure for table `todos`
--

CREATE TABLE `todos` (
  `id` int(11) NOT NULL,
  `title` text NOT NULL,
  `date_time` datetime NOT NULL DEFAULT current_timestamp(),
  `checked` tinyint(1) NOT NULL DEFAULT 0
) ENGINE=InnoDB DEFAULT CHARSET=latin1;

--
-- Indexes for dumped tables
--

--
-- Indexes for table `todos`
--
ALTER TABLE `todos`
  ADD PRIMARY KEY (`id`);

--
-- AUTO_INCREMENT for dumped tables
--

--
-- AUTO_INCREMENT for table `todos`
--
ALTER TABLE `todos`
  MODIFY `id` int(11) NOT NULL AUTO_INCREMENT, AUTO_INCREMENT=30;
COMMIT;

/*!40101 SET CHARACTER_SET_CLIENT=@OLD_CHARACTER_SET_CLIENT */;
/*!40101 SET CHARACTER_SET_RESULTS=@OLD_CHARACTER_SET_RESULTS */;
/*!40101 SET COLLATION_CONNECTION=@OLD_COLLATION_CONNECTION */;
