**Agile Methods (Epic / Story / Task / Subtask)**

---

Estrutura de organização de trabalho onde cada nível define **escopo, granularidade e responsabilidade**.

---

> *Qual a diferença entre eles?*
> *R: É uma hierarquia de decomposição, desde a estratégia ao executável.*

---

## Epic

Grande bloco de trabalho.

* Alto nível
* Longo prazo
* Agrupa múltiplas stories

Exemplo:

```text id="y3k8qp"
Sistema de autenticação completo
```

---

> `• visão macro`
> `• longo prazo`
> `• não executável direto`

---

## Story

Funcionalidade do ponto de vista do usuário.

* Descreve valor entregue
* Ainda não é implementação
* Baseada em necessidade

Exemplo:

```text id="n8v2fw"
Como usuário, quero fazer login para acessar o sistema
```

---

> `• foco no usuário`
> `• define comportamento`
> `• não define implementação`

---

## Task

Unidade de trabalho **técnica e executável**.

* Derivada de uma story
* Já define ação concreta
* Pode ser atribuída

Exemplo:

```text id="c6m1zr"
Implementar endpoint POST /login
```

---

> `• nível de execução`
> `• técnica definida`
> `• pronta para codar`

---

## Subtask

Quebra ainda mais granular da task.

* Passos específicos
* Execução direta
* Ideal para automação

Exemplo:

```text id="f4q9lx"
1. Criar controller de login
2. Validar credenciais
3. Gerar JWT
4. Retornar resposta
```

---

> `• micro execução`
> `• altamente específica`
> `• baixo nível`

---

## Hierarquia

```text id="z2p7kd"
Epic → Story → Task → Subtask
```

---

## Regra para uso com IA

```text id="w5x8vn"
IA deve atuar em:
Task
Subtask
```

```text id="r1m4qy"
IA NÃO deve atuar em:
Story
Epic
```

---

> `• story é ambígua`
> `• epic é abstrata`
> `• task é executável`

---

## Problema clássico

```text id="u7n3ab"
IA recebe:

"Crie um sistema de autenticação"
```

Resultado:

* Escopo indefinido
* Decisões arbitrárias
* Código inconsistente

---

```text id="d9k6ht"
IA recebe:

"Implementar endpoint POST /login com JWT"
```

Resultado:

* Escopo claro
* Execução direta
* Código previsível

```

---

> `• granularidade define qualidade`
> `• quanto mais específico, melhor a execução`
> `• IA precisa de contexto técnico e não abstrato`
```
