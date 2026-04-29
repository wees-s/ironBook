# patternLargeStructureProjects (LSP)

> **O que é este documento.** Um guia genérico, replicável e completo para construir a **base de qualquer projeto de software médio/grande** sob a metodologia **"IA FIRST"** — onde a estrutura do repositório, a documentação, os agentes, as skills e os gatilhos são desenhados para que **um humano e uma IA possam operar o projeto em pé de igualdade**, com o mesmo nível de contexto.
>
> **Origem.** Destilado a partir do projeto interno PumP Core (HubPumP). As regras, formatos e fluxos descritos aqui foram **extraídos como padrões genéricos** — nenhuma menção a stack ou negócio específico permanece como obrigatória. Onde houver exemplo concreto, é apenas exemplo.
>
> **Como ler este documento (humano).** Leia da seção 0 até a seção 12 em ordem na primeira passagem. Depois consulte por tópico.
>
> **Como ler este documento (IA).** Trate este arquivo como um **prompt de inicialização de projeto**. Cada seção numerada é uma instrução acionável. Quando o usuário pedir "construa uma base de projeto seguindo o pattern LSP", siga as seções 1 → 12 na ordem, fazendo as perguntas guiadas da seção 11 quando faltar informação, e gerando os artefatos descritos.

---

## 0. Princípios Fundadores ("AI FIRST")

Toda escolha estrutural deste pattern é otimizada para **2 leitores simultâneos**: um humano e um agente de IA. Os princípios:

1. **Documentação é fonte da verdade arquitetural.** Não o código. Quando código e documento divergem, o documento manda — e o código é corrigido (ou o documento atualizado, com justificativa registrada).
2. **Contexto é hierárquico e explícito.** Todo subprojeto relevante carrega seu próprio `CLAUDE.md` (ou equivalente) com regras locais. Nada de "convenção implícita".
3. **Trabalho é fatiado em granularidade ágil rastreável.** `Epic → Story → Task → Subtask`. Toda Subtask vira uma entrada num diário de construção.
4. **Gatilhos automatizam disciplina.** Documentação, commits e branches **não são etapa final** — são disparados por eventos definidos. Se um gatilho não foi atendido, o trabalho não avança.
5. **Pausas obrigatórias substituem suposição.** Antes de qualquer passo que dependa de credencial, segredo, decisão arquitetural ou input externo, o agente **PARA** e pede via template.
6. **Agentes são especialistas com memória.** Subagentes têm escopo, modelo, ferramentas e memória persistente próprios. Nada de "agente faz-tudo".
7. **Reviewers são read-only por design.** Quem audita não escreve. Reduz risco e força clareza no relatório.
8. **Skills capturam aprendizado reusável.** Erro resolvido vira skill (ou entrada de skill). Nunca repetir o mesmo problema duas vezes.
9. **Sem funções fantasmas.** Toda função criada é 100% implementada na hora. Stubs, `TODO`, `NotImplementedError` são proibidos.
10. **Tudo é API.** Módulos não importam módulos cruzados — comunicam via contratos (HTTP, eventos, packages explícitos).

> **Observação para a IA leitora:** sempre que um destes princípios conflitar com outra instrução do usuário, o princípio que vence é o que o **usuário do projeto declarou** em seu `CLAUDE.md` raiz. Este pattern é um default forte, não uma lei.

---

## 1. Estrutura de Pastas Genérica

A raiz de qualquer projeto LSP tem **três camadas**: **documentação central**, **camada operacional de IA** (`.claude/`), e **código** (organizado como monorepo, polyrepo ou single-app — o pattern é agnóstico).

```
<root>/
│
├── README.md                        # Apresentação humana do projeto (negócio + técnico de alto nível)
├── CLAUDE.md                        # Regras invioláveis do projeto (índice diretivo)
├── <PROJECT>_CORE.md                # Mapa arquitetural — fonte da verdade da arquitetura
├── DEV_PROMPT.md                    # Plano de execução vivo (fases, stories, gatilhos)
├── PROGRESS.md                      # Diário cronológico de Subtasks concluídas
├── CHANGELOG.md                     # Histórico de releases (Keep a Changelog format)
├── FOLDER_STRUCTURE.md              # Snapshot atual da estrutura (atualizado quando muda)
├── .env.example                     # Template de variáveis (valores fictícios)
├── .gitignore
│
├── docs/
│   ├── adr/                         # Architecture Decision Records (numerados)
│   │   ├── _template.md
│   │   ├── README.md                # Índice dos ADRs
│   │   └── 0001-<slug>.md
│   ├── SECURITY.md                  # Política de segurança detalhada
│   ├── OPERATIONS.md                # Ambientes, observabilidade, backup, custos
│   └── onboarding.md
│
├── .claude/                         # Camada operacional de IA (versionada)
│   ├── agents/                      # Subagentes especialistas
│   │   ├── task-planner.md
│   │   ├── code-reviewer.md
│   │   └── ...
│   ├── skills/                      # Skills reusáveis (gatilhos automáticos/manuais)
│   │   └── <skill-name>/
│   │       └── SKILL.md
│   ├── agent-memory/                # Memória persistente por agente
│   │   └── <agent-name>/
│   │       ├── MEMORY.md            # Índice de memórias
│   │       └── *.md                 # Memórias individuais
│   └── settings.local.json          # Configurações locais (não versionar segredos)
│
├── .nextSteps/                      # Guias e refactors planejados (rascunhos de roadmap)
│   └── NN-<slug>.md
│
└── <code>/                          # Código do projeto (apps/, packages/, infra/, src/, etc.)
```

