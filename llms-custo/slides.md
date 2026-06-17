---
theme: default
title: Apresentação como reduzir custos com a IA no vscode
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)

class: text-center
drawings:
  persist: true
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: slide-left
# enable Comark Syntax: https://comark.dev/syntax/markdown
comark: true
# duration of the presentation
duration: 45min
colorSchema: light
---

### Como não gastar todos os tokens até o final do mes?

![img](https://upload.wikimedia.org/wikipedia/commons/e/eb/Ei-sc-github.svg)

---

# Por que a IA está tão cara?

---

| Geração / Modelo | Data Aproximada | Entrada (Input) | Saída (Output) |
| --- | --- | --- | --- |
| Claude 3 Haiku | Março 2024 | US$ 0,25 | US$ 1,25 |
| Claude 3 Sonnet | Março 2024 | US$ 3,00 | US$ 15,00 |
| Claude 3 Opus | Março 2024 | US$ 15,00 | US$ 75,00 |
| Claude 3.5 Haiku | Outubro 2025 | US$ 0,80 | US$ 4,00 |
| Claude 4 Haiku (4.5) | Outubro 2025 | US$ 1,00 | US$ 5,00 |
| Claude 4 Sonnet (4.6) | Fevereiro 2026 | US$ 3,00 | US$ 15,00 |
| Claude 4 Opus (4.6/4.7/4.8) | Fevereiro 2026 | US$ 5,00 | US$ 25,00 |
| Claude Fable 5 | Junho 2026 | US$ 10,00 | US$ 50,00 |

fontes: [supermind](https://suprmind.ai/hub/claude/pricing/) [benchlm](https://benchlm.ai/llm-pricing-trends)

---

# Quais os impactos disso?

Marcos Importantes na Precificação
- O "Reset" do Tier Premium (Fevereiro 2026): O lançamento do Claude Opus 4.6 em 5 de fevereiro de 2026 representou a mudança mais consequente na história da API, com uma redução de 67% no preço (de US15/US 75 para US5/US 25).
- Eliminação da Sobretaxa de Contexto Longo: Anteriormente, requisições que excediam 200 mil tokens de entrada sofriam um aumento de preço (geralmente o dobro)
- A partir de março de 2026, a janela de contexto de 1 milhão de tokens passou a ser oferecida pelo preço padrão nos modelos Opus 4.6 e Sonnet 4.6
- O Impacto do Novo Tokenizador (Abril 2026): Com o lançamento do Claude Opus 4.7, embora o preço "de tabela" tenha permanecido em US5/US 25, a Anthropic introduziu um novo tokenizador que pode produzir até 35% mais tokens para o mesmo texto de entrada, o que pode elevar o custo real por tarefa

fontes: [silicondata](https://www.silicondata.com/use-cases/anthropic-claude-api-pricing-2026/) [evolink](https://evolink.ai/blog/claude-api-pricing-guide-2026)

---

O real custo não está no token, mas sim na quantidade que usamos para tarefas agenticas

O gasto de tokens em modelos de linguagem (LLMs) aumentou drasticamente com a transição de simples interações de chat para a execução de tarefas via ferramentas (tool calling) e agentes autônomos. Enquanto uma pergunta direta consome centenas de tokens, uma única tarefa executada por um sistema de agentes (como pesquisar, redigir, validar e iterar) pode consumir entre 50.000 e 500.000 tokens antes de entregar o resultado final.

Culpados?
- Execução Encadeada (Chain Execution)
- Protocolo Stateless e Contexto Acumulado
- Definições de Ferramentas (tool calling)

Custos de "Taxa de Entrada" por Ferramenta: No ecossistema Claude 4.x, por exemplo, o uso da ferramenta Bash adiciona automaticamente 245 tokens de entrada por chamada, enquanto o Editor de Texto adiciona 700 tokens de entrada fixos

fontes: [intuitionlabs](https://intuitionlabs.ai/articles/claude-pricing-plans-api-costs) [evolink](https://evolink.ai/blog/claude-api-pricing-guide-2026)
---

# Paradoxo de Jevons

William Stanley Jevons observou que as melhorias tecnológicas que aumentaram a eficiência do uso do carvão levaram ao aumento do consumo de carvão em uma ampla gama de indústrias.

À medida que o custo por token cai (ficando até 1.000 vezes mais barato em 3 anos), as empresas criam fluxos de trabalho que antes seriam economicamente inviáveis, como assistentes que processam milhões de tokens por desenvolvedor por dia. O uso deixou de ser uma interação pontual e passou a ser um fluxo de execução contínua em segundo plano

fontes: [theshift](https://theshift.info/hot/a-economia-da-ia-mudou/) [wikipedia](https://en.wikipedia.org/wiki/Jevons_paradox)

---

# Não tem mais dados para treinar??

O que é o Data Wall?

O "Data Wall" na Inteligência Artificial é visto como um obstáculo ou limite para o progresso da IA.
O avanço da IA depende de grandes volumes de dados de alta qualidade para o treinamento.
No entanto, o estoque de dados utilizáveis na internet está diminuindo ou tornando-se mais caro de coletar.

![Model_Collapse](https://upload.wikimedia.org/wikipedia/commons/e/ef/Model_Collapse_in_Generative_Models_Can_Be_Avoided_By_Accumulating_Data.png)

Ou seja, o avanço da IA de agora em diante não será mais na inteligencia, mas no uso de ferramentas autonomas

fonte: [Microsoft](https://www.microsoft.com/en-us/research/articles/synthllm-breaking-the-ai-data-wall-with-scalable-synthetic-data/) [Wikipedia](https://pt.wikipedia.org/wiki/Colapso_de_modelos)

---

# O que é tool caling

![tool-call](https://docs.temporal.io/assets/images/tool-call-openai-python-assets-tool-calling-flow-616ae35809d90e7f788fe8b3702204cf.png)

---

# Final mente como reduzir o uso de tokens?

Existem duas formas
- SKILLS
[ponytail](https://github.com/DietrichGebert/ponytail)
![img](https://raw.githubusercontent.com/DietrichGebert/ponytail/refs/heads/main/assets/benchmark-3model.svg)

- terminal proxy
[rust token killer](https://github.com/rtk-ai/rtk)
  Without rtk:                                    With rtk:

  Claude  --git status-->  shell  -->  git         Claude  --git status-->  RTK  -->  git
    ^                                   |            ^                      |          |
    |        ~2,000 tokens (raw)        |            |   ~200 tokens        | filter   |
    +-----------------------------------+            +------- (filtered) ---+----------+
