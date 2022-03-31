# angular-1.8.2

### O Básico

```
<!doctype html>
<html ng-app>
  <head>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.8.2/angular.min.js"></script>
  </head>
  <body>
    <div>
      <label>Name:</label>
      <input type="text" ng-model="yourName" placeholder="Enter a name here">
      <hr>
      <h1>Hello {{yourName}}!</h1>
    </div>
  </body>
</html>
```

### 1. Adicionar algum controle
Ligação de dados
A vinculação de dados é uma maneira automática de atualizar a visão sempre que o modelo muda, bem como atualizar o modelo sempre que a visão muda. Isso é incrível porque elimina a manipulação do DOM da lista de coisas com as quais você precisa se preocupar.

### 2. Controlador
Os controladores são o comportamento por trás dos elementos DOM. O AngularJS permite que você expresse o comportamento em um formato legível sem o clichê usual de atualizar o DOM, registrar retornos de chamada ou observar mudanças no modelo.

### 3. JavaScript simples
Ao contrário de outros frameworks, não há necessidade de herdar de tipos proprietários para envolver o modelo em métodos acessadores. Os modelos AngularJS são objetos JavaScript simples e antigos. Isso torna seu código fácil de testar, manter, reutilizar e, novamente, livre de clichês.


**index.html**
```
<!doctype html>
<html ng-app="todoApp">
  <head>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.8.2/angular.min.js"></script>
    <script src="todo.js"></script>
    <link rel="stylesheet" href="todo.css">
  </head>
  <body>
    <h2>Todo</h2>
    <div ng-controller="TodoListController as todoList">
      <span>{{todoList.remaining()}} of {{todoList.todos.length}} remaining</span>
      [ <a href="" ng-click="todoList.archive()">archive</a> ]
      <ul class="unstyled">
        <li ng-repeat="todo in todoList.todos">
          <label class="checkbox">
            <input type="checkbox" ng-model="todo.done">
            <span class="done-{{todo.done}}">{{todo.text}}</span>
          </label>
        </li>
      </ul>
      <form ng-submit="todoList.addTodo()">
        <input type="text" ng-model="todoList.todoText"  size="30"
               placeholder="add new todo here">
        <input class="btn-primary" type="submit" value="add">
      </form>
    </div>
  </body>
</html>
```
**todo.js**
```
angular.module('todoApp', [])
  .controller('TodoListController', function() {
    var todoList = this;
    todoList.todos = [
      {text:'learn AngularJS', done:true},
      {text:'build an AngularJS app', done:false}];
 
    todoList.addTodo = function() {
      todoList.todos.push({text:todoList.todoText, done:false});
      todoList.todoText = '';
    };
 
    todoList.remaining = function() {
      var count = 0;
      angular.forEach(todoList.todos, function(todo) {
        count += todo.done ? 0 : 1;
      });
      return count;
    };
 
    todoList.archive = function() {
      var oldTodos = todoList.todos;
      todoList.todos = [];
      angular.forEach(oldTodos, function(todo) {
        if (!todo.done) todoList.todos.push(todo);
      });
    };
  });
```

**todo.css**
```
.done-true {
  text-decoration: line-through;
  color: grey;
}
```
