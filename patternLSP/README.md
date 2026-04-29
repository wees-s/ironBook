# patternLargeStructureProjects (LSP)

Guia para estruturar projetos de software médio/grande seguindo o modelo **AI FIRST**.

A proposta é simples: organizar o repositório, a documentação e os fluxos de trabalho de forma que **humano e IA tenham o mesmo nível de contexto**, sem depender de suposições.

---

## O que este documento faz

* Define uma **estrutura padrão de projeto**
* Estabelece **regras claras de documentação**
* Organiza o trabalho em **níveis rastreáveis (Fase → Story → Task → Subtask)**
* Introduz **gatilhos automáticos** para evitar bagunça
* Define o uso de **agentes especializados e memória persistente**

---

## Ideia central

* Documentação é a fonte da verdade
* Nada fica implícito
* Todo trabalho é rastreável
* Toda decisão relevante é registrada
* IA não “adivinha”, ela opera com contexto estruturado

---

## Componentes principais

* **README.md** → visão geral do projeto
* **CLAUDE.md** → regras invioláveis
* **<PROJECT>_CORE.md** → arquitetura (fonte da verdade)
* **DEV_PROMPT.md** → plano de execução
* **PROGRESS.md** → histórico do que foi feito

Além disso:

* `.claude/` → agentes, skills e memória
* `docs/` → segurança, operações e ADRs
* `.nextSteps/` → roadmap em rascunho

---

## Para quem é

* Projetos que vão crescer (ou já são grandes)
* Times que querem **padronização real**, não só convenção
* Quem quer integrar IA no fluxo de desenvolvimento de forma séria

---

## O que você ganha usando isso

* Menos retrabalho
* Menos “cadê o contexto?”
* Histórico completo do projeto
* Onboarding mais rápido
* Decisões rastreáveis

---

## Como usar

1. Use este documento como base
2. Siga a seção de **bootstrap (seção 12)**
3. Gere os documentos fundadores
4. Só depois comece a escrever código

---

## Em uma linha

Um padrão para construir projetos onde **nada depende de memória humana ou improviso** — tudo é explícito, rastreável e operável por IA.

---
