# Glossário — Constituição para Agentes de IA e Skills

Termos canônicos usados na apresentação. Este arquivo é um glossário: define
o vocabulário compartilhado, não decisões de implementação.

> **Contexto da palestra:** 15–20 min (~12–16 slides). Arco C (por quê → o que
> é → como escrever → portabilidade/bootstrap → anti-padrões → aplicação).
> **Audiência:** já usa IA no dia a dia, mas ainda **não migrou de consumidor
> para configurador** — não extrai skills do trabalho repetitivo nem escreve
> constituição. A constituição é apresentada como o **primeiro artefato durável**
> (a porta de entrada), não como tópico avançado.
> **Demo ao vivo:** o fecho mostra constituição + skill funcionando na prática.
> Os slides constroem o modelo mental; a **demo entrega o "aha"** (por isso
> skills não precisam de seção teórica — a demo carrega isso).
> - **Placement:** slide 10 (penúltimo beat); CTA vem DEPOIS da demo.
> - **Subject:** repo REAL do apresentador (credibilidade "meu trabalho de
>   verdade"). Repo específico será decidido depois (NÃO é este repo de notas).
>   Riscos a mitigar: (1) sanitizar segredos/ruído na tela;
>   (2) legibilidade no projetor (fonte grande, foco no arquivo certo).
> - **Shape:** ANTES/DEPOIS, mesma tarefa. Run 1 (sem constituição → agente
>   ultrapassa/ignora estilo) **pré-gravado/pré-encenado** p/ de-riscar; Run 2
>   (com constituição + skill → limpo, respeita guardrails) **ao vivo**.
> - Slide 10 fica como placeholder estruturado (compromete com o formato
>   antes/depois, não com repo/tarefa específicos).

---

## Constituição (de agente)

Documento de governança, autorado pela pessoa/time, que reúne os **princípios
invioláveis** que governam como os agentes de IA e skills de um projeto/stack se
comportam — escopo, limites de segurança, valores, regras de escalonamento.

- **É:** camada de governança acima das regras operacionais do dia a dia.
- **Não é:** o método de treino "Constitutional AI" da Anthropic (isso é
  *upstream*, feito no treinamento do modelo). Aqui o sentido é *downstream*:
  algo que **você** escreve para o **seu** stack de agentes.
- Relaciona-se com, mas não se confunde com, [[rule]] (CLAUDE.md) e [[skill]].

**Constituição é um conceito agnóstico de ferramenta.** O *conceito* (princípios
invioláveis a respeitar) é o mesmo em Claude, Devin, Copilot, Cursor, Codex etc.
O que muda é o **arquivo canônico** onde ele mora:

| Ferramenta | Arquivo canônico da constituição |
|------------|----------------------------------|
| Padrão aberto (Devin, Copilot, Cursor, Codex, Gemini CLI, Windsurf, Jules…) | `AGENTS.md` |
| Claude Code | `CLAUDE.md` (fino) que **importa** `AGENTS.md` via `@AGENTS.md` |

- `AGENTS.md` = padrão aberto (proposto pela OpenAI em ago/2025; doado à Linux
  Foundation / Agentic AI Foundation em dez/2025; lido nativamente por 30+ agentes).
- **Claude Code lê `CLAUDE.md`, não `AGENTS.md` diretamente.** A ponte é o
  import `@AGENTS.md` na 1ª linha do `CLAUDE.md`.
- **Como iniciar (Claude Code):** `/init` gera o `CLAUDE.md`; se já existir
  `AGENTS.md`, o `/init` lê e incorpora. Padrão recomendado: `AGENTS.md` como
  fonte única + `CLAUDE.md` fino que só faz `@AGENTS.md`.

## As 5 categorias de princípios de uma constituição

Backbone conceitual (slide 7). Cada categoria produz regras invioláveis:

1. **Limites de ação** — o que nunca fazer sem permissão humana explícita
   (apagar dado de produção, rodar migration, force-push).
2. **Honestidade / anti-alucinação** — nunca forjar teste passando; se não
   rodou, dizer que não rodou; não inventar APIs/arquivos.
3. **Escalonamento na incerteza** — escopo/intenção ambíguo → perguntar antes
   de agir, não adivinhar.
4. **Escopo & foco** — fazer só o que foi pedido; sem refatoração de brinde;
   não tocar em código fora do escopo.
5. **Fidelidade ao existente** — seguir estilo/estrutura já presentes; não
   introduzir libs/padrões novos por conta própria.

Simetria com as dores de abertura: cat. 4 e 5 curam a **dor da repetição**
(slide 2); cat. 1, 2 e 3 curam a **dor da ação não-autorizada** (slide 3).

> **Fonte dos exemplos:** o apresentador TEM uma constituição real, mas ela
> não está nesta máquina. Slide 7 + demo Run 2 usam a estrutura das 5 categorias
> como **template com placeholders marcados** (ex.: `‹sua regra aqui›`). As
> linhas reais serão coladas depois — swap num único ponto. Nada inventado deve
> aparecer como se fosse a constituição real do apresentador.

## Aplicação hoje à noite (CTA — slide 12)

1. **Escreva seus princípios COM a IA.** Não parta de página em branco: use a
   própria IA para estruturar seus princípios (as 5 categorias como guia) num
   `AGENTS.md`. A IA organiza; **você** é a fonte dos princípios. É o mesmo loop
   que estrutura esta palestra — o ponto é *meta*: fazer com a IA o que o
   apresentador acabou de fazer no palco.
2. **Conecte à sua ferramenta.** Claude: `/init` + `@AGENTS.md` na 1ª linha do
   `CLAUDE.md`. Copilot/Cursor/Devin: `AGENTS.md` já é lido.
3. **Ache a próxima repetição.** Uma tarefa que você fez do mesmo jeito 3+ vezes
   esta semana = sua primeira skill (próximo passo).

## Resumo em uma frase (fecho — slide 12)

> **"Uma constituição é a primeira coisa que você escreve quando para de repetir
> instruções pra IA e começa a governá-la — e é o mesmo instinto que vira sua
> primeira skill."**

## Constituição vs Convenção (distinção-chave)

- **Convenção** (o grosso do CLAUDE.md/AGENTS.md): *como fazemos as coisas aqui*
  — camelCase, usamos Postgres, estrutura de pastas. Estilo.
- **Constituição** (camada de topo): *o que NUNCA pode acontecer e o que fazer
  na dúvida* — nunca apagar dado de produção sem confirmação humana, nunca
  forjar teste passando, sempre escalar quando o escopo for incerto. Governa o
  **julgamento** do agente, não o estilo. Poucas, altas, invioláveis.