**Decisões a tomar ao instanciar:**

- **Monorepo vs polyrepo:** se houver compartilhamento real (UI, auth, schema), monorepo. Caso contrário, polyrepo com submódulo `.claude/` em cada repo.
- **Layout de `<code>/`:** `apps/<nome>/{backend,frontend}` + `packages/<nome>` + `infra/` é o default sugerido para sistemas com múltiplos serviços. Para projetos single-app, basta `src/` + `tests/`.

---

## 2. Os Quatro Documentos Fundadores

Esta é a **espinha dorsal documental** do pattern. Sem estes quatro arquivos, o projeto não está "AI FIRST".

### 2.1. `README.md` — Apresentação humana

**Audiência:** qualquer pessoa (técnica ou não) que abra o repositório.

**Estrutura sugerida:**

1. Nome + 1 frase do que é.
2. **O que é o projeto** em 2-3 parágrafos (linguagem acessível).
3. **Para quem é** (tabela papel × como usa).
4. **Objetivos** (3-5 bullets).
5. **Funcionalidades** (em construção / planejadas).
6. **Como o projeto está organizado** (alto nível, sem detalhe técnico).
7. **Tecnologias principais** em **linguagem simples** (tabela: O quê / Escolha / Por quê).
8. **Segurança em poucas linhas.**
9. **Status do projeto** (tabela de fases).
10. **Setup local** (comandos).
11. **Documentos para aprofundar** (links para os outros 3 fundadores e docs/).
12. **Em uma frase** (resumo final).

**Regra:** o README **nunca** é a fonte da verdade arquitetural. Ele aponta para `<PROJECT>_CORE.md`.

### 2.2. `CLAUDE.md` — Regras invioláveis (índice diretivo)

**Audiência:** IA (lido com prioridade máxima) + dev sênior buscando regras.

**Princípio:** este arquivo é um **índice diretivo**. Regras invioláveis ficam aqui em forma curta; detalhes ficam nos documentos referenciados (`docs/SECURITY.md`, `docs/OPERATIONS.md`, ADRs).

**Seções obrigatórias:**

1. Cabeçalho explicando que este arquivo deve ser lido **antes** de qualquer outro.
2. **Documentos relacionados** (tabela: Documento / Conteúdo).
3. **Arquitetura — regras invioláveis** (bullets curtos, sem prosa).
4. **Ambientes** (dev/staging/prod ou equivalente).
5. **Git Workflow** (branches, PRs, merge strategy, conventional commits).
6. **Stack tecnológico** (1 linha por camada).
7. **Padrões de código** (lint/format/types por linguagem).
8. **Testes** (tipos obrigatórios, cobertura mínima com regra qualitativa).
9. **Segurança (resumo)** — referenciar `docs/SECURITY.md` para detalhes.
10. **Observabilidade (resumo)** — referenciar `docs/OPERATIONS.md`.
11. **Backup e DR (resumo).**
12. **Limites padrão** (timeouts, payloads, etc.).
13. **Performance** (paginação, latências-alvo).
14. **Documentação** (que documentos existem e quando atualizar cada um).
15. **Adendo de segurança** (proibição de commit de segredos, gitleaks no pre-commit e CI).
16. **Agentes (subagentes)** — tabela completa: Agente / Tipo / Modelo / Função.
17. **Fluxos padrão de trabalho** (delegação por tipo de mudança).
18. **Regras de uso dos agentes** (read-only para reviewers, opus para crítico, haiku para repetitivo).
19. **Convenção de invocação** (delegação automática vs explícita).

**Regra de manutenção:** atualizar este arquivo **sempre** que uma regra inviolável mudar. Mudança de regra exige ADR.

### 2.3. `<PROJECT>_CORE.md` — Mapa arquitetural

**Audiência:** dev (humano ou IA) que precisa entender o sistema antes de codar.

**Princípio:** **fonte da verdade arquitetural.** Se algo conflitar com `DEV_PROMPT.md`, este vence.

**Seções obrigatórias:**

