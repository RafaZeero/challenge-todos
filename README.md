# 💻 Sobre o desafio

Essa será uma aplicação para gerenciar tarefas (em inglês _todos_). Será permitida a criação de um usuário com `name` e `username`, bem como fazer o CRUD de *todos*:

- Criar um novo _todo_;
- Listar todos os _todos_;
- Alterar o `title` e `deadline` de um _todo_ existente;
- Marcar um _todo_ como feito;
- Excluir um _todo_;

Tudo isso para cada usuário em específico (o `username` será passado pelo header). A seguir veremos com mais detalhes o que e como precisa ser feito 🚀

## Rotas da aplicação

Com o template já clonado e o arquivo `index.js` aberto, você deve completar onde não possui código com o código para atingir os objetivos de cada teste.

### POST `/users`

A rota deve receber `name`, e `username` dentro do corpo da requisição. Ao cadastrar um novo usuário, ele deve ser armazenado dentro de um objeto no seguinte formato:

```javascript
{
	id: 'uuid',
	name: 'Rafael Lima',
	username: 'rafazeero',
	todos: []
}
```

O objeto do usuário deve ser retornado na resposta da requisição.

### GET `/todos`

A rota deve receber, pelo header da requisição, uma propriedade `username` contendo o username do usuário e retornar uma lista com todas as tarefas desse usuário.

### POST `/todos`

A rota deve receber `title` e `deadline` dentro do corpo da requisição e, uma propriedade `username` contendo o username do usuário dentro do header da requisição. Ao criar um novo _todo_, ele deve ser armazenada dentro da lista `todos` do usuário que está criando essa tarefa. Cada tarefa deverá estar no seguinte formato:

```javascript
{
	id: 'uuid',
	title: 'Completar challenge todos rocketseat',
	done: false,
	deadline: '2022-07-20T00:00:00.000Z',
	created_at: '2022-07-10T00:00:00.000Z'
}
```

Certifique-se que o ID seja um UUID.

**Observação**: Lembre-se de iniciar a propriedade `done` sempre como `false` ao criar um _todo_.

**Dica**: Ao fazer a requisição com o Insomnia ou Postman, preencha a data de `deadline` com o formato `ANO-MÊS-DIA` e ao salvar a tarefa pela rota, faça da seguinte forma:

```javascript
{
	id: 'uuid',
	title: 'Completar challenge todos rocketseat',
	done: false,
	deadline: new Date(deadline),
	created_at: new Date()
}
```

Usar _new Date(deadline)_ irá realizar a transformação da string `ANO-MÊS-DIA` (por exemplo "2021-02-25") para uma data válida do JavaScript.
O objeto do todo deve ser retornado na resposta da requisição.

### PUT `/todos/:id`

A rota deve receber, pelo header da requisição, uma propriedade `username` contendo o username do usuário e receber as propriedades `title` e `deadline` dentro do corpo. É preciso alterar **apenas** o `title` e o `deadline` da tarefa que possua o `id` igual ao `id` presente nos parâmetros da rota.

### PATCH `/todos/:id/done`

A rota deve receber, pelo header da requisição, uma propriedade `username` contendo o username do usuário e alterar a propriedade `done` para `true` no _todo_ que possuir um `id` igual ao `id` presente nos parâmetros da rota.

### DELETE `/todos/:id`

A rota deve receber, pelo header da requisição, uma propriedade `username` contendo o username do usuário e excluir o _todo_ que possuir um `id` igual ao `id` presente nos parâmetros da rota.

## Específicação dos testes

Em cada teste, tem uma breve descrição no que sua aplicação deve cumprir para que o teste passe.

Para esse desafio, temos os seguintes testes:

### Testes de usuários

- **Should be able to create a new user**

