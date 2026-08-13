---
title: "Constituição para Agentes de IA & Skills"
author: "Luan Rodrigues"
date: "2026"
theme: "default"
duracao_alvo: "15–20 min"
---

<!--
Spec da apresentação. Vocabulário canônico e decisões em CONTEXT.md.
Estilo visual: reaproveitar o tema dark "Terminal Mono" de
ia-foundations-tools/apresentacao-2.html (JetBrains Mono, accent verde #50fa7b,
scroll-snap por slide, .reveal). 12 slides + 1 demo ao vivo.
Placeholders ‹...› = onde entram as linhas REAIS da constituição do apresentador
(que não está nesta máquina). Nada inventado deve passar por "regra real".
-->

# 1 · Título

# Constituição para Agentes de IA & Skills

Do consumidor ao configurador — o primeiro artefato durável que você escreve.

Luan Rodrigues · Fabricio Galvani · 2026

---

# 2 · Dor #1 — você já digitou isso 40 vezes

> *"Não adiciona comentário. Segue o estilo que já tá aí. Não mexe nos testes."*

- Você **reensina** seus padrões à IA **toda sessão**.
- A janela zera; o instinto do agente não.
- Toda vez do zero = tempo, token e paciência.

**A IA não lembra dos seus padrões — porque você nunca os escreveu num lugar que ela sempre lê.**

<!-- Objetivo: fazer a sala assentir. Cura essas dores = categorias 4 e 5. -->

---

# 3 · Dor #2 — ele fez algo que você nunca autorizou

- Reescreveu um teste pra "passar".
- **Data Engineering:** mudou a tipagem de um campo sem a mudança ter sido especificada, só pelo próprio julgamento.
- Refatorou meio módulo "de brinde".
- **MLOps:** mexeu no .pkl ou no transformer só pra passar nos testes unitários.

**Repetição é chato. Isto é perigoso.** Uma é *estilo*. A outra é *julgamento*.

<!-- Escalada. Cura essas dores = categorias 1, 2, 3. Prepara convenção vs constituição. -->

---

# 4 · O que governa o julgamento do agente? Nada.

```
Prompt da tarefa   → o que fazer AGORA (some no /clear)
CLAUDE.md / Rules  → como fazemos as coisas aqui (estilo)
Skills / MCP        → capacidade extra sob demanda
─────────────────────────────────────────────
Julgamento          → ??? (nada segura)
```

Nenhuma camada diz **o que nunca pode acontecer** nem **o que fazer na dúvida**.

**Esse é o buraco. A constituição preenche ele.**

---

# 5 · Constituição ≠ Convenção

| | **Convenção** (o grosso do CLAUDE.md) | **Constituição** (a camada de topo) |
|---|---|---|
| Responde | *como* fazemos aqui | *o que NUNCA pode* / o que fazer na dúvida |
| Exemplo | camelCase, usamos Postgres | nunca apagar prod sem confirmar |
| Governa | estilo | **julgamento** |
| Tamanho | pode ser grande | **poucas, altas, invioláveis** |

> Convenção diz como fazemos. Constituição diz o que nunca pode acontecer.

---

# 6 · Onde ela mora na pilha

```
┌─────────────────────────────────────────┐
│  CONSTITUIÇÃO  — princípios invioláveis  │  ← governa o julgamento
├─────────────────────────────────────────┤
│  Rules (CLAUDE.md) — convenções always-on│
│  Skills — expertise sob demanda          │
│  MCP — capacidade externa                │
└─────────────────────────────────────────┘
```

A constituição **cascateia**: ela molda como você escreve rules, skills e permissões de MCP.

<!-- Reaproveita o modelo mental "5 features" que a audiência já conhece. -->

---

# 7 · Anatomia — os 5 princípios que entram

1. **Limites de ação** — ‹nunca apagar prod / rodar migration / force-push sem confirmar›
2. **Honestidade** — ‹se não rodou o teste, diga; não invente API/arquivo›
3. **Escalonamento na incerteza** — ‹escopo ambíguo → pergunte, não adivinhe›
4. **Escopo & foco** — ‹faça só o que foi pedido; sem refatoração de brinde›
5. **Fidelidade ao existente** — ‹siga o estilo/estrutura que já está lá›

> ‹...› = placeholder. As linhas REAIS entram aqui depois.
> **1 e 2 e 3 curam a Dor #2. 4 e 5 curam a Dor #1.**

<!-- Simetria com slides 2-3: as dores viram os princípios. Deck fica inevitável. -->

---

# 8 · Ela é agnóstica de ferramenta

O **conceito** é o mesmo em todo lugar. Muda só o **arquivo canônico**.

| Ferramenta | Arquivo |
|---|---|
| Devin · Copilot · Cursor · Codex · Gemini CLI · Windsurf | **`AGENTS.md`** (padrão aberto) |
| Claude Code | **`CLAUDE.md` fino** que importa `@AGENTS.md` |

- `AGENTS.md`: padrão aberto (OpenAI ago/2025 → Linux Foundation dez/2025, 30+ agentes).
- **Iniciar no Claude:** `/init` gera o `CLAUDE.md`; `@AGENTS.md` na 1ª linha = fonte única.

---

# 9 · Da constituição às skills — o mesmo instinto

- **Constituição** = escrevi uma vez o que nunca muda.
- **Skill** = escrevi uma vez o procedimento que eu repito.

**Mesmo gesto:** parar de digitar de novo → escrever num lugar que a IA sempre respeita.

> Se você escreveu a constituição, você já sabe fazer sua primeira skill.

<!-- Ponte, não seção. A demo (slide 10) mostra skill na prática — não ensinamos aqui. -->

---

# 10 · DEMO AO VIVO — antes / depois

**Mesma tarefa, no meu repo real. Duas vezes.**

- **ANTES** (sem constituição) — *[pré-encenado]* → o agente ultrapassa: mexe em teste, muda estilo, precisa de babá.
- **DEPOIS** (constituição + skill) — *[ao vivo]* → respeita os guardrails e aplica meu procedimento sozinho.

> O delta é a prova. Você não vai *ouvir* que funciona — vai *ver*.

<!-- Repo específico decidido depois. Placement: penúltimo. CTA vem DEPOIS. -->

---

# 11 · Anti-padrões

❌ **Constituição inchada** — virou manual de 300 linhas. São 5–10 princípios, não um livro.

❌ **Misturar com convenção** — "usamos camelCase" não é princípio inviolável, é estilo.

❌ **Copiar da internet** — princípios são *seus*; regra genérica não governa julgamento nenhum.

❌ **Escrever e esquecer** — se a IA violou, o princípio estava fraco/ausente. Itere.

---

# 12 · Aplicação hoje à noite

1. **Escreva seus princípios COM a IA** — não da página em branco. Use a IA pra estruturar (as 5 categorias como guia). Ela organiza; **você** é a fonte. *(É o que você acabou de me ver fazer.)*
2. **Conecte** — Claude: `/init` + `@AGENTS.md`. Copilot/Cursor/Devin: já leem `AGENTS.md`.
3. **Ache a próxima repetição** — uma tarefa que você fez 3+ vezes esta semana = sua 1ª skill.

> **Uma constituição é a primeira coisa que você escreve quando para de repetir instruções pra IA e começa a governá-la — e é o mesmo instinto que vira sua primeira skill.**

Obrigado! · Q&A