1. **Visão Geral** (o que é o sistema, em 1 página).
2. **Contexto de Negócio** (tabela: empresa, usuários, stakeholders, escala).
3. **Princípios Arquiteturais** (5-10 bullets, herdados do `CLAUDE.md`).
4. **Stack Tecnológico** (tabela completa Camada × Tecnologia × Observação).
5. **Estrutura de Pastas** (árvore comentada com **decisões de estrutura — explicação**).
6. **Estratégia de Deploy / Independência** (como deploys não se atrapalham, roteamento, versionamento de pacotes compartilhados, cuidados com banco compartilhado).
7. **Fluxo de Autenticação / Sessão / Domínio Principal** (diagrama ASCII ou mermaid).
8. **Setup Local** (esboço — detalhe vai no README).
9. **Plano de Execução — Próximos Passos** (alto nível: Fases).
10. **Sugestões Proativas** (coisas que valem que ainda não estão no plano — fica como semente para ADRs futuros).
11. **Glossário** (termos da stack que aparecem muito).
12. **O Que Está Fora do Escopo** (com justificativa por item — evita re-discussão).
13. **Referências Cruzadas.**

### 2.4. `DEV_PROMPT.md` — Plano de execução vivo

**Audiência:** IA executando o trabalho, dev acompanhando.

**Princípio:** roteiro **fase a fase** com checklists, gatilhos e templates de pausa. **Vivo** — atualizado a cada Story concluída.

**Seções obrigatórias:**

1. **REGRA — Documentos de Leitura Obrigatória** (ordem: PROGRESS → CLAUDE → CORE → este).
2. **REGRA PRINCIPAL — Pausa Obrigatória** (antes de qualquer credencial / chave / acesso externo).
3. **REGRA — Documentação Contínua (Gatilho Automático)** (Subtask/Task/Story/Fase → o que atualizar onde).
4. **REGRA — Gatilhos de Git** (branch / commit / push / PR / especiais).
5. **REGRA — Granularidade Ágil** (Epic → Story → Task → Subtask).
6. **REGRA — Sem Funções Fantasmas.**
7. **Estado Atual do Projeto** (tabela: Fase × Acesso necessário × Status).
8. **Fase 0 / Coleta de Informações Iniciais** (rodando em paralelo, não bloqueia código).
9. **Fase 1 — Fundação Offline** (tudo que pode ser feito sem credenciais externas).
10. **Fase 2..N — Fases sequenciais** com Stories numeradas (`X.Y`), cada uma com checklist `[ ]`.
11. **Apêndice — Coleta Guiada de Informações Externas** (templates para credenciais, secrets, domínios, APIs de terceiros).
12. **Referências.**

> **Regra de coerência:** se este arquivo conflitar com `<PROJECT>_CORE.md`, **pare e pergunte**. O CORE é a fonte da verdade arquitetural; este é o roteiro de execução. Roteiro nunca redefine arquitetura.

---

## 3. `PROGRESS.md` — Diário de Construção

**Propósito:** registro cronológico de tudo que foi feito. **Fonte primária para o agente se reorientar** ao retomar o trabalho em uma nova sessão.

**Gatilho de escrita:** ao finalizar **qualquer Task ou Subtask**, adicionar **bloco novo no topo do log** (entradas mais recentes primeiro).

**Regras:**

- **Não editar entradas antigas** (apenas correção factual com nota `[correção YYYY-MM-DD]`). O histórico é imutável.
- **Uma entrada por Subtask** — não agrupar.
- Sempre incluir hash do commit quando aplicável (vincula log ↔ diff real). Sem código gerado: `Commit: N/A`.
- Pausas para validação humana viram entrada (`Tipo: Pausa`).

**Formato de cada entrada:**

```markdown
### YYYY-MM-DD HH:MM — [Fase X.Y.Z] Título curto da Task/Subtask
- **Tipo:** Subtask | Task | Story | Fase | Pausa
- **Status:** ✅ Concluída | ⚠️ Parcial (motivo) | 🛑 Bloqueada (motivo)
- **O que foi feito:** descrição objetiva (1–3 linhas; mais quando a Subtask for densa)
- **Arquivos tocados:** `path/arquivo1`, `path/arquivo2`
- **Commit:** `<hash curto>` — `<mensagem conventional commit>`
- **Branch:** `feat/...` (quando aplicável)
- **Próximo passo:** o que deve ser feito a seguir
- **Bloqueios / Pendências:** info externa aguardada, decisão pendente, etc.
```

**Topo do arquivo — "Estado Resumido":** tabela curta atualizada junto com a entrada mais recente — Fase atual / Story atual / Última Subtask / Próxima ação / Bloqueios ativos.

---

## 4. Granularidade Ágil — Epic → Story → Task → Subtask

