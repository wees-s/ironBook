**Migration**

---

Processo de **alterar a estrutura do banco de dados de forma controlada e versionada**.

Permite evoluir o schema sem quebrar o sistema.

---

> *O que é uma migration na prática?*
> *R: Um arquivo que descreve mudanças no banco (criar tabela, alterar coluna, etc.).*

---

## Para que serve

* Criar tabelas
* Alterar colunas
* Adicionar/remover campos
* Versionar mudanças no banco

---

> `• evolução do banco`
> `• controle de versão`
> `• mudanças rastreáveis`

---

## Exemplo

```sql id="k2f8nz"
-- up
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  name VARCHAR(100)
);

-- down
DROP TABLE users;
```

---

> `• up aplica mudança`
> `• down reverte mudança`

---

## Fluxo

```text id="p4v9qm"
1. Criar migration
2. Executar (apply)
3. Banco atualizado
4. Versionamento mantido
```

---

## Exemplo com ferramenta (Node.js)

```bash id="m7t1xr"
npx prisma migrate dev
```

---

> `• aplica alterações`
> `• sincroniza schema`
> `• mantém histórico`

---

## Problema clássico

```text id="c9k2wl"
Sem migration:

1. Dev altera banco manualmente
2. Outro ambiente fica diferente
3. Sistema quebra
```

---

```text id="z3h8vs"
Com migration:

1. Mudança versionada
2. Todos aplicam igual
3. Banco consistente
```

---

> `• evita divergência`
> `• garante consistência`
> `• facilita rollback`

---

## Boas práticas

* Nunca editar banco manualmente em produção
* Versionar todas as mudanças
* Sempre ter `down` (rollback)
* Rodar migrations em staging antes de produção

---

> `• controle total do banco`
> `• previsibilidade`
> `• segurança em mudanças`
