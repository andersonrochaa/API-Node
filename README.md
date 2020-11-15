#  Api Node Sem BANCO DE DADOS
- API node.js usando o express. 
- Foram ultilizados objetos para fazer o CRUD.

#### BancoDeDados.js
Módulo responsável pelas funções que faz o CRUD dos objetos.

```javascript
const sequence = {
    _id: 1,
    get id() {return this._id++}
}

const produtos = {
    //produto (objeto com valor de produto) 
}

function salvarProduto(produto){
    if(!produto.id) produto.id = sequence.id
    produtos[produto.id] = produto
    return produto
}
function getProduto(id){
    return produtos[id] || {}
}

function getProdutos(){
    return Object.values(produtos)
}

function excluirProduto(id){
    const produto = produtos[id] 
    delete produtos[id]
    return produto
}

module.exports = {salvarProduto, getProduto, getProdutos, excluirProduto}
```
#### servidor.js
Módulo do servidor , responsável pelo CREATE,POST,PUT... dos objetos.
Captar os requet's e reponse's do front-end.
```javascript
const porta = 3003


const express = require('express')
const app = express()
const bodyParser = require('body-parser')
const bancoDeDados = require('./bancoDeDados')

app.use(bodyParser.urlencoded({extended: true }))

app.get('/produtos', (req, res, next) => {
    res.send(bancoDeDados.getProdutos())
})

app.get('/produtos/:id', (req, res , next) => {
    res.send(bancoDeDados.getProduto(req.params.id))
})

app.post('/produtos', (req , res , next) => {
   const produto = bancoDeDados.salvarProduto({
       nome: req.body.nome,
       preco: req.body.preco
   }) 
   res.send(produto)
})

app.put('/produtos', (req , res , next) => {
    const produto = bancoDeDados.salvarProduto({
        id: req.body.id,
        nome: req.body.nome,
        preco: req.body.preco
    }) 
    res.send(produto)
 })

 app.delete('/produtos/:id', (req , res , next) => {
    const produto = bancoDeDados.excluirProduto(req.params.id) 
    res.send(produtos)
 })


app.listen(porta, () => {
    console.log(`Servidor executanto na porta ${porta}`)
})
```