| Nível | Equivalente | Onde fica | Quem executa |
|---|---|---|---|
| **Fase** | Epic | `DEV_PROMPT.md` (seção da Fase) | Humano valida entrada/saída |
| **Story** | User Story | Item numerado `X.Y` da Fase | Agente executa, humano valida ao final |
| **Task** | Task | Checkbox `[ ]` da Story | Agente executa |
| **Subtask** | Subtask | Quebra mental da Task | Agente executa, registra em PROGRESS |

**Regras de quebra:**

- Story > ~2h efetivas ou > 3 arquivos → **quebrar em Tasks** explícitas.
- Task com mais de 1 passo lógico (criar + configurar + testar) → **quebrar em Subtasks** antes de iniciar.
- **Pausa entre Tasks** para reportar e atualizar documentação.
- **Pausa obrigatória entre Stories** para validação humana.

**Quando NÃO formalizar:** Stories com checklist curto e escopo isolado podem ser executadas direto sem Tasks/Subtasks por escrito — mas a **pausa de documentação ao final continua obrigatória**.

---

## 5. Sistema de Gatilhos Automáticos

Os gatilhos são o coração da disciplina do pattern. **Eventos disparam ações obrigatórias.**

### 5.1. Gatilhos de Documentação

| Evento | Ação obrigatória |
|---|---|
| ✅ Subtask concluída | Entrada nova no topo do `PROGRESS.md` + marcar `[x]` em `DEV_PROMPT.md` + linha em `CHANGELOG.md` (sob `## [Unreleased]`) |
| ✅ Task concluída | Entrada em PROGRESS + revisar `Estado Atual` em `DEV_PROMPT.md` se mudou status |
| ✅ Story/Fase concluída | Entrada em PROGRESS (Tipo: Story/Fase) + atualizar tabela `Estado Atual` + criar/atualizar **ADR** se houve decisão estrutural + atualizar `FOLDER_STRUCTURE.md` se estrutura mudou |
| ✅ Pausa atendida | Registrar **o que foi recebido** (sem valores sensíveis) na seção correspondente |
| ⚠️ Bloqueio identificado | Adicionar nota `> ⚠️ BLOQUEIO:` na seção da task, com data e motivo |

> **Antes de iniciar a próxima Subtask, confirme:** "entrada no `PROGRESS.md` foi escrita?". Se não, escreva antes de avançar.

### 5.2. Gatilhos de Git

**Branch:**

- 🌱 Antes de iniciar uma Story → criar branch a partir da branch base (ex.: `develop`):
  - Feature: `feat/<fase>-<slug>`
  - Correção: `fix/<slug>`
  - Infra: `chore/<slug>`
  - Docs: `docs/<slug>`
- 🚫 Nunca commitar direto em `main` ou `develop` — sempre via PR.

**Commit (Conventional Commits obrigatório):**

- 💾 A cada Subtask concluída → commit atômico no formato `<tipo>(<escopo>): <descrição curta>`
  - Tipos: `feat`, `fix`, `chore`, `docs`, `refactor`, `test`, `build`, `ci`, `perf`
- 💾 Toda atualização de doc no mesmo gatilho → commit `docs(<escopo>): ...` no mesmo push
- 🛑 Antes de commitar: rodar lint/format/type-check/secret-scan (já cobertos por pre-commit, confirmar que rodaram)
- 🚫 Nunca usar `--no-verify` — se hook bloqueou, há problema real

**Push:**

- ⬆️ Ao concluir uma Task → `git push` (backup + review parcial)
- ⬆️ Story concluída → push final + abertura de PR

**PR:**

- 📬 Story concluída → PR `feat/...` → branch base com:
  - Título Conventional Commit
  - Descrição: o que mudou, por quê, como testar, screenshots se UI
  - Link para a Story no `DEV_PROMPT.md`
  - Checklist: testes passando, doc atualizada, agentes de review acionados
- 📬 Fase concluída → após Stories mergeadas, PR para branch de produção + tag semver

**Especiais:**

- 🚨 Detecção de secret no diff → abortar commit, rotacionar, registrar em `SECURITY.md`
- 🚨 Conflito ao rebasear → pausar e pedir orientação humana
- 🚨 Migração destrutiva → snapshot manual antes do merge

> **Sem commit em dia, nenhuma nova Subtask começa.** Trabalho não commitado é trabalho perdido.

### 5.3. Gatilho de Pausa

Antes de qualquer passo que dependa de:

- Credenciais (cloud, banco, API)
- Chaves / secrets
- Domínios / DNS
- IDs externos
- Decisão arquitetural ainda não tomada
- Dados de usuário ainda não fornecidos

→ **PARE.** Use o template apropriado do **Apêndice de Coleta Guiada** (seção 11). Nunca assuma valores. Nunca pule a coleta. Se a informação não vier, o próximo passo não começa.

---

## 6. Camada `.claude/` — Agentes, Skills, Memória

### 6.1. Subagentes (`.claude/agents/<name>.md`)

