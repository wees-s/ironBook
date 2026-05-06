**API (Application Programming Interface)**

---

Interface que permite **comunicação entre sistemas**.

Define como uma aplicação pode **enviar e receber dados** de outra.

---

> *O que é uma API na prática?*
> *R: Um conjunto de endpoints que expõem funcionalidades de um sistema.*

---

## Ideia central

```text id="z8m2qp"
Cliente → API → Sistema
```

* Cliente faz requisição
* API recebe
* Sistema processa
* API retorna resposta

---

> `• comunicação entre sistemas`
> `• entrada e saída de dados`
> `• contrato definido`

---

## Endpoint

Ponto de acesso da API.

* URL + método HTTP
* Representa uma ação

Exemplo:

```text id="q1v7xn"
GET /users
POST /login
DELETE /orders/1
```

---

> `• define ação`
> `• acessa recurso`
> `• estrutura padronizada`

---

## Métodos HTTP

Principais operações:

```text id="m5k9zr"
GET     → buscar dados
POST    → criar
PUT     → atualizar
DELETE  → remover
```

---

> `• CRUD via HTTP`
> `• comportamento previsível`
> `• padrão web`

---

## Exemplo de requisição

```http id="c3p8wd"
POST /login
Content-Type: application/json

{
  "email": "user@email.com",
  "password": "123456"
}
```

---

## Exemplo de resposta

```json id="t7x2lk"
{
  "token": "abc123",
  "user": {
    "id": 1,
    "name": "Wesley"
  }
}
```

---

> `• troca de dados`
> `• formato estruturado`
> `• comunicação clara`

---

## Tipos comuns

```text id="h4n6qy"
REST     → baseado em HTTP
GraphQL  → consulta flexível
gRPC     → comunicação binária performática
```

---

## Problema clássico

```text id="v9k1ts"
Sem API:

1. Sistemas acoplados
2. Acesso direto ao banco
3. Difícil escalar
```

---

```text id="d2m8rf"
Com API:

1. Interface definida
2. Sistemas desacoplados
3. Escalabilidade maior
```

---

> `• reduz acoplamento`
> `• melhora organização`
> `• facilita integração`

---

## Regra prática

```text id="n7q3xp"
API define o contrato
Cliente consome sem saber a implementação interna
```

---

> `• abstração`
> `• padronização`
> `• controle de acesso`
