# Agentes do Claude Code — Guia Completo

> Este README explica como funcionam os subagentes do Claude Code, como criá-los e usá-los (analisar se faz sentido para o seu contexto).

---

## Índice

1. [O que são subagentes?](#o-que-são-subagentes)
2. [Por que usar?](#por-que-usar)
3. [Como criar](#como-criar)
4. [Onde vivem os arquivos](#onde-vivem-os-arquivos)
5. [Agentes disponíveis no projeto](#agentes-disponíveis-no-projeto)
6. [Fluxo padrão de delegação](#fluxo-padrão-de-delegação)
7. [Uso prático](#uso-prático)
8. [Troubleshooting](#troubleshooting)

---

## O que são subagentes?

Subagentes são **assistentes de IA especializados** que o Claude pode delegar tarefas automaticamente. Cada subagente:

- Roda em sua **própria janela de contexto isolada** (não compartilha tokens com a conversa principal)
- Tem um **system prompt customizado** focado em um tipo específico de trabalho
- Possui **acesso controlado a ferramentas** (pode ter apenas leitura, ou acesso a escrita/bash)
- Trabalha de forma **independente** e retorna apenas os resultados relevantes

### Exemplo visual

```
Você (conversa principal)
   ↓
   ├─→ task-planner (quebra a tarefa)
   │        ↓ retorna: lista de subtarefas
   │
   ├─→ backend-developer (implementa endpoint)
   │        ↓ retorna: código pronto
   │
   ├─→ security-auditor (revisa)
   │        ↓ retorna: relatório de vulns
   │
   └─→ test-writer (cria testes)
          ↓ retorna: testes em pytest
```

A orquestração é **hierárquica**: sempre você (o agente principal) decidindo qual subagente chamar a seguir. Subagentes não chamam outros subagentes.

---

## Por que usar?

### 1. Preservar o contexto principal

O Claude tem uma janela de contexto de 200.000 tokens. Quando você manda o `security-auditor` fazer uma varredura, todos os achados vão para a **janela isolada dele**. Ele retorna um resumo, e você não enche seu contexto principal com detalhes que não vai reler.

**Exemplo:** Uma busca na web pode gerar 50.000 tokens de resultados. Com um subagente de `search-specialist`, você fica com só 500 tokens no seu contexto (o resumo).

### 2. Paralelização

Você pode spinner múltiplos subagentes simultaneamente para tarefas independentes.

**Exemplo:**
```
> Enquanto backend-developer cria o endpoint de login,
> frontend-developer cria a tela de login em paralelo.
```

### 3. Especialização

Cada subagente tem instruções muito focadas. O `auth-specialist` sabe sobre JWT, bcrypt, MFA e rate limiting. O `ui-reviewer` sabe sobre acessibilidade (WCAG), responsividade e design system. Desempenho melhor em domínios específicos.

### 4. Controle fino de custos

Tarefas "automáticas" (lint, formatação, revisão superficial) rodam em **Haiku** (mais rápido, mais barato). Tarefas críticas (segurança, auth) rodam em **Opus** (raciocínio profundo). Você escolhe o modelo certo para cada job.

### 5. Reusabilidade

Uma vez criado, um subagente está disponível em **todos os seus projetos**. Basta salvar em `~/.claude/agents/`.

---

## Como criar

### Opção 1: Via `/agents` no Claude Code Terminal (recomendado)

Essa é a forma interativa e guiada.

```bash
cd seu-projeto
claude
```

Dentro do Claude Code:
```
/agents
```

Escolha:
- `Create new agent`
- `Personal` (para usar em todos os projetos) ou `Project` (só neste projeto)
- `Generate with Claude`

Digite a descrição do agente (em linguagem natural):
```
Um agente especialista em autenticação que cuida de login, 
cadastro, JWT RS256, bcrypt, refresh tokens e controle de 
permissões por role. Use quando a tarefa envolver identidade, 
sessão ou autorização.
```

O Claude vai gerar o arquivo `.md` com `name`, `description` e `system prompt` prontos. Revise e salve.

### Opção 2: Criar manualmente (mais rápido para vários)

Crie o arquivo `.md` na mão:

```bash
# Para agentes pessoais (todos os projetos)
mkdir -p ~/.claude/agents
nano ~/.claude/agents/auth-specialist.md
```

Template básico:

```markdown
---
name: auth-specialist
description: Use quando a tarefa envolver login, cadastro, JWT, OAuth, sessões ou controle de permissões por role
tools: Read, Write, Edit, Bash, Glob, Grep
model: opus
---

Você é um especialista em autenticação e segurança de aplicações web.

## Sua responsabilidade

Implementar e revisar fluxos de autenticação seguindo estas regras **invioláveis**:

### Passwords
- Sempre usar **bcrypt com cost 12**
- Mínimo 12 caracteres
- Validar com zxcvbn antes de aceitar

### JWT
- Algorithm: **RS256** (assimétrico, chaves no Secrets Manager)
- Access token: **15 minutos**
- Refresh token: **7 dias**
- Nunca colocar PII no payload

### Rate Limiting
- `/auth/login`: **10 req/min por IP**
- Demais endpoints: 100 req/min por IP
- 1000 req/min por user autenticado

### MFA
- Obrigatório para admin
- TOTP (time-based one-time password)

### Segurança geral
- HTTPS obrigatório (TLS 1.2+)
- HSTS habilitado
- Tokens no http-only cookie quando possível
- Logout invalida refresh token no backend

## Checklist antes de marcar como pronto

- [ ] Senhas hasheadas com bcrypt cost 12
- [ ] JWT com RS256 e timing correto (15min + 7 dias)
- [ ] Rate limit configurado (10/min no login)
- [ ] MFA implementado para admin
- [ ] Testes incluem cenários de falha (senha errada, token expirado, etc.)
- [ ] Revisado por security-auditor antes de PR
- [ ] Sem secrets hardcoded ou em logs
```

---

## Onde vivem os arquivos

### Agentes Pessoais (todo projeto)

```
~/.claude/agents/
├── auth-specialist.md
├── backend-developer.md
├── code-reviewer.md
├── database-reviewer.md
├── devops.md
├── doc-writer.md
├── frontend-developer.md
├── infra-reviewer.md
├── security-auditor.md
├── task-planner.md
├── test-writer.md
├── ui-reviewer.md
└── architect.md
```

### Agentes do Projeto (só neste projeto)

```
seu-projeto/
├── .claude/
│   └── agents/
│       └── (agentes específicos do projeto)
```

**Ordem de prioridade:** Se o mesmo agente existir em ambos os lugares, **o do projeto tem precedência**.

---

## Agentes disponíveis no projeto

Cada agente tem uma descrição que começa com "Use quando..." — é isso que aciona a delegação automática.

### 🏗️ Arquitetura

**`architect`** — `sonnet` — read-only + write em `docs/adr/`
- Use quando: Tomar decisões estruturais, quebrar dependências, avaliar trade-offs, criar/atualizar ADRs
- Exemplo: "Qual é a melhor forma de integrar um novo app de automação sem acoplamento?"

**`task-planner`** — `sonnet` — read-only
- Use quando: Começar uma feature nova ou refatoração; precisa de breakdown de subtarefas
- Exemplo: "Quebre a feature de MFA em subtarefas com dependências"

### 🎨 Frontend

**`frontend-developer`** — `sonnet` — full (read, write, edit, bash)
- Use quando: Criar/editar componentes, páginas, integração com API
- Exemplo: "Crie a tela de login com formulário validado via Zod e integração com /auth/login"

**`ui-reviewer`** — `haiku` — read-only
- Use quando: Revisar componentes/telas prontas buscando acessibilidade, responsividade, design system
- Exemplo: "Revise se esse componente está 100% responsivo e acessível (WCAG AA)"

### ⚙️ Backend

**`backend-developer`** — `sonnet` — full
- Use quando: Criar/editar endpoints, services, repositories, regras de negócio
- Exemplo: "Crie endpoint POST /v1/automations com validação Pydantic e logging estruturado"

**`database-reviewer`** — `haiku` — read-only
- Use quando: Revisar schemas, migrations, queries buscando performance, índices, integridade
- Exemplo: "Revise se essa migration de Alembic está segura para rodar em produção"

### 🔐 Segurança & Auth

**`auth-specialist`** — `opus` — full
- Use quando: Implementar/revisar login, cadastro, JWT, MFA, permissões
- Exemplo: "Implemente refresh token com 7 dias expirando corretamente"

**`security-auditor`** — `opus` — read-only
- Use quando: Fazer varredura de vulnerabilidades, OWASP Top 10, LGPD, secrets
- Exemplo: "Audite este diff antes de fazer PR: SQL injection, XSS, secrets hardcoded?"

### 🚀 Deploy & Infra

**`devops`** — `sonnet` — full
- Use quando: CDK, Lambda, API Gateway, pipelines CI/CD, variáveis de ambiente
- Exemplo: "Configure Lambda com reserved concurrency e timeout de 30s"

**`infra-reviewer`** — `haiku` — read-only
- Use quando: Revisar Dockerfiles, CDK, pipelines antes de deploy
- Exemplo: "Revise se o Dockerfile está otimizado para Lambda (tamanho < 50MB)"

### 🧪 Qualidade

**`test-writer`** — `sonnet` — full
- Use quando: Gerar testes unitários, integração, E2E após implementação
- Exemplo: "Crie testes em pytest para esse endpoint cobrir casos de sucesso, erro e rate limit"

**`code-reviewer`** — `haiku` — read-only
- Use quando: Revisar padrões, legibilidade, duplicação, complexidade
- Exemplo: "Revise se o código segue Ruff/MyPy (backend) e ESLint (frontend)"

### 📋 Documentação

**`doc-writer`** — `haiku` — read-only + write em `docs/` e READMEs
- Use quando: Gerar/atualizar README, CHANGELOG, OpenAPI, ADRs, guias
- Exemplo: "Atualize o README com instruções de setup local para a nova auth"

---

## Fluxo padrão de delegação

### Para features novas (ordem recomendada)

1. **`task-planner`** — quebra a tarefa em subtarefas, mapeia dependências front/back
2. **`architect`** — apenas se impactar arquitetura ou cruzar domínios
3. **`frontend-developer`** e/ou **`backend-developer`** — implementação em paralelo
4. **`auth-specialist`** — sempre que envolver login, sessão, JWT ou autorização
5. **`code-reviewer`** + **`security-auditor`** — revisão final obrigatória
6. **`test-writer`** — cobertura mínima 70% antes do PR
7. **`doc-writer`** — atualizar README, CHANGELOG, ADRs

### Antes de qualquer deploy

**Obrigatório:**
- ✅ **`security-auditor`** — varredura completa do diff (OWASP, secrets, LGPD)
- ✅ **`infra-reviewer`** — qualquer mudança em `/infra` ou Dockerfiles
- ✅ **`database-reviewer`** — obrigatório se houver migration na PR

**PR não pode ser mergeada sem aprovação destes.**

---

## Uso prático

### Delegação Automática (para tarefas pequenas)

O Claude lê as descrições dos agentes e delega automaticamente:

```
> Crie um endpoint GET /v1/users/:id com testes
```

O Claude provavelmente vai:
1. Chamar `task-planner` para quebrar
2. Chamar `backend-developer` para implementar
3. Chamar `test-writer` para gerar testes
4. Chamar `code-reviewer` para revisar

Sem você fazer nada.

### Invocação Explícita (para tarefas críticas)

Quando a delegação automática pode falhar ou você quer garantir o fluxo:

```
> Use task-planner para quebrar a feature de MFA.
> Depois auth-specialist para implementar.
> Depois security-auditor para revisar.
> Depois test-writer para testes.
```

Isso força a ordem exata.

### Modo Plan (para features complexas)

Antes de executar, peça o plano:

```
/plan
> Implemente autenticação com Google OAuth + MFA via TOTP
```

O Claude:
1. Chama `task-planner` e `architect`
2. Mostra um plano detalhado (sem executar ainda)
3. Você revisa e aprova
4. Aí sim executa orquestrando os agentes

---

## Dicas de ouro

### 1. Sempre comece com task-planner em features novas

O `task-planner` evita que você faça os mesmos passos 3x. Ele mapeia dependências, identifica riscos técnicos e quebrа a tarefa.

### 2. Use o CLAUDE.md como gospel

O arquivo `CLAUDE.md` na raiz do projeto é lido automaticamente em cada sessão. Atualize sempre que a orquestração dos agentes mudar. É o único lugar que documenta "como usamos agentes neste projeto".

### 3. Reviewers devem ser sempre read-only

Nunca dê `Write/Edit/Bash` para `*-reviewer`, `security-auditor` ou `code-reviewer`. Eles só observam e reportam. Se você deixar com permissão de escrita, eles podem "arrumar" coisas que você não planejava.

### 4. Modelos importam

- **`haiku`** para tarefas repetitivas/superficiais (lint, review básico, documentação)
- **`sonnet`** para implementação e balanceamento entre qualidade e custo
- **`opus`** para raciocínio profundo (auth, segurança, decisões arquiteturais)

### 5. Descrições são chaves

A descrição de um agente (aquela com "Use quando...") é **literalmente** o que aciona a delegação automática. Se for vaga, o Claude não sabe quando chamar. Se for específica, ele chama no momento certo.

---

## Troubleshooting

### "O Claude não chamou o agente que eu esperava"

**Causa:** Descrição muito vaga ou prompt ambíguo.

**Solução:**
1. Reescreva a descrição do agente para ser mais específica
2. Use invocação explícita: `> Use o [agente] para fazer isso`
3. Teste a delegação em um prompt simples antes de uma tarefa grande

### "O agente está demorando muito"

**Causa:** Contexto cheio ou tarefa muito grande para uma janela isolada.

**Solução:**
1. Verifique se o agente precisa de menos ferramentas (talvez remova `Write` se é só review)
2. Divida a tarefa em partes menores
3. Chame `task-planner` primeiro para quebrar

### "Subagente chamou outro subagente (nested calls)"

**Causa:** Isso não é permitido no Claude Code. Subagentes não podem chamar outros subagentes.

**Solução:** Você (agente principal) é responsável pela orquestração. Quebre manualmente:
```
> task-planner → (retorna plano)
> backend-developer → (implementa)
> security-auditor → (revisa)
```

### "O agente está ignorando o CLAUDE.md"

**Causa:** O arquivo precisa estar na **raiz do projeto** e nomeado exatamente `CLAUDE.md`.

**Solução:**
1. Confirme localização: `ls -la CLAUDE.md` (raiz do projeto)
2. Reinicie a sessão do Claude Code: `exit` e `claude` novamente
3. Verifique se o nome está exato: **CLAUDE.md** (case-sensitive)

### "Preciso de um agente customizado"

Você pode criar qualquer agente que quiser. Siga o template e coloque em:
- `~/.claude/agents/seu-agente.md` (pessoal, todos os projetos)
- `.claude/agents/seu-agente.md` (projeto, só neste projeto)

Registre no `CLAUDE.md` para documentar quando usá-lo.

---

## Próximos passos

1. **Crie os 13 agentes padrão** usando `/agents` no terminal ou criando os `.md` manualmente
2. **Atualize o `CLAUDE.md`** com a seção de agentes (você já tem o template)
3. **Comece com uma feature pequena** e invoque explicitamente: `> Use task-planner, depois backend-developer...`
4. **Observe o padrão** e refine as descrições dos agentes conforme aprende

---

## Referências

- **Documentação oficial:** https://docs.claude.com/en/docs/claude-code/overview
- **Seu CLAUDE.md:** Leia sempre antes de qualquer sessão longa
- **ADRs no projeto:** [`docs/adr/`](docs/adr/) para decisões arquiteturais

---

**Última atualização:** Abril 2026  
**Mantido por:** Você (ou o time)