Cada subagente é um arquivo Markdown com **frontmatter YAML** + corpo de instruções.

**Frontmatter padrão:**

```yaml
---
name: "nome-do-agente"
description: "Use this agent when... (3-5 frases descritivas + 2-3 exemplos com tags <example> e <commentary>)"
model: opus | sonnet | haiku
memory: project
tools: "Read, Grep, Glob, ..."   # Omitir = todas as ferramentas
---
```

**Regras de design:**

- **`description` deve começar com "Use when..."** (ou "Use this agent when...") — é o que dispara delegação automática.
- **Modelos por criticidade:**
  - `opus` — auth, segurança, decisões arquiteturais (raciocínio profundo)
  - `sonnet` — desenvolvimento normal, planejamento, doc-writing
  - `haiku` — reviews, lint, tarefas repetitivas bem definidas (custo ↓)
- **Reviewers e auditors são SEMPRE read-only.** Nunca dar `Write/Edit/Bash` para `*-reviewer` ou `security-auditor`.
- **Subagentes não chamam outros subagentes.** Orquestração é hierárquica, sempre a partir do agente principal.
- **`memory: project`** habilita memória persistente versionada (compartilhada com a equipe).

**Estrutura do corpo (template):**

```markdown
Você é um <papel> especialista em <domínio>.

## Contexto do Desenvolvedor
<adapte tom ao perfil — sênior, júnior, etc.>

## Sua Missão
<o que faz e o que NÃO faz>

## Metodologia
### 1. <Etapa>
- ...

## Sistema de Prioridades
🔴 CRÍTICO / 🟡 IMPORTANTE / 🟢 SUGESTÃO

## Formato de Saída
<estrutura markdown padronizada>

## Princípios de Conduta
- Idioma, tom, especificidade, justificativa

## Auto-verificação
- [ ] <checks antes de finalizar>

## Atualização de Memória
<o que registrar na memória persistente>

# Persistent Agent Memory
<bloco padrão sobre como ler/escrever memórias>
```

**Conjunto de agentes recomendado (genérico):**

| Agente | Tipo | Modelo | Função |
|---|---|---|---|
| `task-planner` | read-only | sonnet/opus | Quebra features em Subtasks, mapeia dependências e riscos |
| `architect` | read-only + write em `docs/adr/` | sonnet | Decisões estruturais, ADRs |
| `backend-developer` | full | sonnet | Implementação de backend |
| `frontend-developer` | full | sonnet | Implementação de frontend |
| `auth-specialist` | full | opus | Tudo de auth/sessão/JWT/permissões |
| `database-reviewer` | read-only | haiku | Schemas, migrations, queries |
| `security-auditor` | read-only | opus | OWASP, secrets, compliance |
| `infra-reviewer` | read-only | haiku | IaC, Dockerfiles, pipelines |
| `code-reviewer` | read-only | haiku | Lint, padrões, complexidade, naming |
| `ui-reviewer` | read-only | haiku | Acessibilidade, responsividade, design system |
| `test-writer` | full | sonnet | Testes unitários, integração, E2E |
| `doc-writer` | read-only + write em `docs/` e READMEs | haiku/sonnet | README, CHANGELOG, ADRs, OpenAPI |

**Fluxo padrão para feature nova:**

```
task-planner → [architect se cruza domínios] → backend/frontend-developer
            → [auth-specialist se toca auth] → code-reviewer + security-auditor
            → test-writer → doc-writer
```

**Fluxo obrigatório antes de deploy:**

```
security-auditor (varredura do diff)
+ infra-reviewer (se mudou /infra ou Dockerfile)
+ database-reviewer (se há migration)
→ sem aprovação destes, PR não merge
```

### 6.2. Skills (`.claude/skills/<name>/SKILL.md`)

Skills capturam **conhecimento reusável com gatilho** (automático ou manual).

**Frontmatter:**

```yaml
---
name: nome-da-skill
description: <quando usar — começar com situação/intenção>. Trigger AUTOMÁTICO <quando>. Trigger MANUAL via "<frases>".
---
```

**Estrutura do corpo:**

1. **Princípio** (1 frase).
2. **Quando acionar** (trigger automático + trigger manual).
3. **Estrutura de dados** que a skill mantém (se houver).
4. **Formato de cada entrada** (template fixo).
5. **Fluxo de uso** (consultar / registrar / reincidir).
6. **Regras de qualidade.**
7. **Limites.**
8. **Manutenção.**

**Skill canônica recomendada — `error-memory`:** memória persistente de erros e soluções. Após **2 falhas consecutivas na mesma operação**, o agente PARA e consulta antes de tentar de novo. Salva categorias (`build/`, `runtime/`, `deploy/`, `dependency/`, `config/`, `git/`, `database/`, `network/`, `auth/`, `other/`) com `INDEX.md` navegável.

