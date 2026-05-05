**AWS Lambda**

---

Comparação entre execução tradicional baseada em servidor e modelo serverless usando Amazon Web Services.

---

> *Qual a diferença central entre AWS Lambda e uma VM?*
> *R: Resumo: VM você gerencia o servidor. Lambda você só envia a função.*

---

## AWS Lambda

Execução de código **sob demanda e orientada a eventos**.

* Sem gerenciamento de servidor
* Execução temporária
* Escala automática
* Cobrança por execução

Exemplo de fluxo:

```text id="k7m2xq"
1. Evento ocorre (HTTP, fila, etc.)
2. Lambda é acionada
3. Código executa
4. Encerra execução
```

---

> `• sem servidor`
> `• execução efêmera`
> `• escala automática`

---

## VM (Virtual Machine)

Ambiente completo com **sistema operacional próprio**.

* Controle total da máquina
* Processo contínuo
* Provisionamento manual
* Escala manual

Exemplo de fluxo:

```text id="p3k9zr"
1. Criar VM
2. Instalar dependências
3. Subir aplicação
4. Manter servidor rodando
```

---

> `• controle total`
> `• ambiente persistente`
> `• maior responsabilidade operacional`

---

## Comparação direta

```text id="z5v8nc"
VM                → servidor ativo o tempo todo
AWS Lambda        → executa apenas quando necessário
```

```text id="u1r4qp"
VM                → você gerencia infra
AWS Lambda        → AWS gerencia infra
```

```text id="d9x3kt"
VM                → custo fixo (tempo ligado)
AWS Lambda        → custo por execução
```

---

> `• VM = controle + custo constante`
> `• Lambda = abstração + custo variável`

---

## Analogia: AWS Lambda vs Container

```text id="y2n6mw"
Container → você empacota e executa um ambiente
Lambda    → você envia só a função, o ambiente é gerado automaticamente
```

---

### Container

* Ambiente isolado (ex: Docker)
* Processo controlado por você
* Pode rodar continuamente
* Você define runtime e execução

---

> `• controle do ambiente`
> `• execução contínua`
> `• responsabilidade de gerenciamento`

---

### AWS Lambda

* Ambiente criado sob demanda
* Execução curta e descartada
* Infra totalmente abstraída
* Foco na função, não no ambiente

---

> `• foco na lógica`
> `• ambiente invisível`
> `• execução efêmera`

---

## Analogia prática

```text id="q4t8sv"
Container:
"Tenho uma cozinha equipada e preparo refeições quando quiser"

Lambda:
"Peço um prato pronto — a cozinha aparece, cozinha e some"
```

---

> `• container = você gerencia o ambiente`
> `• lambda = você delega o ambiente`
> `• ambos isolam execução, mas com níveis diferentes de controle`
