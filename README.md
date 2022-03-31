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

### Adicionar algum controle
Ligação de dados
A vinculação de dados é uma maneira automática de atualizar a visão sempre que o modelo muda, bem como atualizar o modelo sempre que a visão muda. Isso é incrível porque elimina a manipulação do DOM da lista de coisas com as quais você precisa se preocupar.

### Controlador
Os controladores são o comportamento por trás dos elementos DOM. O AngularJS permite que você expresse o comportamento em um formato legível sem o clichê usual de atualizar o DOM, registrar retornos de chamada ou observar mudanças no modelo.

### JavaScript simples
Ao contrário de outros frameworks, não há necessidade de herdar de tipos proprietários para envolver o modelo em métodos acessadores. Os modelos AngularJS são objetos JavaScript simples e antigos. Isso torna seu código fácil de testar, manter, reutilizar e, novamente, livre de clichês.
