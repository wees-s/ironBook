**MCP (Model Context Protocol)**

---

Protocolo para **padronizar como aplicações fornecem contexto para IAs**.

Define uma interface para que modelos recebam **dados, ferramentas e estado** de forma estruturada.

---

> *O que resolve?*
> *R: Evita integrações ad-hoc e cria um padrão único para conectar IA com sistemas.*

---

## Ideia central

Separar:

```text id="m2k9qp"
IA (modelo)
↓
MCP (protocolo)
↓
Ferramentas / dados / APIs
```

---

> `• desacopla IA do sistema`
> `• padroniza integração`
> `• reduz complexidade`

---

## Componentes

### Client (IA)

* Modelo que consome contexto
* Solicita dados e ações

---

### Server (MCP Server)

* Expõe ferramentas e dados
* Responde às requisições da IA

---

### Tools

* Funções executáveis
* Ações que a IA pode chamar

Exemplo:

```text id="v7x3rn"
getUser(id)
createOrder(data)
sendEmail(payload)
```

---

> `• ações disponíveis`
> `• interface controlada`
> `• execução externa`

---

### Resources

* Dados disponíveis para leitura
* Contexto estruturado

Exemplo:

```text id="p4z8lw"
users.json
config.yaml
logs/
```

---

> `• fornece contexto`
> `• leitura estruturada`
> `• base para decisão`

---

## Fluxo

```text id="c9t2mk"
1. IA solicita contexto ou ação
2. MCP Server recebe
3. Executa tool ou retorna resource
4. IA continua com base na resposta
```

---

> `• comunicação padronizada`
> `• ida e volta de contexto`
> `• execução controlada`

---

## Exemplo conceitual

```json id="h6n1qb"
{
  "tool": "getUser",
  "params": {
    "id": 123
  }
}
```

---

> `• chamada estruturada`
> `• sem ambiguidade`
> `• fácil integração`

---

## Problema clássico

```text id="r2k5vs"
Sem MCP:

1. Integração direta com APIs
2. Cada sistema com padrão diferente
3. Código acoplado
```

---

```text id="d8w4ym"
Com MCP:

1. Interface padronizada
2. Ferramentas centralizadas
3. IA integrada de forma consistente
```

---

> `• reduz acoplamento`
> `• melhora escalabilidade`
> `• facilita manutenção`

---

## Regra prática

```text id="x1q7pz"
IA não acessa sistema direto
IA usa MCP como camada intermediária
```

---

> `• controle de acesso`
> `• segurança`
> `• previsibilidade`