**Outras skills úteis:** `using-superpowers`, `brainstorming`, `writing-plans`, `executing-plans`, `verification-before-completion`, `systematic-debugging` — adapte conforme cultura do projeto.

### 6.3. Memória de Agente (`.claude/agent-memory/<agent>/`)

Cada agente que tem `memory: project` ganha um diretório próprio com:

- `MEMORY.md` — **índice puro**, uma linha por entrada (≤150 chars cada, ≤200 linhas total).
- Arquivos `.md` individuais com frontmatter `name / description / type` e corpo da memória.

**Tipos de memória:**

- `user` — sobre o desenvolvedor (papel, preferências, conhecimento).
- `feedback` — correções E confirmações do usuário (lead com regra + **Why** + **How to apply**).
- `project` — fatos sobre trabalho em andamento (datas absolutas, motivação).
- `reference` — pointers para sistemas externos (Linear, Slack, dashboards).

**O que NÃO salvar:** padrões de código, convenções, paths, git history, debugging recipes, conteúdo já em CLAUDE.md, estado efêmero da conversa.

---

## 7. ADRs — Architecture Decision Records

Toda decisão arquitetural relevante vira um ADR em `docs/adr/NNNN-<slug>.md`.

**Template (`docs/adr/_template.md`):**

```markdown
# ADR-NNNN: <Título da decisão>

- **Status:** Proposed | Accepted | Deprecated | Superseded by ADR-XXXX
- **Data:** YYYY-MM-DD
- **Autor:** <nome>

## Contexto
<situação que motivou a decisão; restrições; perguntas em aberto>

## Decisão
<o que foi decidido — em uma frase + parágrafo curto>

## Alternativas Consideradas
### <Alternativa 1>
- **Prós:** ...
- **Contras:** ...
### <Alternativa 2>
...

## Consequências
- **Positivas:** ...
- **Negativas / Tradeoffs:** ...
- **Mitigações:** ...

## Referências
- Links, RFCs, threads de discussão
```

**Regras:**

- Numeração sequencial (`0001`, `0002`, ...).
- ADR Aceito **não é editado** — se mudar, criar novo ADR com status `Supersedes ADR-XXXX` e atualizar o antigo para `Deprecated — superseded by ADR-YYYY`.
- `docs/adr/README.md` mantém o índice (status × título × data).

---

## 8. Documentos de Suporte (`docs/`)

### 8.1. `docs/SECURITY.md`

Conteúdo: política completa de autenticação, criptografia, secrets management, rate limiting, CORS, network isolation, compliance (LGPD/GDPR/SOC2 conforme aplicável), resposta a incidentes (rotação de chaves, comunicação, postmortem).

### 8.2. `docs/OPERATIONS.md`

Conteúdo: ambientes (dev/staging/prod), observabilidade (logs estruturados, métricas, alertas), backup e DR (RPO/RTO, snapshots, teste de restore), runbooks (cenários comuns), custos (orçamento mensal, alertas de billing).

### 8.3. `docs/onboarding.md`

Conteúdo: setup do ambiente local, acessos, primeiras leituras (ordem dos 4 fundadores), primeiros passos práticos.

---

## 9. `.nextSteps/` — Roadmap em rascunho

Diretório livre para guias de iniciativas planejadas mas ainda não em execução. Cada arquivo é numerado (`01-<slug>.md`, `02-<slug>.md`).

Quando uma iniciativa entra em execução:

1. Conteúdo migra para Stories no `DEV_PROMPT.md`.
2. Arquivo é movido para `.nextSteps/done/` ou deletado.

**Bom para:** "como adicionar app novo", "refactor planejado", "funcionalidades a replicar de sistemas externos".

---

## 10. Setup Local Padrão

Independente de stack, todo projeto LSP entrega:

1. **Pré-requisitos enumerados** no README (versões exatas).
2. **`.env.example`** completo com valores fictícios — usuário copia para `.env` e preenche.
3. **Comando único de bootstrap** quando possível (`make setup`, `pnpm bootstrap`, `./scripts/setup.sh`).
4. **Banco / serviços externos via Docker Compose** quando aplicável.
5. **Comandos de "rodar tudo localmente"** (`make dev`, `pnpm dev`, etc.).
6. **Validação pós-setup** (1 comando que confirma tudo OK — ex.: `make doctor`).

---

## 11. Apêndice — Coleta Guiada (Templates de Pausa)

Sempre que uma situação de pausa ocorrer (seção 5.3), a IA **deve** usar um destes templates. Adicione novos conforme o projeto demandar.

### Template — Credenciais de Cloud

```
Preciso das seguintes informações de [AWS/GCP/Azure] antes de continuar:

1. ID da conta de [staging/produção]: __________
2. Região padrão: __________
3. Perfil CLI configurado localmente (nome do perfil): __________

Importante: nunca envie chaves de acesso (access key / secret) pelo chat.
Configure via `<provider> configure` ou SSO na sua máquina.
```

