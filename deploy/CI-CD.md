**CI/CD (Continuous Integration / Continuous Delivery)**

---

Pipeline de automação para **build, teste e deploy de código**.

Garante que mudanças sejam **integradas, validadas e entregues com consistência**.

---

> *O que isso resolve?*
> *R: Evita deploy manual, reduz erro humano e padroniza entrega.*

---

## CI (Continuous Integration)

Processo de **integrar código frequentemente** com validações automáticas.

* Build automático
* Testes automáticos
* Verificação de qualidade

Exemplo de fluxo:

```text id="k3m9qp"
1. Dev faz push
2. Pipeline inicia
3. Build executa
4. Testes rodam
5. Código validado
```

---

> `• valida código automaticamente`
> `• detecta erro cedo`
> `• mantém qualidade`

---

## CD (Continuous Delivery / Deployment)

Processo de **entregar código pronto para produção**.

* Delivery → pronto para deploy manual
* Deployment → deploy automático

Exemplo de fluxo:

```text id="v7x2rn"
1. CI aprovado
2. Deploy em staging
3. (opcional) aprovação manual
4. Deploy em produção
```

---

> `• entrega contínua`
> `• deploy previsível`
> `• menos intervenção manual`

---

## Pipeline

Sequência de etapas automatizadas.

```text id="p4z6lw"
build → test → deploy
```

---

> `• fluxo definido`
> `• execução automática`
> `• padronização`

---

## Exemplo (conceito YAML)

```yaml id="c8t1mk"
steps:
  - name: install
    run: npm install

  - name: test
    run: npm test

  - name: build
    run: npm run build

  - name: deploy
    run: ./deploy.sh
```

---

> `• configuração declarativa`
> `• etapas sequenciais`
> `• automatização total`

---

## Problema clássico

```text id="r2k7vs"
Sem CI/CD:

1. Deploy manual
2. Esquece etapa
3. Bug em produção
```

---

```text id="d5w4ym"
Com CI/CD:

1. Pipeline executa
2. Tudo validado
3. Deploy consistente
```

---

> `• reduz erro humano`
> `• aumenta confiabilidade`
> `• acelera entrega`

---

## Regra prática

```text id="x1q9pz"
Se precisa rodar manualmente, deveria estar no pipeline
```

---

> `• automatizar tudo que for repetível`
> `• garantir consistência`
> `• manter fluxo previsível`
