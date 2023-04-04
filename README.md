# The CRUDs
Collection of CRUDS in different languages

Abaixo alguns exemplos simples de CRUD ( create, read, update and delete ) em algumas linguagens para referência de quem está iniciando. Veja que são referências de funcionalidades. Não foi levado em consideração padrões de segurança, anti-injection ou mesmo estruturais funcionais completas.

<div id="p0"></div>

## Índice
- [PHP + MySQL](https://github.com/olavomello/the-cruds?p1)
- [ReacjJS + Nodejs + MongoDB](https://github.com/olavomello/the-cruds?p2)
- [PHP + Laravel + MySQL](https://github.com/olavomello/the-cruds?p3)
- [Rust + MySQL](https://github.com/olavomello/the-cruds?p4)
- [LUA + MySQL](https://github.com/olavomello/the-cruds?p15)
- [Python + MySQL](https://github.com/olavomello/the-cruds?p6)



<div id="p1></div>
CRUD : PHP + MySQL
1. Criar a tabela no banco de dados MySQL:

```sql
CREATE TABLE usuarios (
  id INT(6) UNSIGNED AUTO_INCREMENT PRIMARY KEY,
  nome VARCHAR(50) NOT NULL,
  email VARCHAR(50) NOT NULL,
  senha VARCHAR(50) NOT NULL
);
```

2. Conectar ao banco de dados MySQL:

```php
  $servername = “localhost”;
  $username = “username”;
  $password = “password”;
  $dbname = “myDB”;

  // Cria a conexão
  $conn = new mysqli($servername, $username, $password, $dbname);

  // Verifica se a conexão foi bem sucedida
  if ($conn->connect_error) {
    die(“Falha na conexão: “ . $conn->connect_error);
  }
```

3. Criar um formulário HTML para adicionar novos usuários:
```html
  <form action=”adicionar_usuario.php” method=”post”>
    <label for=”nome”>Nome:</label>
    <input type=”text” name=”nome” id=”nome”><br>

    <label for=”email”>E-mail:</label>
    <input type=”email” name=”email” id=”email”><br>

    <label for=”senha”>Senha:</label>
    <input type=”password” name=”senha” id=”senha”><br>

    <input type=”submit” value=”Adicionar usuário”>
  </form>
```

4. Criar um arquivo PHP para adicionar novos usuários:
```php
  // Recebe os dados do formulário
  $nome = $_POST[‘nome’];
  $email = $_POST[‘email’];
  $senha = $_POST[‘senha’];

  // Insere os dados na tabela
  $sql = “INSERT INTO usuarios (nome, email, senha) VALUES (‘$nome’, ‘$email’, ‘$senha’)”;

  if ($conn->query($sql) === TRUE) {
    echo “Usuário adicionado com sucesso!”;
  } else {
    echo “Erro ao adicionar usuário: “ . $conn->error;
  }
```

5. Criar uma página PHP para exibir todos os usuários:
```php
  // Seleciona todos os usuários da tabela
  $sql = “SELECT * FROM usuarios”;
  $resultado = $conn->query($sql);

  // Exibe os usuários em uma tabela HTML
  if ($resultado->num_rows > 0) {
    echo “<table>”;
    echo “<tr><th>ID</th><th>Nome</th><th>E-mail</th></tr>”;

    while($linha = $resultado->fetch_assoc()) {
      echo “<tr>”;
      echo “<td>” . $linha[“id”] . “</td>”;
      echo “<td>” . $linha[“nome”] . “</td>”;
      echo “<td>” . $linha[“email”] . “</td>”;
      echo “</tr>”;
    }

    echo “</table>”;
  } else {
    echo “Nenhum usuário encontrado.”;
  }
```

6. Criar uma página PHP para editar um usuário:
```php
  // Recebe o ID do usuário a ser editado
  $id = $_GET[‘id’];

  // Seleciona o usuário com o ID recebido
  $sql = “SELECT * FROM usuarios WHERE id = ‘$id’”;
  $resultado = $conn->query($sql);

  if ($resultado->num_rows == 1) {
    // Exibe um formulário com os dados do usuário
    $linha = $resultado->fetch_assoc();
    echo “<form action=’atualizar_usuario.php’ method=’post’>”;
    echo “<input type=’hidden’ name=’id’ value=’” . $linha[“id”] . “‘>”;
    echo “<label>Nome:</label>”;
    echo “<input type=’text’ name=’nome’ value=’” . $linha[“nome”] . “‘><br>”;
    echo “<label>Email:</label>”;
    echo “<input type=’email’ name=’email’ value=’” . $linha[“email”] . “‘><br>”;
    echo “<label>Senha:</label>”;
    echo “<input type=’password’ name=’senha’><br>”;
    echo “<input type=’submit’ value=’Atualizar’>”;
    echo “</form>”;
  } else {
    echo “Usuário não encontrado”;
  }
```

7. Criar um arquivo PHP para atualizar um usuário:
```php
  // Recebe os dados do formulário
  $id = $_POST[‘id’];
  $nome = $_POST[‘nome’];
  $email = $_POST[‘email’];
  $senha = $_POST[‘senha’];

  // Atualiza os dados na tabela
  $sql = “UPDATE usuarios SET nome=’$nome’, email=’$email’, senha=’$senha’ WHERE id=’$id’”;

  if ($conn->query($sql) === TRUE) {
    echo “Usuário atualizado com sucesso!”;
  } else {
    echo “Erro ao atualizar usuário: “ . $conn->error;
  }
```

8. Criar um arquivo PHP para excluir um usuário:
```php
  // Recebe o ID do usuário a ser excluído
  $id = $_GET[‘id’];

  // Exclui o usuário com o ID recebido
  $sql = “DELETE FROM usuarios WHERE id=’$id’”;

  if ($conn->query($sql) === TRUE) {
    echo “Usuário excluído com sucesso!”;
  } else {
    echo “Erro ao excluir usuário: “ . $conn->error;
  }
```

<div id="p2></div>
CRUD : ReacjJS + Nodejs + MongoDB
Para variar um pouco do MySQL, o CRUD á seguir com Reacj, utiliza MongoDB.

O <b>MySQL</b> é uma excelente escolha se você tiver dados estruturados e precisar de um banco de dados relacional tradicional. O <b>MongoDB</b> é adequado para análises em tempo real, gerenciamento de conteúdo, IOT e algumas aplicações para apps. Para um sistema expresso quase sempre o MongoDB é uma ótima escolha. Avalie detalhadamente as necessidades antes de iniciar seu projeto.

1. Criar um projeto Node.js com Express e Mongoose:

Primeiro, crie uma pasta para o seu projeto e abra o terminal dentro dela. Em seguida, execute os seguintes comandos:

```shell
  npm init -y
  npm install express mongoose cors
```

2. Criar um arquivo server.js para configurar o servidor e a conexão com o banco de dados:

Este arquivo configura o servidor Express, habilita o middleware CORS para permitir a comunicação com o front-end e conecta o servidor ao banco de dados MongoDB.

```javascript
  const express = require(“express”);
  const cors = require(“cors”);
  const mongoose = require(“mongoose”);

  const app = express();
  const port = process.env.PORT || 5000;

  app.use(cors());
  app.use(express.json());

  mongoose.connect(“mongodb://localhost:27017/crud”, {
    useNewUrlParser: true,
    useUnifiedTopology: true,
  });

  const connection = mongoose.connection;
  connection.once(“open”, () => {
    console.log(“MongoDB database connection established successfully”);
  });

  app.listen(port, () => {
    console.log(`Server is running on port: ${port}`);
  });
```

3. Criar um modelo de usuário no arquivo models/User.js:

Este modelo de usuário define o esquema para o objeto de usuário no banco de dados MongoDB. Neste exemplo, o usuário terá um nome de usuário único e um registro de data e hora criado automaticamente pelo Mongoose.

```javascript
  const mongoose = require(“mongoose”);

  const Schema = mongoose.Schema;

  const userSchema = new Schema(
    {
      username: {
        type: String,
        required: true,
        unique: true,
        trim: true,
        minlength: 3,
      },
    },
    {
      timestamps: true,
    }
  );

  const User = mongoose.model(“User”, userSchema);

  module.exports = User;
```

4. Criar rotas CRUD no arquivo routes/users.js:

```javascript
  const router = require(“express”).Router();
  let User = require(“../models/user.model”);

  router.route(“/”).get((req, res) => {
    User.find()
      .then((users) => res.json(users))
      .catch((err) => res.status(400).json(“Error: “ + err));
  });

  router.route(“/add”).post((req, res) => {
  const username = req.body.username;

  const newUser = new User({ username });

  newUser
    .save()
    .then(() => res.json(“User added!”))
    .catch((err) => res.status(400).json(“Error: “ + err));
    });

  router.route(“/:id”).get((req, res) => {
    User.findById(req.params.id)
    .then((user) => res.json(user))
    .catch((err) => res.status(400).json(“Error: “ + err));
  });

  router.route(“/:id”).delete((req, res) => {
    User.findByIdAndDelete(req.params.id)
    .then(() => res.json(“User deleted.”))
    .catch((err) => res.status(400).json(“Error: “ + err));
  });

  router.route(“/update/:id”).post((req, res) => {
    User.findById(req.params.id)
    .then((user) => {
      user.username = req.body.username;

      user
      .save()
      .then(() => res.json(“User updated!”))
      .catch((err) => res.status(400).json(“Error: “ + err));
    })
    .catch((err) => res.status(400).json(“Error: “ + err));
  });
```

5. Configurar as rotas no arquivo server.js:

Este código define a rota base /users para as rotas definidas no arquivo users.js.
```javascript
  const userRouter = require(“./routes/users”);
  app.use(“/users”, userRouter);
```

6. Criar um componente React para exibir uma lista de usuários:

Este componente React faz uma requisição GET ao back-end para obter a lista de usuários e renderiza uma tabela com os nomes de usuário e um botão de exclusão para cada usuário.

```javascript
    import React, { useState, useEffect } from “react”;
    import axios from “axios”;

    const UserList = () => {
    const [users, setUsers] = useState([]);

    useEffect(() => {
      axios.get(“http://localhost:5000/users/").then((res) => {
        setUsers(res.data);
      });
    }, []);

    const deleteUser = (id) => {
      axios.delete(`http://localhost:5000/users/${id}`).then((res) => {
        setUsers(users.filter((user) => user._id !== id));
      });
    };

    const userList = () => {
    return users.map((user) => {
    return (
      <tr key={user._id}>
        <td>{user.username}</td>
        <td>
          <a href=”#” onClick={() => deleteUser(user._id)}>delete</a>
        </td>
      </tr>
    );
    });
    };

    return (
    <div>
      <h3>Users</h3>
      <table className=”table”>
        <thead className=”thead-light”>
          <tr>
            <th>Username</th>
            <th>Actions</th>
          </tr>
        </thead>
        <tbody>{userList()}</tbody>
      </table>
    </div>
    );
    };

    export default UserList;
```

7. Criar um componente React para adicionar um novo usuário:

Este componente React renderiza um formulário para adicionar um novo usuário e faz uma requisição POST ao back-end para criar o novo usuário.

```javascript
  import React, { useState } from “react”;
  import axios from “axios”;

  const CreateUser = () => {
  const [username, setUsername] = useState(“”);

  const onSubmit = (e) => {
    e.preventDefault();

    const user = {
    username: username,
    };

    console.log(user);

    axios.post(“http://localhost:5000/users/add", user).then((res) => {
      console.log(res.data);
    });

    setUsername(“”);
  };

  return (
  <div>
    <h3>Create New User</h3>
    <form onSubmit={onSubmit}>
      <div className=”form-group”>
        <label>Username: </label>
        <input type=”text” required className=”form-control” value={username} onChange={(e) => setUsername(e.target.value)}/>
      </div>
      <div className=”form-group”>
        <input type=”submit” value=”Create User” className=”btn btn-primary” />
      </div>
    </form>
  </div>
  );
  };

  export default CreateUser;
```

8. Criar um componente React para editar um usuário existente:

```javascript
  import React, { useState, useEffect } from “react”;
  import axios from “axios”;

  const EditUser = (props) => {
  const [username, setUsername] = useState(“”);

  const handleChange = (event) => {
  setUsername(event.target.value);
  };

  const handleSubmit = (event) => {
    event.preventDefault();
    const user = {
    username: username,
  };

  axios.put(`http://localhost:5000/users/${props.match.params.id}`, user)
    .then(res => console.log(res.data));
    setUsername(“”);
  };

  useEffect(() => {
    axios.get(`http://localhost:5000/users/${props.match.params.id}`)
    .then((res) => setUsername(res.data.username));
  }, [props.match.params.id]);

  return (
    <div>
      <h2>Edit User</h2>
      <form onSubmit={handleSubmit}>
        <div>
          <label>Username:</label>
          <input type=”text” value={username} onChange={handleChange} />
        </div>
        <button type=”submit”>Update</button>
      </form>
    </div>
    );
  };

  export default EditUser;
```

<div id="p3></div>
CRUD : PHP + Laravel + MySQL
1. Criar um novo projeto Laravel:

```shell
  composer create-project — prefer-dist laravel/laravel laravel-crud
```

2. Configurar as credenciais de banco de dados no arquivo .env:

```.env
  DB_CONNECTION=mysql
  DB_HOST=127.0.0.1
  DB_PORT=3306
  DB_DATABASE=laravel_crud
  DB_USERNAME=root
  DB_PASSWORD=
```

3. Criar uma tabela “users” no banco de dados:

```shell
  php artisan make:model User -m
```

O comando acima cria o modelo User e a migração correspondente. Abrir o arquivo de migração (localizado em database/migrations) e adicionar as colunas:

```
  $table->increments(‘id’);
  $table->string(‘name’);
  $table->string(‘email’)->unique();
  $table->timestamps();
```

Executar a migração:

```shell
  php artisan migrate
```

4. Criar um controlador para lidar com as operações CRUD:

Este comando cria um controlador UserController com métodos para as operações CRUD.

```shell
  php artisan make:controller UserController — resource
```

5. Configurar as rotas no arquivo web.php:

Este código define as rotas para as operações CRUD do controlador UserController.

```shell
  Route::resource(‘users’, ‘UserController’);
```

6. Criar uma view para exibir uma lista de usuários:

Esta view faz um loop sobre a lista de usuários (passada como variável $users) e exibe uma tabela com o nome, email e botões para visualizar, editar e excluir cada usuário.

```php
  @extends(‘layout’)

  @section(‘content’)
  <div class=”row”>
    <div class=”col-lg-12 margin-tb”>
      <div class=”pull-left”>
        <h2>Users</h2>
        </div>
        <div class=”pull-right”>
        <a class=”btn btn-success” href=”{{ route(‘users.create’) }}”> Create New User</a>
      </div>
    </div>
  </div>
  @if ($message = Session::get(‘success’))
    <div class=”alert alert-success”>
      <p>{{ $message }}</p>
    </div>
  @endif
  <table class=”table table-bordered”>
    <tr>
      <th>No</th>
      <th>Name</th>
      <th>Email</th>
      <th width=”280px”>Action</th>
    </tr>
    @foreach ($users as $user)
    <tr>
    <td>{{ ++$i }}</td>
    <td>{{ $user->name }}</td>
    <td>{{ $user->email }}</td>
    <td>
    <form action=”{{ route(‘users.destroy’,$user->id) }}” method=”POST”>
      <a class=”btn btn-info” href=”{{ route(‘users.show’,$user->id) }}”>Show</a>
      <a class=”btn btn-primary” href=”{{ route(‘users.edit’,$user->id) }}”>Edit</a>
      @csrf
      @method(‘DELETE’)
      <button type=”submit” class=”btn btn-danger”>Delete</button>
    </form>
    </td>
  </tr>
  @endforeach
  </table>
  {!! $users->links() !!}
  @endsection
```

7. Criar uma view para adicionar um novo usuário:

Esta view exibe um formulário para adicionar um novo usuário, com campos para nome, email e senha.

```php
  @extends(‘layout’)

  @section(‘content’)
  <div class=”row”>
    <div class=”col-lg-12 margin-tb”>
      <div class=”pull-left”>
        <h2>Create New User</h2>
      </div>
      <div class=”pull-right”>
        <a class=”btn btn-primary” href=”{{ route(‘users.index’) }}”> Back</a>
      </div>
    </div>
  </div>
  @if ($errors->any())
  <div class=”alert alert-danger”>
    <strong>Whoops!</strong> There were some problems with your input.<br><br>
    <ul>
      @foreach ($errors->all() as $error)
      <li>{{ $error }}</li>
      @endforeach
    </ul>
  </div>
  @endif
  <form action=”{{ route(‘users.store’) }}” method=”POST”>
    @csrf
    <div class=”row”>
      <div class=”col-xs-12 col-sm-12 col-md-12">
          <div class=”form-group”>
            <strong>Name:</strong>
            <input type=”text” name=”name” class=”form-control” placeholder=”Name”>
          </div>
        </div>
        <div class=”col-xs-12 col-sm-12 col-md-12">
            <div class=”form-group”>
              <strong>Email:</strong>
              <input type=”email” name=”email” class=”form-control” placeholder=”Email”>
            </div>
          </div>
          <div class=”col-xs-12 col-sm-12 col-md-12">
            <div class=”form-group”>
              <strong>Password:</strong>
              <input type=”password” name=”password” class=”form-control” placeholder=”Password”>
            </div>
          </div>
          <div class=”col-xs-12 col-sm-12 col-md-12 text-center”>
          <button type=”submit” class=”btn btn-primary”>Submit</button>
        </div>
    </div>
  </form>
  @endsection

```

8. Criar um método store no controlador UserController para processar a adição de um novo usuário:

Este método recebe uma requisição HTTP POST com os dados do novo usuário e os valida. Se a validação for bem-sucedida, cria um novo objeto User e salva no banco de dados.

```php
  public function store(Request $request)
  {
    $request->validate([
      ‘name’ => ‘required’,
      ‘email’ => ‘required|email|unique:users’,
      ‘password’ => ‘required’,
    ]);

    $user = new User;
    $user->name = $request->name;
    $user->email = $request->email;
    $user->password = Hash::make($request->password);
    $user->save();

    return redirect()->route(‘users.index’)
      ->with(‘success’,’User created successfully.’);
  }
```

9. Criar uma view para exibir os detalhes de um usuário:

Esta view exibe os detalhes de um usuário, incluindo nome, email, data de criação e data de atualização.

```php
  @extends(‘layout’)

  @section(‘content’)
  <div class=”row”>
    <div class=”col-lg-12 margin-tb”>
      <div class=”pull-left”>
        <h2>Show User</h2>
      </div>
      <div class=”pull-right”>
        <a class=”btn btn-primary” href=”{{ route(‘users.index’) }}”> Back</a>
      </div>
    </div>
  </div>
  <div class=”row”>
    <div class=”col-xs-12 col-sm-12 col-md-12">
      <div class=”form-group”>
        <strong>Name:</strong>
        {{ $user->name }}
      </div>
    </div>
    <div class=”col-xs-12 col-sm-12 col-md-12">
      <div class=”form-group”>
        <strong>Email:</strong>
        {{ $user->email }}
      </div>
    </div>
    <div class=”col-xs-12 col-sm-12 col-md-12">
    <div class=”form-group”>
      <strong>Created At:</strong>
      {{ date_format($user->created_at, ‘jS M Y’) }}
    </div>
    </div>
    <div class=”col-xs-12 col-sm-12 col-md-12">
    <div class=”form-group”>
      <strong>Updated At:</strong>
      {{ date_format($user->updated_at, ‘jS M Y’) }}
    </div>
  </div>
  @endsection

```

10. Criar um método show no controlador UserController para exibir os detalhes de um usuário:

Este método recebe um objeto User como parâmetro e retorna a view de detalhes de usuário.

```php
  public function show(User $user)
  {
    return view(‘users.show’,compact(‘user’));
  }
```

11 . Criar uma view para editar um usuário:

Esta view exibe um formulário para editar um usuário existente, com campos para nome e email.

```php
  @extends(‘layout’)

  @section(‘content’)
  <div class=”row”>
  <div class=”col-lg-12 margin-tb”>
  <div class=”pull-left”>
  <h2>Edit User</h2>
  </div>
  <div class=”pull-right”>
  <a class=”btn btn-primary” href=”{{ route(‘users.index’) }}”> Back</a>
  </div>
  </div>
  </div>
  @if ($errors->any())
  <div class=”alert alert-danger”>
  <strong>Whoops!</strong> There were some problems with your input.<br><br>
  <ul>
  @foreach ($errors->all() as $error)
  <li>{{ $error }}</li>
  @endforeach
  </ul>
  </div>
  @endif
  <form action=”{{ route(‘users.update’,$user->id) }}” method=”POST”>
    @csrf
    @method(‘PUT’)
    <div class=”row”>
      <div class=”col-xs-12 col-sm-12 col-md-12">
        <div class=”form-group”>
          <strong>Name:</strong>
          <input type=”text” name=”name” value=”{{ $user->name }}” class=”form-control” placeholder=”Name”>
        </div>
      </div>
      <div class=”col-xs-12 col-sm-12 col-md-12">
        <div class=”form-group”>
          <strong>Email:</strong>
          <input type=”email” name=”email” value=”{{ $user->email }}” class=”form-control” placeholder=”Email”>
        </div>
      </div>
      <div class=”col-xs-12 col-sm-12 col-md-12 text-center”>
        <button type=”submit” class=”btn btn-primary”>Submit</button>
      </div>
    </div>
  </form>
  @endsection
```

12. Criar um método edit no controlador UserController para exibir o formulário de edição de usuário:

```php
  public function edit(User $user)
  {
    return view(‘users.edit’,compact(‘user’));
  }
```

13. Criar um método update no controlador UserController para atualizar um usuário existente:

Este método recebe uma requisição HTTP com os dados atualizados do usuário e o objeto User que será atualizado. Primeiro, validamos os dados recebidos e, em seguida, atualizamos o usuário com os novos dados e redirecionamos para a página de listagem de usuários.

```php
  public function update(Request $request, User $user)
  {
    $request->validate([
      ‘name’ => ‘required’,
      ‘email’ => ‘required|email|unique:users,email,’.$user->id,
    ]);

    $user->update($request->all());

    return redirect()->route(‘users.index’)
    ->with(‘success’,’User updated successfully’);
  }
```

14. Criar uma rota para exibir a view de criação de usuário:

Esta rota aponta para o método create do controlador UserController.

```php
  Route::get(‘/users/create’, [UserController::class, ‘create’])->name(‘users.create’);
```

15. Criar uma rota para exibir a view de detalhes de usuário:

Esta rota aponta para o método show do controlador UserController, e usa a sintaxe de parâmetro dinâmico para passar o objeto User como parâmetro.

```php
  Route::get(‘/users/{user}’, [UserController::class, ‘show’])->name(‘users.show’);
```

16. Criar uma rota para exibir o formulário de edição de usuário:

Esta rota aponta para o método edit do controlador UserController, e também usa a sintaxe de parâmetro dinâmico para passar o objeto User como parâmetro.

```php
  Route::get(‘/users/{user}/edit’, [UserController::class, ‘edit’])->name(‘users.edit’);
```

17. Criar uma rota para atualizar um usuário existente:

Esta rota aponta para o método update do controlador UserController, e também usa a sintaxe de parâmetro dinâmico para passar o objeto User como parâmetro.

```php
  Route::put(‘/users/{user}’, [UserController::class, ‘update’])->name(‘users.update’);
```
<div id="p4></div>
CRUD : Rust + MySQL
Para criar um CRUD básico em Rust com banco de dados MySQL, vamos utilizar as seguintes bibliotecas:

mysql — para interagir com o banco de dados MySQL.
serde e serde_derive — para serializar e desserializar os dados.

1. Adicionar as dependências ao arquivo Cargo.toml:

```shell
  [dependencies]
  mysql = “20.0.1”
  serde = { version = “1.0”, features = [“derive”] }
```

2. Criar uma estrutura User para representar um usuário:

Esta estrutura contém os campos id, name, email e password, onde id é opcional, já que o valor será gerado pelo banco de dados.

```rust
  use serde::{Deserialize, Serialize};

  #[derive(Debug, Serialize, Deserialize)]
  struct User {
    id: Option<u32>,
    name: String,
    email: String,
    password: String,
  }
```

3. Criar uma conexão com o banco de dados MySQL:

Este método retorna uma conexão com o banco de dados MySQL utilizando as informações de conexão fornecidas.

```rust
  use mysql::prelude::*;
  use mysql::{from_row, OptsBuilder};

  fn establish_connection() -> mysql::Pool {
    let opts = OptsBuilder::new()
    .ip_or_hostname(Some(“localhost”))
    .user(Some(“root”))
    .pass(Some(“password”))
    .db_name(Some(“mydatabase”));

    mysql::Pool::new(opts).unwrap()
  }
```

4. Criar um método create para inserir um novo usuário no banco de dados:

Este método insere um novo usuário na tabela users do banco de dados MySQL utilizando os valores passados na estrutura User.

```rust
  fn create(conn: &mysql::Pool, user: User) -> Result<(), mysql::Error> {
    conn.exec_drop(
      “INSERT INTO users (name, email, password) VALUES (:name, :email, :password)”,
      params! {
        “name” => user.name,
        “email” => user.email,
        “password” => user.password
      }
    )?;
    Ok(())
  }
```

5. Criar um método read para ler um usuário do banco de dados:

Este método lê um usuário do banco de dados MySQL a partir do ID passado como parâmetro e retorna um objeto Option<User>.

```rust
  fn read(conn: &mysql::Pool, user_id: u32) -> Result<Option<User>, mysql::Error> {
    let mut stmt = conn.prepare(
      “SELECT id, name, email, password FROM users WHERE id = :id”
    )?;
    let result = stmt.execute(params! {“id” => user_id})?;
    let mut rows = result.collect::<Vec<_>>().unwrap();

    if rows.is_empty() {
      Ok(None)
    } else {
      let row = rows.pop().unwrap();
      Ok(Some(User {
        id: Some(from_row(row.clone())?),
        name: from_row(row.clone())?,
        email: from_row(row.clone())?,
        password: from_row(row.clone())?,
      }))
    }
  }
```

6. Criar um método update para atualizar um usuário existente:

Este método atualiza um usuário existente na tabela `users` do banco de dados MySQL utilizando os valores passados na estrutura `User`.

```rust
  fn update(conn: &mysql::Pool, user: User) -> Result<(), mysql::Error> {
    conn.exec_drop(
      “UPDATE users SET name = :name, email = :email, password = :password WHERE id = :id”,
      params! {
      “id” => user.id.unwrap(),
      “name” => user.name,
      “email” => user.email,
      “password” => user.password
    }
    )?;
    Ok(())
```

7. Criar um método `delete` para excluir um usuário do banco de dados:

Este método exclui um usuário da tabela users do banco de dados MySQL a partir do ID passado como parâmetro.

```rust
  fn delete(conn: &mysql::Pool, user_id: u32) -> Result<(), mysql::Error> {
    conn.exec_drop(
      “DELETE FROM users WHERE id = :id”,
      params! {
      “id” => user_id
    }
  )?;
  Ok(())
  }
```

8. Utilizando os métodos criados:

```rust
  fn main() -> Result<(), mysql::Error> {
  let conn = establish_connection();

  let user1 = User {
    id: None,
    name: “John”.to_string(),
    email: “john@example.com”.to_string(),
    password: “password”.to_string(),
  };

  create(&conn, user1)?;

  let user2 = read(&conn, 1)?;
  println!(“{:?}”, user2);

  let mut user3 = read(&conn, 1)?.unwrap();
  user3.name = “Jane”.to_string();
  update(&conn, user3)?;

  let user4 = read(&conn, 1)?;
  println!(“{:?}”, user4);

  delete(&conn, 1)?;

  let user5 = read(&conn, 1)?;
  println!(“{:?}”, user5);

  Ok(())
}
```
<div id="p5></div>
CRUD : LUA + MySQL
Em Lua, podemos criar um CRUD simples utilizando o módulo luasql para acessar o banco de dados MySQL.

1. Instale o módulo luasql.mysql:

```bash
  luarocks install luasql-mysql
```

2. Importe o módulo e estabeleça a conexão com o banco de dados:

Neste exemplo, o banco de dados é chamado “mydatabase” e o usuário e a senha são “user” e “password”, respectivamente.

```lua
local mysql = require “luasql.mysql”

— Conexão com o banco de dados
  local env = mysql.mysql()
  local conn = env:connect(“mydatabase”, “user”, “password”, “localhost”)
```

3. Crie uma tabela no banco de dados MySQL chamada “users”:

```sql
  CREATE TABLE users (
    id INT(11) NOT NULL AUTO_INCREMENT,
    name VARCHAR(255) NOT NULL,
    email VARCHAR(255) NOT NULL,
    password VARCHAR(255) NOT NULL,
  PRIMARY KEY (id)
  ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
```

4. Defina uma estrutura User:

```lua
    local User = {
    id = 0,
    name = “”,
    email = “”,
    password = “”
  }
```

5. Criar um método create para inserir um novo usuário no banco de dados:

Este método insere um novo usuário na tabela users do banco de dados MySQL utilizando os valores passados na estrutura User.

```lua
  function create(conn, user)
    local stmt = conn:prepare(“INSERT INTO users (name, email, password) VALUES (?, ?, ?)”)
    stmt:bind(user.name, user.email, user.password)
    stmt:execute()
    stmt:close()
  end
```

6. Criar um método read para ler um usuário do banco de dados a partir de um ID:

Este método lê um usuário da tabela users do banco de dados MySQL a partir do ID passado como parâmetro.

```lua
  function read(conn, user_id)
    local stmt = conn:prepare(“SELECT * FROM users WHERE id = ?”)
    stmt:bind(user_id)
    local row = stmt:fetch({}, “a”)
    stmt:close()

    if row then
      local user = {
      id = row.id,
      name = row.name,
      email = row.email,
      password = row.password
      }
      return user
    else
      return nil
    end
  end
```

7. Criar um método update para atualizar um usuário existente no banco de dados:

Este método atualiza um usuário existente na tabela users do banco de dados MySQL utilizando os valores passados na estrutura User.

```lua
  function update(conn, user)
    local stmt = conn:prepare(“UPDATE users SET name = ?, email = ?, password = ? WHERE id = ?”)
    stmt:bind(user.name, user.email, user.password, user.id)
    stmt:execute()
    stmt:close()
  end
```
8. Criar um método delete para excluir um usuário do banco de dados:

Este método exclui um usuário da tabela users do banco de dados MySQL a partir do ID passado como parâmetro.

```lua
  function delete(conn, user_id)
    local stmt = conn:prepare(“DELETE FROM users WHERE id = ?”)
    stmt:bind(user_id)
    stmt:execute()
    stmt:close()
  end
```

<div id="p6></div>

<br><br><div id=”p6"><h2>CRUD : <b>Python + MySQL</b></h2></div>

Em Python, podemos criar um CRUD simples utilizando o pacote mysql-connector-python para acessar o banco de dados MySQL.

1. Instale o pacote mysql-connector-python:
```bash
  pip install mysql-connector-python
```

2. Importe o pacote e estabeleça a conexão com o banco de dados:

Neste exemplo, o banco de dados é chamado “mydatabase” e o usuário e a senha são “user” e “password”, respectivamente.

```python
  import mysql.connector

  # Conexão com o banco de dados
  cnx = mysql.connector.connect(user=’user’, password=’password’,
  host=’localhost’,
  database=’mydatabase’)
  cursor = cnx.cursor()
```

3. Crie uma tabela no banco de dados MySQL chamada “users”:
```sql
  CREATE TABLE users (
    id INT(11) NOT NULL AUTO_INCREMENT,
    name VARCHAR(255) NOT NULL,
    email VARCHAR(255) NOT NULL,
    password VARCHAR(255) NOT NULL,
  PRIMARY KEY (id)
  ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
```

4. Defina uma classe User:

```python
  class User:
    def __init__(self, id, name, email, password):
    self.id = id
    self.name = name
    self.email = email
    self.password = password
```

5. Criar um método create para inserir um novo usuário no banco de dados:

Este método insere um novo usuário na tabela users do banco de dados MySQL utilizando os valores passados na classe User.

```python
  def create(cursor, user):
    add_user = (“INSERT INTO users “
    “(name, email, password) “
    “VALUES (%s, %s, %s)”)
    data_user = (user.name, user.email, user.password)
    cursor.execute(add_user, data_user)
    cnx.commit()
```

6. Criar um método read para ler um usuário do banco de dados a partir de um ID:

Este método lê um usuário da tabela users do banco de dados MySQL a partir do ID passado como parâmetro.

```python
  def read(cursor, user_id):
    query = (“SELECT id, name, email, password FROM users WHERE id = %s”)
    cursor.execute(query, (user_id,))
    row = cursor.fetchone()

    if row:
      user = User(row[0], row[1], row[2], row[3])
      return user
      else:
    return None
```

7. Criar um método update para atualizar um usuário existente no banco de dados:

Este método atualiza um usuário existente na tabela users do banco de dados MySQL utilizando os valores passados na classe User.

```python
  def update(cursor, user):
    update_user = (“UPDATE users SET name=%s, email=%s, password=%s “
    “WHERE id=%s”)
    data_user = (user.name, user.email, user.password, user.id)
    cursor.execute(update_user, data_user)
    cnx.commit()
```

8. Criar um método delete para excluir um usuário do banco de dados:

Este método exclui um usuário da tabela users do banco de dados MySQL a partir do ID passado como parâmetro.
```python
  def delete(cursor, user_id):
    delete_user = (“DELETE FROM users WHERE id=%s”)
    cursor.execute(delete_user, (user_id,))
    cnx.commit()
```

Quando iniciei em desenvolvimento, a 158 anos atrás :) , o grande desafio foi entender conteitos básico como <b>CRUD</b> e optar pela linguagem que eu utilizaria para me aprofundar na área dev.

Meu intuito aqui é te ajudar neste empurrão inicial, direcionando para que seu(s) primeiro(s) projeto(s) saim do papel. Os próximos serão bem mais fáceis e te farão evoluir ainda mais rápido 🚀 !!!

Bora codar e se precisar me chama lá no [Linkedin](https://www.linkedin.com/in/olavo-mello/)