Para que esse teste passe, você deve permitir que um usuário seja criado e retorne um JSON com o usuário criado. Você pode ver o formato de um usuário [aqui](https://www.notion.so/Desafio-01-Conceitos-do-Node-js-59ccb235aecd43a6a06bf09a24e7ede8).

Também é necessário que você retorne a resposta com o código `201`.

- **Should not be able to create a new user when username already exists**

Para que esse teste passe, antes de criar um usuário você deve validar se outro usuário com o mesmo `username` já existe. Caso exista, retorne uma resposta com status `400` e um json no seguinte formato:

```javascript
{
  error: "Mensagem do erro";
}
```

A mensagem pode ser de sua escolha, desde que a propriedade seja **error**.

### Testes de _todos_

**Middleware**

Para completar todos os testes referentes à _todos_ é necessário antes ter completado o código que falta no middleware `checkExistsUserAccount`. Para isso, você deve pegar o `username` do usuário no header da requisição, verificar se esse usuário existe e então colocar esse usuário dentro da `request` antes de chamar a função `next`. Caso o usuário não seja encontrado, você deve retornar uma resposta contendo status `404` e um json no seguinte formato:

```javascript
{
  error: "Mensagem do erro";
}
```

**Observação**: O username deve ser enviado pelo header em uma propriedade chamada username:

![](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F5c538c34-498a-4789-9bb4-0f286d9b2cf2%2FUntitled.png?table=block&id=1e560d27-9a65-407c-81a6-5b1c1abe0d4f&spaceId=08f749ff-d06d-49a8-a488-9846e081b224&width=1110&userId=8fd0f9ce-7588-4a70-8a21-7c2cd0d9782c&cache=v2)

- **Should be able to list all user's todos**

Para que esse teste passe, na rota GET `/todos` é necessário pegar o usuário que foi repassado para o `request` no middleware `checkExistsUserAccount` e então retornar a lista `todos` que está no objeto do usuário conforme foi criado para satisfazer o [primeiro teste](https://www.notion.so/Desafio-01-Conceitos-do-Node-js-59ccb235aecd43a6a06bf09a24e7ede8).

- **Should be able to create a new todo**

Para que esse teste passe, na rota POST `/todos` é necessário pegar o usuário que foi repassado para o `request` no middleware `checkExistsUserAccount`, pegar também o `title` e o `deadline` do corpo da requisição e adicionar um novo _todo_ na lista `todos` que está no objeto do usuário. Lembre-se de seguir a estrutura padrão de um _todo_ como mostrado [aqui](https://www.notion.so/Desafio-01-Conceitos-do-Node-js-59ccb235aecd43a6a06bf09a24e7ede8).

Após adicionar o novo _todo_ na lista, é necessário retornar um status `201` e o _todo_ no corpo da resposta.

- **Should be able to update a todo**

Para que esse teste passe, na rota PUT `/todos/:id` é necessário atualizar um _todo_ existente, recebendo o `title` e o `deadline` pelo corpo da requisição e o `id` presente nos parâmetros da rota.

- **Should not be able to update a non existing todo**

Para que esse teste passe, você não deve permitir a atualização de um _todo_ que não existe e retornar uma resposta contendo um status `404` e um json no seguinte formato:

```javascript
{
  error: "Mensagem do erro";
}
```

- **Should be able to mark a todo as done**

Para que esse teste passe, na rota PATCH `/todos/:id/done` você deve mudar a propriedade `done`de um _todo_ de `false` para `true`, recebendo o `id` presente nos parâmetros da rota.

- **Should not be able to mark a non existing todo as done**

Para que esse teste passe, você não deve permitir a mudança da propriedade `done` de um _todo_ que não existe e retornar uma resposta contendo um status `404` e um json no seguinte formato:

```javascript
{
  error: "Mensagem do erro";
}
```

- **Should be able to delete a todo**

Para que esse teste passe, DELETE `/todos/:id` você deve permitir que um _todo_ seja excluído usando o `id` passado na rota. O retorno deve ser apenas um status `204` que representa uma resposta sem conteúdo.

- **Should not be able to delete a non existing todo**

Para que esse teste passe, você não deve permitir excluir um _todo_ que não exista e retornar uma resposta contendo um status `404` e um json no seguinte formato:

```javascript
{
  error: "Mensagem do erro";
}
```

### Checklist

- [ ] **Rotas da aplicação**

  - [x] POST `/users`
  - [x] GET `/todos`
  - [x] POST `/todos`
  - [x] PUT `/todos/:id`
  - [x] PATCH `/todos/:id/done`
  - [ ] DELETE `/todos/:id`

- [x] **Testes de Usuários**

  - [x] Should be able to create a new user
  - [x] Should not be able to create a new user when username already exists

- [ ] **Testes de _todos_**
  - [x] Middleware
  - [x] Should be able to list all user's todos
  - [x] Should be able to create a new todo
  - [x] Should be able to update a todo
  - [x] Should not be able to update a non existing todo
  - [x] Should be able to mark a todo as done
  - [ ] Should be able to delete a todo
  - [ ] Should not be able to delete a non existing todo
