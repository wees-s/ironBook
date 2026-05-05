**Docker / Dockerfile / Docker Compose / YAML**

---

Stack para **containerização e padronização de ambientes**.
Permite empacotar aplicação + dependências e rodar de forma consistente em qualquer máquina.

---

> *O que isso resolve?*
> *R: Elimina diferença de ambiente e garante execução previsível.*

---

## Docker

Plataforma que executa aplicações em **containers isolados**.

* Compartilha o kernel do host
* Leve comparado a VMs
* Portável

Exemplo:

```bash id="3g9w2a"
docker run -p 3000:3000 node-app
```

---

> `• isolamento de ambiente`
> `• portabilidade`
> `• consistência`

---

## Dockerfile

Arquivo que define **como construir uma imagem**.

* Define imagem base
* Instala dependências
* Copia código
* Define comando de execução

Exemplo:

```dockerfile id="k1d8fh"
FROM node:18

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

CMD ["npm", "start"]
```

---

> `• define o ambiente`
> `• build reproduzível`
> `• versionável`

---

## Docker Compose

Ferramenta para **orquestrar múltiplos containers**.

* Define serviços (app, banco, etc.)
* Cria rede entre eles automaticamente
* Sobe tudo com um comando

Exemplo de fluxo:

```text id="wz8c1m"
1. Define serviços no docker-compose.yml
2. Executa: docker-compose up
3. Containers sobem integrados
```

---

Exemplo:

```yaml id="f2k9qp"
version: '3.8'

services:
  app:
    build: .
    ports:
      - "3000:3000"
    depends_on:
      - db

  db:
    image: postgres:15
    environment:
      POSTGRES_PASSWORD: example
```

---

> `• múltiplos serviços`
> `• rede automática`
> `• ambiente completo`

---

## Arquivos `.yaml` / `.yml`

Formato de **configuração declarativa**.

* Baseado em indentação
* Legível
* Amplamente usado em ferramentas DevOps

Exemplo:

```yaml id="x7n3vs"
app:
  name: api
  port: 3000
```

---

> `• estrutura simples`
> `• fácil leitura`
> `• padrão de configuração`

---

## Relação entre eles

```text id="q0j2lm"
Dockerfile     → define a imagem
Docker         → executa containers
Docker Compose → orquestra serviços
YAML           → formato usado na configuração
```

---

## Problema clássico

```text id="n6c8yr"
Sem Docker:

1. Dependências diferentes
2. Versões conflitantes
3. Ambiente inconsistente
```

---

```text id="b4z1ke"
Com Docker:

1. Ambiente definido no Dockerfile
2. Mesmo setup para todos
3. Execução previsível
```

---

> `• elimina variação de ambiente`
> `• reduz erro de setup`
> `• padroniza execução`
