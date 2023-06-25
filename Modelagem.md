<p float="left">

 <img src="https://user-images.githubusercontent.com/img\Modelagem.png" width="200" />

</p>

Comandos MongoDB:

Criar as coleções e inserir os dados:


use GerenciamentoPedidosPizzaria


db.createCollection("clientes")
db.createCollection("pedidos")
db.createCollection("pizzas")
db.createCollection("bebidas")


db.clientes.insertOne({
  "_id": "cliente1",
  "nome": "João",
  "endereco": "Rua A, 123",
  "telefone": "987654321"
})


db.pedidos.insertOne({
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


db.pizzas.insertOne({
  "_id": "pizza1",
  "preco": 20.0,
  "tamanho": "pequena",
  "sabor": "margherita",
  "descricao": "Molho de tomate, mussarela, manjericão"
})


db.bebidas.insertOne({
  "_id": "bebida1",
  "tipo": "refrigerante",
  "nome": "Coca-Cola",
  "preco": 5.0,
  "litros": 2
})


Atualizações e exclusões de dados:

/* Atualizar o nome do cliente */
db.clientes.updateOne({ "_id": "cliente1" }, { $set: { "nome": "Maria" } })


/* Excluir um pedido */
db.pedidos.deleteOne({ "_id": "pedido1" })








Consultas:

Consulta simples:

/*Consultar todos os clientes */
db.clientes.find()


/* Consultar todos os pedidos */
db.pedidos.find()


/* Consultar todas as pizzas */
db.pizzas.find()


/*Consultar todas as bebidas */
db.bebidas.find()





Consulta com uma condição:

/* Consultar pedidos com tipo "entrega" */
db.pedidos.find({ "tipo": "entrega" })


/* Consultar pizzas com tamanho "média" */
db.pizzas.find({ "tamanho": "média" })


/* Consultar bebidas com preço menor que 10.0 */
db.bebidas.find({ "preco": { $lt: 10.0 } })





Consulta utilizando agreggate com $group (Group by):
Consulta para contar quantas vezes uma pizza de tamanho "pequena" foi pedida



db.pedidos.aggregate([
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






Consulta utilizando agreggate com $lookup (Joins):
Consulta para contar quantas vezes um cliente fez pedidos e mostrar o nome dele

db.pedidos.aggregate([
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

