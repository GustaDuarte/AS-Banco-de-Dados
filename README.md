# AS-Banco-de-Dados

<p float="left">
 <img src="img\Modelagem.png" width="200" />
</p>

<h2>Comandos MongoDB:</h2>

<h1>Criar as coleções e inserir os dados:</h1>

<b>use</b> GerenciamentoPedidosPizzaria

<b>db.createCollection</b>("clientes")
<b>db.createCollection</b>("pedidos")
<b>db.createCollection</b>("pizzas")
<b>db.createCollection</b>("bebidas")


<b>db.clientes.insertOne</b>({
  "_id": "cliente1",
  "nome": "João",
  "endereco": "Rua A, 123",
  "telefone": "987654321"
})


<b>db.pedidos.insertOne</b>({
  "_id": "pedido1",
  "cliente_id": "cliente1",
  "tipo": "entrega",
  "data": "2023-06-21",
  "hora": "18:30",
  "endereco": "Rua A, 123",
  "pizzas": [
    {
      "_id": "pizza1",
      "preco": 20.0,
      "tamanho": "pequena",
      "sabor": "margherita",
      "descricao": "Molho de tomate, mussarela, manjericão"
    },
    {
      "_id": "pizza2",
      "preco": 30.0,
      "tamanho": "média",
      "sabor": "calabresa",
      "descricao": "Molho de tomate, mussarela, calabresa"
    }
  ],
  "bebidas": [
    {
      "_id": "bebida1",
      "tipo": "refrigerante",
      "nome": "Coca-Cola",
      "preco": 5.0,
      "litros": 2
    }
  ],
  "total": 55.0
})


<b>db.pizzas.insertOne</b>({
  "_id": "pizza1",
  "preco": 20.0,
  "tamanho": "pequena",
  "sabor": "margherita",
  "descricao": "Molho de tomate, mussarela, manjericão"
})


<b>db.bebidas.insertOne</b>({
  "_id": "bebida1",
  "tipo": "refrigerante",
  "nome": "Coca-Cola",
  "preco": 5.0,
  "litros": 2
})


<h1>Atualizações e exclusões de dados:</h1>

<h1> Atualizar o nome do cliente </h1>
<b>db.clientes.updateOne</b>({ "_id": "cliente1" }, { $set: { "nome": "Maria" } })


<h1>Excluir um pedido</h1>
<b>db.pedidos.deleteOne</b>({ "_id": "pedido1" })



<h3>Consultas:</h3>

<h2>Consulta simples:</h2>

<h1>Consultar todos os clientes</h1>
<b>db.clientes.find()</b>


<h1>Consultar todos os pedidos</h1>
<b>db.pedidos.find()</b>


<h1>Consultar todas as pizzas</h1>
<b>db.pizzas.find()</b>


<h1>Consultar todas as bebidas</h1>
<b>db.bebidas.find()</b>





<h1>Consulta com uma condição:</h2>

<h1>Consultar pedidos com tipo "entrega"</h1>
<b>db.pedidos.find({ "tipo": "entrega" })</b>


<h1>Consultar pizzas com tamanho "média"</h1>
<b>db.pizzas.find({ "tamanho": "média" })</b>


<h1>Consultar bebidas com preço menor que 10.0</h1>
<b>db.bebidas.find({ "preco": { $lt: 10.0 } })</b>





</h2>Consulta utilizando agreggate com $group (Group by):</h2>
<i>Consulta para contar quantas vezes uma pizza de tamanho "pequena" foi pedida</i>



<b>db.pedidos.aggregate</b>([
  {
    $match: {
      "pizzas.tamanho": "pequena"
    }
  },
  {
    $group: {
      _id: null,
      vezesPedida: { $sum: 1 }
    }
  }
]);



</h2>Consulta utilizando agreggate com $lookup (Joins):</h2>
<i>Consulta para contar quantas vezes um cliente fez pedidos e mostrar o nome dele</i>

<b>db.pedidos.aggregate</b>([
  {
    $lookup: {
      from: "clientes",
      localField: "cliente_id",
      foreignField: "_id",
      as: "cliente"
    }
  },
  {
    $unwind: "$cliente"
  },
  {
    $group: {
      _id: "$cliente_id",
      nomeCliente: { $first: "$cliente.nome" },
      vezesPedida: { $count: {} }
    }
  }
]);
