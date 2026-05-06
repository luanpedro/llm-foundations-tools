# llm-foundations-tools

Materiais e ferramentas para a palestra **"Introdução a AI Coding Tools"** — fundamentos de LLMs, *Context Engineering*, *Long-term Memory* (Rules / Skills / MCP) e anti-padrões.

Autor: Luan Rodrigues e Fabricio Galvani

---

## Estrutura do repositório

```
llm-foundations-tools/
├── ia-foundations-tools/
│   ├── slides-executivo.md     # Conteúdo fonte da apresentação (Markdown)
│   └── apresentacao.html       # Slide deck HTML self-contained (gerado)
├── agent/
│   └── html-presentation-specialist.md  # Agente Claude Code que gera o HTML
└── .claude/                    # Configuração local do Claude Code
```

### `ia-foundations-tools/`

- [`slides-executivo.md`](ia-foundations-tools/slides-executivo.md) — fonte canônica do conteúdo. Estrutura em 5 blocos: Fundamentos · Context Engineering · Long-term Memory · Anti-padrões · Para Onde Vamos.
- [`apresentacao.html`](ia-foundations-tools/apresentacao.html) — versão renderizada estilo PowerPoint, **sem dependências externas** (CSS/JS inline). Roda offline em qualquer browser.

**Navegação do deck:**

| Tecla | Ação |
|-------|------|
| `→` `↓` `Espaço` `Click` | Próximo slide |
| `←` `↑` | Slide anterior |
| `F` | Fullscreen |

### `agent/html-presentation-specialist.md`

Definição de um *subagent* do Claude Code especializado em converter o Markdown executivo em HTML dark-theme com navegação por teclado, progress bar e contador de slides. Centraliza o design system (cores, tipografia, padrões de SVG para diagramas de contexto).

Para usar localmente, copie para `~/.claude/agents/` ou `.claude/agents/` no projeto.

---

## Como atualizar a apresentação

1. Edite [`ia-foundations-tools/slides-executivo.md`](ia-foundations-tools/slides-executivo.md).
2. Peça ao Claude Code para regenerar o HTML usando o agente `html-presentation-specialist`:
   ```
   atualize apresentacao.html a partir do slides-executivo.md
   ```
3. Abra `apresentacao.html` no browser para validar.

Edições incrementais (correções pontuais) podem ser feitas direto no HTML — o agente sabe localizar e editar trechos sem reescrever o arquivo inteiro.

---

## Conteúdo da palestra (resumo)

> **Context Engineering é a arte de controlar o array de mensagens que entra na LLM a cada request — e Rules + Skills + MCP são como você reusa contexto sem queimar tokens.**

- **Fundamentos:** LLM é stateless; memória é da ferramenta; token é unidade de cobrança.
- **Context Engineering:** janela de contexto, roles (`system`/`user`/`assistant`/`tool`), anatomia de tool calls, modelos atuais.
- **Long-term Memory:** árvore de decisão Rule × Skill × MCP, custos reais de cada estratégia.
- **Anti-padrões:** prompts evasivos, MCPs não auditados, regras coladas da internet, aprovar tool call manual.
