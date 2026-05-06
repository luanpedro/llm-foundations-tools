---
title: "Introdução a AI Coding Tools"
author: "Luan Rodrigues"
date: "2026"
theme: "default"
---

# Introdução a AI Coding Tools

Foco: Fundamentos · Context Engineering · Long-term Memory · Anti-padrões

---

## Agenda

1. Fundamentos
2. Context Engineering ⭐
3. Long-term Memory ⭐
4. Anti-padrões
5. Q&A

---

# 1. Fundamentos

---

## O que é (e o que NÃO é) IA

> *"AI não é nem inteligente nem artificial — é engenharia de software em cima de cálculo probabilístico"*

- **LLM é stateless** — Transformer probabilístico que recebe tokens, devolve o próximo
- **Memória é da ferramenta**, não da LLM
- **Token = unidade de cobrança** (preço por milhão)
- **Mercado é realidade:** 90% do código gerado por IA já é prática comum

---

## Mudança de perfil do dev

| Antes | Agora |
|-------|-------|
| Codificador braçal | Analista de negócio + arquiteto |
| Lê código linha-a-linha | Escreve specs e revisa com IA |
| Decide tudo manualmente | Orquestra agentes |

**"IA é o anabolizante da programação"**

---

# 2. Context Engineering ⭐

---

## Por que isso importa

A janela de contexto é o **único canal** por onde a LLM enxerga seu problema.

Cada token mal investido = **menos espaço para a inteligência da LLM raciocinar** + mais idas-e-vindas que custam tempo e dinheiro.

**Não é sobre escolher o LLM mais caro.** É sobre orquestrar com precisão **o que entra, quando entra e quando sai** da janela.

---

## Janela de Contexto

- **Array de mensagens** reenviado **inteiro** a cada request
- **Limites:** Opus 4.5 (200k) · GPT-5.2 (400k) · Gemini 3 Pro (1M)
- **4 Roles:** `system` · `user` · `assistant` · `tool`
- **Compactação:** quando enche, ferramenta resume → **degrada e alucina**
- **Short-term memory:** vive na janela; some com `/clear`

---

## Tool Call (RPC puro)

```
Usuário → Ferramenta → LLM
              ↑          ↓
              └─ executa ┘
              (a LLM NÃO executa)
```

- LLM devolve **JSON** pedindo execução
- **Host** executa (Claude Code, Cursor, etc.)
- Resultado volta como nova mensagem (`tool` role)

---

## Anatomia de um fix

1. `system` + tools + `user: "corrija arquivo X"`
2. LLM → `tool_call: list_directory`
3. Host executa → `tool_result`
4. LLM → `tool_call: read_file`
5. Host executa → `tool_result`
6. LLM → `tool_call: edit_file`
7. Host executa → `tool_result`
8. LLM → `assistant: "corrigido"`

**Cada passo reenvia o array INTEIRO.** Por isso a janela infla.

---

## Modelos (Janeiro 2026)

| Modelo | Janela | Forte em |
|--------|--------|----------|
| **GPT-5.2 Codex** | 400k | Bugs complexos |
| **Opus 4.5** | 200k | Generalista day-to-day |
| **Gemini 3 Pro** | 1M | Front-end, docs grandes |

---

## Tipos de ferramenta

| Tipo | Exemplos |
|------|----------|
| **IDE-first** | Cursor, Windsurf |
| **Plugin** | Copilot |
| **CLI Agent** | Claude Code, Codex CLI, OpenCode, Gemini CLI, Droid |
| **Async/Cloud** | Antigravity |

---

# 3. Long-term Memory ⭐

---

## A Regra de Ouro

> **Se é sempre → Rule**
>
> **Se é às vezes → Skill**
>
> **Se é dado externo → MCP**

---

## Rules (Sempre Carregadas)

Arquivo Markdown injetado no system prompt em **toda conversa**.

| Ferramenta | Arquivo |
|------------|---------|
| Claude Code | `CLAUDE.md` |
| Codex | `AGENTS.md` |
| Gemini | `GEMINI.md` |
| Copilot | `.github/copilot-instructions.md` |
| Cursor | `.cursor/rules/*.mdc` |