### Template — Secret / Chave de API

```
A próxima etapa exige um(a) [nome do secret/chave].

1. Você já tem? [ ] Sim — confirme sem revelar o valor / [ ] Não — posso guiar a criação
2. Onde será armazenado: <Secrets Manager / Vault / equivalente> — nunca em .env versionado.

Não envie o valor pelo chat. Confirme apenas que está disponível e configurado.
```

### Template — Domínio e DNS

```
Preciso de informações sobre o domínio antes de configurar TLS/CDN:

1. Domínio: __________
2. DNS gerenciado em: [ ] Route 53  [ ] Cloudflare  [ ] Outro: __________
3. Você tem acesso para criar registros DNS? [ ] Sim  [ ] Não
4. Certificado SSL já existe? [ ] Sim (envie ARN/ID)  [ ] Não (posso guiar a criação)
```

### Template — Serviço Externo / API de Terceiro

```
O próximo passo integra com [nome do serviço].

1. Você já tem conta/acesso? [ ] Sim  [ ] Não
2. Tem documentação ou link do serviço? __________
3. Tem chave de API disponível?
   [ ] Sim — vou armazenar no <secret store> (não envie pelo chat)
   [ ] Não — precisa criar antes de continuar
```

### Template — Decisão Arquitetural Pendente

```
Antes de avançar, preciso de uma decisão sobre [tópico].

Opções identificadas:
- Opção A: [descrição] — Prós: [...] / Contras: [...]
- Opção B: [descrição] — Prós: [...] / Contras: [...]

Recomendação: <recomendação + razão>.

A decisão deve virar ADR-NNNN. Confirma a opção e abro o ADR antes de codar?
```

---

## 12. Receita de Bootstrap — Como Instanciar Este Pattern em um Projeto Novo

> **Para a IA leitora:** quando o usuário disser "construa uma base de projeto seguindo o pattern LSP", siga esta sequência **literalmente**, fazendo as perguntas via templates da seção 11 quando faltar informação.

### Passo 0 — Coleta inicial (humano responde)

Pergunte ao humano (use formulário enxuto, não interrogatório):

1. **Nome do projeto** + 1 frase do que é + 1 frase de para quem.
2. **Stack pretendida** (linguagens / frameworks / cloud / banco). Aceite "ainda não sei" — ofereça 2-3 opções típicas.
3. **Escala estimada** (usuários, requisições/dia, dados).
4. **Escopo de início:** `monorepo` (vários apps/packages) ou `single-app`?
5. **Quais ambientes** terão (dev/staging/prod ou outro)?
6. **Quem é o time** (solo / pequeno / grande) e nível técnico predominante.
7. **Compliance aplicável** (LGPD, GDPR, HIPAA, PCI, SOC2, nenhum).
8. **Idioma da documentação** (PT-BR, EN, outro).
9. **Repositório já existe?** Branch principal? Convenção de commits?
10. **Restrições conhecidas** (custo, deadline, dependências externas).

### Passo 1 — Esqueleto de pastas

Crie a árvore da seção 1, vazia ou com placeholders. **Não crie arquivos de código ainda** — apenas os documentais e a camada `.claude/`.

### Passo 2 — Documentos fundadores (na ordem)

Gere, **um por vez, com pausa para revisão humana entre cada um**:

1. `README.md` (seção 2.1).
2. `CLAUDE.md` (seção 2.2). **Comece pelas regras invioláveis** — não adivinhe; pergunte.
3. `<PROJECT>_CORE.md` (seção 2.3). Inclua diagrama ASCII da arquitetura.
4. `DEV_PROMPT.md` (seção 2.4). **Liste as Fases 1..N** com Stories já fatiadas.
5. `PROGRESS.md` (seção 3) — **vazio**, apenas o cabeçalho e o "Estado Resumido".
6. `CHANGELOG.md` — apenas `## [Unreleased]`.
7. `.env.example` — variáveis nomeadas com valores fictícios + comentário do que cada uma faz.
8. `.gitignore` adequado à stack.

> **Pausa obrigatória após cada um.** Validação humana antes de avançar.

### Passo 3 — `docs/`

1. `docs/adr/_template.md` (seção 7).
2. `docs/adr/README.md` (índice vazio).
3. `docs/adr/0001-<primeira-decisao-estrutural>.md` — primeira decisão real do projeto (geralmente "monorepo vs polyrepo" ou "stack X vs Y").
4. `docs/SECURITY.md` (seção 8.1) — esqueleto preenchido com defaults sensatos para a stack escolhida.
5. `docs/OPERATIONS.md` (seção 8.2) — esqueleto.
6. `docs/onboarding.md` (seção 8.3) — referência aos 4 fundadores.

### Passo 4 — Camada `.claude/`

