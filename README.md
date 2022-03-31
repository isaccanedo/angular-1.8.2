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

# Criar componentes

### Diretivas
As diretivas são um recurso exclusivo e poderoso disponível no AngularJS. As diretivas permitem que você invente uma nova sintaxe HTML, específica para seu aplicativo.

### Componentes reutilizáveis
Usamos diretivas para criar componentes reutilizáveis. Um componente permite ocultar estrutura, CSS e comportamento complexo do DOM. Isso permite que você se concentre no que o aplicativo faz ou na aparência do aplicativo separadamente.

### Localização
Uma parte importante dos aplicativos sérios é a localização. Os filtros com reconhecimento de localidade do AngularJS e as diretivas de lematização fornecem blocos de construção para tornar seu aplicativo disponível em todas as localidades.

**index.html**
```
<!doctype html>
<html ng-app="app">
  <head>
    <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.8.2/angular.min.js"></script>
    <script src="components.js"></script>
    <script src="app.js"></script>
  </head>
  <body>
    <tabs>
      <pane title="Localization">
        <span>Date: {{ '2012-04-01' | date:'fullDate' }}</span><br>
        <span>Currency: {{ 123456 | currency }}</span><br>
        <span>Number: {{ 98765.4321 | number }}</span><br>
      </pane>
      <pane title="Pluralization">
        <div ng-controller="BeerCounter">
          <div ng-repeat="beerCount in beers">
            <ng-pluralize count="beerCount" when="beerForms"></ng-pluralize>
          </div>
        </div>
      </pane>
    </tabs>
  </body>
</html>
```

**components.js**
```
angular.module('components', [])
 
  .directive('tabs', function() {
    return {
      restrict: 'E',
      transclude: true,
      scope: {},
      controller: function($scope, $element) {
        var panes = $scope.panes = [];
 
        $scope.select = function(pane) {
          angular.forEach(panes, function(pane) {
            pane.selected = false;
          });
          pane.selected = true;
        }
 
        this.addPane = function(pane) {
          if (panes.length == 0) $scope.select(pane);
          panes.push(pane);
        }
      },
      template:
        '<div class="tabbable">' +
          '<ul class="nav nav-tabs">' +
            '<li ng-repeat="pane in panes" ng-class="{active:pane.selected}">'+
              '<a href="" ng-click="select(pane)">{{pane.title}}</a>' +
            '</li>' +
          '</ul>' +
          '<div class="tab-content" ng-transclude></div>' +
        '</div>',
      replace: true
    };
  })
 
  .directive('pane', function() {
    return {
      require: '^tabs',
      restrict: 'E',
      transclude: true,
      scope: { title: '@' },
      link: function(scope, element, attrs, tabsController) {
        tabsController.addPane(scope);
      },
      template:
        '<div class="tab-pane" ng-class="{active: selected}" ng-transclude>' +
        '</div>',
      replace: true
    };
  })
```

**app.js**
```
angular.module('app', ['components'])
 
.controller('BeerCounter', function($scope, $locale) {
  $scope.beers = [0, 1, 2, 3, 4, 5, 6];
  if ($locale.id == 'en-us') {
    $scope.beerForms = {
      0: 'no beers',
      one: '{} beer',
      other: '{} beers'
    };
  } else {
    $scope.beerForms = {
      0: 'žiadne pivo',
      one: '{} pivo',
      few: '{} pivá',
      other: '{} pív'
    };
  }
});
```

# Navegação, formulários e back-ends
### Deep Linking
Um link direto reflete onde o usuário está no aplicativo. Isso é útil para que os usuários possam marcar e enviar links por e-mail para locais no aplicativo. Os aplicativos de ida e volta obtêm isso automaticamente, mas os aplicativos AJAX, por sua natureza, não. O AngularJS combina os benefícios do deep linking com o comportamento semelhante ao de um aplicativo de desktop.

### Validação de formulário
A validação de formulário do lado do cliente é uma parte importante de uma ótima experiência do usuário. AngularJS permite declarar as regras de validação do formulário sem ter que escrever código JavaScript. Escreva menos código, vá tomar cerveja mais cedo.

### Comunicação do servidor
O AngularJS fornece serviços integrados no XHR, bem como vários outros back-ends usando bibliotecas de terceiros. As promessas simplificam ainda mais seu código ao lidar com o retorno assíncrono de dados.

# Testabilidade Integrada
### Injetável
A injeção de dependência no AngularJS permite que você descreva declarativamente como seu aplicativo está conectado. Isso significa que seu aplicativo não precisa do método main(), que geralmente é uma bagunça insustentável. A injeção de dependência também é um núcleo do AngularJS. Isso significa que qualquer componente que não atenda às suas necessidades pode ser facilmente substituído.

### Testável
AngularJS foi projetado desde o início para ser testável. Ele incentiva a separação de visão de comportamento, vem pré-empacotado com mocks e aproveita ao máximo a injeção de dependência. Ele também vem com um executor de cenários de ponta a ponta que elimina falhas de teste ao entender o funcionamento interno do AngularJS.