---

## Rules — Cuidados

✅ **Colocar:**
- Convenções (camelCase, kebab-case)
- Estrutura de pastas
- Libs realmente usadas

❌ **NÃO colocar:**
- Tecnologias que o projeto não usa
- Regras de negócio condicionais
- Texto gerado por LLM sem revisar (10k+ tokens fácil)

---

## Skills (Carregadas On-Demand)

```
.claude/skills/react-best-practices/
  SKILL.md       ← header (name + description) vai pro contexto
  references/
  templates/
  scripts/
```

- Criadas pela Anthropic em **25/nov/2025**
- Marketplace: **skills.sh** (~34k) e **skillsmcp.com**
- **Problema:** LLM frequentemente ignora skills
- **Solução:** reforce via Rule (`sempre que X, use skill Y`)

---

## MCP (Model Context Protocol)

```
Host → Client → Server → Recurso externo
(Cursor)        (stdio ou URL)
```

| MCP popular | Uso |
|-------------|-----|
| Context7 | Docs de libs |
| GitHub | PRs, issues |
| Terraform | Provisionamento de infra |

---

## ⚠️ MCP é caro

- Cada MCP injeta **TODAS** suas tools no início da janela
- GitHub MCP consumia ~26k tokens só pra existir
- **Tool Search Tool** (Anthropic, 24/nov/2025) reduziu para ~8.7k
- **Audite MCPs:** desligue o que não usou na semana

---

## Árvore de decisão

```
Preciso adicionar conhecimento/capacidade?
│
├── Dado externo dinâmico?  ──→ MCP
├── Vale para TODA tarefa?  ──→ Rule
├── Processo c/ templates?  ──→ Skill
└── Senão                   ──→ Prompt da tarefa
```

---

# 4. Anti-padrões

---

## Anti-padrões de Context

❌ **Prompt evasivo** ("tem um bug, corrige") → cava via tool calls, queima token

❌ **Mesma janela pra múltiplas tarefas** → compactação cedo, alucinação

❌ **Arquivo > 5k linhas inteiro** → janela explode

❌ **"Bom dia", "obrigado"** → tokens reais persistindo

---

## Anti-padrões de Long-term Memory

❌ Copiar rules da internet sem checar o projeto

❌ Regras de negócio no `CLAUDE.md` num ERP de 25 módulos

❌ Encher de MCPs que você não usa

❌ Rules geradas por LLM coladas sem revisar

❌ Referenciar **path** da skill em vez do **nome**

---

## Anti-padrões de Postura

❌ **Aprovar cada tool call manualmente**
> "imagina pagar júnior e perguntar se pode ler arquivo"

❌ **"Fiz em 5 min com IA" na daily** → CEO vai cobrar todos nesse ritmo

❌ **Acreditar em "problema resolvido"** sem rodar o teste

❌ **LLM local achando que substitui API** → TPS é baixíssimo

---

# 5. Para Onde Vamos

---

## Tendências

1. **Tool Search Tool** (~70k → 8.7k tokens iniciais)
2. **Skills + MCP on-demand** (MCP só carrega quando skill é chamada)
3. **Multi-agent** — system prompts diferentes por etapa
4. **Throughput como diferencial** (Cerebras 1000+ TPS)
5. **Novo perfil do dev:** spec writer + reviewer

---

## A visão consensual

> *"A LLM bateu no teto. O avanço agora vem do **ferramental** (harness, tools, agentes, skills), não dos modelos."*

---

# Resumo em uma frase

> **Context Engineering é a arte de controlar o array de mensagens que entra na LLM a cada request — e Rules + Skills + MCP são como você reusa contexto sem queimar tokens.**

---

# Aplicação Imediata

1. **Auditar** MCPs registrados hoje à noite
2. **Refatorar** seu `CLAUDE.md` removendo regras que o projeto não segue
3. **Instalar** 2-3 skills de domínios que a LLM erra mais
4. **Praticar** prompt estruturado (XML + Markdown + `<critical>`)

---

# Q&A

Obrigado!

📚 **Material completo:** `apresentacao-introducao-ai-coding-tools-v2.md`
