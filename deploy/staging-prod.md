**Staging vs Production**

---

Separação de ambientes onde:

* **Staging** → validação antes do deploy
* **Production** → execução real do sistema

Reduz risco e controla qualidade de entrega.

---

> *Qual a diferença prática?*
> *R: Staging replica o comportamento. Production executa com usuários e dados reais.*

---

## Staging

Ambiente para **testes finais e validação**.

* Infraestrutura **similar à produção**
* Dados:

  * mockados **ou**
  * anonimizados
* Deploy frequente (CI/CD)
* Acesso restrito

Exemplo de fluxo:

```text
1. Feature finalizada
2. Deploy em staging
3. Testes (QA / integração)
4. Ajustes
5. Aprovação
```

---

> `• valida antes de ir ao ar`
> `• ambiente controlado`
> `• reduz risco de falha`

---

## Production

Ambiente de **execução real**.

* Dados reais
* Alta disponibilidade
* Monitoramento ativo
* Deploy controlado (rollback necessário)

Exemplo de fluxo:

```text
1. Código aprovado
2. Deploy em produção
3. Usuários acessam
4. Logs e métricas monitorados
```

---

> `• sistema em uso`
> `• impacto direto`
> `• exige estabilidade`

---

## Diferença direta

```text
Staging     → validar comportamento
Production  → executar com usuários reais
```

---

## Custo vs Benefício

### Custo

* Infra duplicada (compute, banco, rede)
* Manutenção de ambientes
* Pipeline de deploy mais complexo

---

### Benefício

* Detecção de bugs antes de produção
* Redução de incidentes
* Menor custo de correção (bug antes ≪ bug em produção)
* Maior previsibilidade de deploy
* Segurança em mudanças críticas

---

> `• mais custo operacional`
> `• muito menos custo de erro`

---

## Problema clássico

```text
Sem staging:

1. Dev faz mudança
2. Deploy direto em produção
3. Bug aparece
4. Sistema quebra
```

---

> `• sem validação`
> `• risco alto`
> `• correção sob pressão`

---

## Regra prática

```text
Ambientes diferentes → comportamento imprevisível
Ambientes iguais     → deploy confiável
```

---

> `• manter paridade entre ambientes`
> `• mesma config`
> `• mesma versão de dependências`
