**Schema**

---

Define a **estrutura e organização dos dados** dentro de um sistema.

Usado principalmente em bancos de dados para garantir **consistência, validação e previsibilidade**.

---

> *O que é um schema na prática?*
> *R: É o “contrato” que define como os dados devem ser armazenados e estruturados.*

---

## Em banco de dados

Define:

* Tabelas
* Colunas
* Tipos de dados
* Relacionamentos

Exemplo:

```sql id="y8f2pd"
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  name VARCHAR(100),
  email VARCHAR(100) UNIQUE,
  created_at TIMESTAMP
);
```

---

> `• estrutura dos dados`
> `• tipos definidos`
> `• regras de integridade`

---

## Validação de dados

Schema também pode validar entrada de dados antes de salvar ou processar.

Exemplo (TypeScript + validação):

```typescript id="c3m9qv"
import { z } from 'zod';

const UserSchema = z.object({
  name: z.string(),
  email: z.string().email(),
  age: z.number().min(18)
});

UserSchema.parse({
  name: "Wesley",
  email: "wesley@email.com",
  age: 25
});
```

---

> `• valida entrada`
> `• evita dados inválidos`
> `• padroniza formato`

---

## Tipos comuns de schema

```text id="h2v7sk"
Database Schema   → estrutura do banco
JSON Schema       → validação de JSON
GraphQL Schema    → define queries e tipos
```

---

## Exemplo JSON Schema

```json id="u5n1xr"
{
  "type": "object",
  "properties": {
    "name": { "type": "string" },
    "email": { "type": "string" }
  },
  "required": ["name", "email"]
}
```

---

> `• estrutura JSON`
> `• valida formato`
> `• usado em APIs`

---

## Problema clássico

```text id="k9r4bx"
Sem schema:

1. Dados inconsistentes
2. Campos faltando
3. Tipos errados
```

---

```text id="d7w3pt"
Com schema:

1. Estrutura definida
2. Validação automática
3. Dados previsíveis
```

---

> `• reduz erro`
> `• melhora confiabilidade`
> `• facilita manutenção`