1. `.claude/agents/` — gere os agentes da tabela da seção 6.1, **adaptados à stack do projeto** (ex.: `frontend-developer` com instruções de React/Tailwind se for o caso).
2. `.claude/skills/error-memory/SKILL.md` (seção 6.2).
3. `.claude/agent-memory/<cada-agente>/MEMORY.md` (vazio, apenas cabeçalho).
4. `.claude/settings.local.json` (vazio ou com defaults seguros — sem segredos).

### Passo 5 — Esqueleto de código

Só agora — e ainda como **estrutura vazia**:

1. Pastas de `apps/`, `packages/`, `infra/`, `src/`, `tests/` conforme decidido.
2. `_template/` para apps/módulos novos quando aplicável.
3. Arquivos de configuração de lint, format, type-check, pre-commit hooks.
4. `Dockerfile` / `docker-compose.yml` se aplicável.
5. Workflow CI mínimo (`.github/workflows/ci.yml` ou equivalente) — lint + testes em todo PR.

### Passo 6 — Validação final

Antes de declarar "pronto":

- [ ] Os 4 fundadores estão coerentes entre si (nenhum conflito)?
- [ ] `CLAUDE.md` referencia todos os outros docs?
- [ ] `DEV_PROMPT.md` tem **Fase 1** executável **sem credenciais externas**?
- [ ] Cada agente tem frontmatter completo e descrição com "Use when..."?
- [ ] `error-memory` skill instalada?
- [ ] ADR-0001 registrado?
- [ ] `.gitignore` cobre `.env`, build artifacts, caches?
- [ ] Pre-commit hooks de lint + secret-scan configurados?
- [ ] PROGRESS.md e CHANGELOG.md prontos para receber a primeira entrada?

### Passo 7 — Primeiro commit

Mensagem sugerida (Conventional Commits):

```
chore(repo): bootstrap inicial do projeto seguindo pattern LSP

- Estrutura de pastas (apps, packages, infra, docs, .claude)
- Documentos fundadores (README, CLAUDE, <PROJECT>_CORE, DEV_PROMPT)
- ADR-0001 com primeira decisão estrutural
- 13 subagentes especialistas configurados
- Skill error-memory instalada
- Pre-commit + CI mínimo
```

E **primeira entrada em PROGRESS.md** registrando este bootstrap como `Tipo: Fase` (Fase 0 — Bootstrap).

---

## 13. Anti-padrões — O Que Não Fazer

- ❌ **Documentação "para depois".** Toda Subtask que não atualiza PROGRESS é Subtask perdida.
- ❌ **Funções fantasmas** (`pass`, `TODO`, `NotImplementedError`). Pause e pergunte se não dá pra implementar agora.
- ❌ **Reviewer com permissão de escrita.** Quebra o princípio.
- ❌ **Subagente chamando subagente.** Orquestração é hierárquica.
- ❌ **Decidir arquitetura no `DEV_PROMPT.md`.** Decisão arquitetural mora em `<PROJECT>_CORE.md` + ADR.
- ❌ **`CLAUDE.md` com prosa longa.** É índice diretivo. Detalhe vai pros documentos referenciados.
- ❌ **Editar entrada antiga de PROGRESS.** Histórico é imutável.
- ❌ **Pular pausa por pressa.** A pausa **é** o trabalho.
- ❌ **Commit sem Conventional Commits.**
- ❌ **`--no-verify`** para passar do hook. O hook está certo até prova em contrário.
- ❌ **Segredo em arquivo versionado.** Nunca. `.env.example` com valor fictício é o caminho.
- ❌ **Subagente sem `description` começando com "Use when..."**. Quebra a delegação automática.
- ❌ **Skill sem trigger claro.** Skill que ninguém invoca é skill morta.
- ❌ **Múltiplos `_CORE.md` ou múltiplas fontes da verdade.** Um por projeto. Subprojetos têm `CLAUDE.md` próprio, não outro CORE.

---

## 14. Manutenção do Pattern

Este documento é vivo. Quando aprender algo novo construindo um projeto LSP que mereça ser default:

1. Edite a seção correspondente.
2. Registre no rodapé (changelog interno):
   ```
   ## Histórico de mudanças
   - YYYY-MM-DD — <mudança> — Por quê: <razão>
   ```
3. Se a mudança for grande (novo princípio, novo agente padrão, mudança de fluxo), considere versionar como `patternLargeStructureProjects-vN.md` e referenciar a versão anterior.

---

## 15. Em Uma Frase

**LSP é um pattern AI FIRST onde documentação, granularidade ágil, gatilhos automáticos, agentes especialistas e memória persistente formam a base operacional de qualquer projeto — feita para um humano e uma IA trabalharem com o mesmo contexto, sem suposições, sem improviso e sem perda de histórico.**

---

## Histórico de mudanças

- 2026-04-29 — Versão inicial extraída do PumP Core.
