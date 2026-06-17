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

### Como não gastar todos os tokens até o final do mês?

![img](https://upload.wikimedia.org/wikipedia/commons/e/eb/Ei-sc-github.svg)

---

# Por que a IA está tão cara?

A eficiência aumentou, mas o custo total continua subindo.

---

| Geração / Modelo | Data Aproximada | Entrada (Input) | Saída (Output) |
| --- | --- | --- | --- |
| Claude 3 Haiku | Março 2024 | US$ 0,25 | US$ 1,25 |
| Claude 3 Sonnet | Março 2024 | US$ 3,00 | US$ 15,00 |
| Claude 3 Opus | Março 2024 | US$ 15,00 | <span v-mark="{ color: 'rgb(255, 0, 0)' }">US$ 75,00</span> |
| Claude 3.5 Haiku | Outubro 2025 | US$ 0,80 | US$ 4,00 |
| Claude 4 Haiku (4.5) | Outubro 2025 | US$ 1,00 | US$ 5,00 |
| Claude 4 Sonnet (4.6) | Fevereiro 2026 | US$ 3,00 | US$ 15,00 |
| Claude 4 Opus (4.6/4.7/4.8) | Fevereiro 2026 | US$ 5,00 | US$ 25,00 |
| Claude Fable 5 | Junho 2026 | US$ 10,00 | <span v-mark="{ color: 'rgb(0, 255, 170)' }">US$ 50,00</span> |

<small>fontes: [supermind](https://suprmind.ai/hub/claude/pricing/) [benchlm](https://benchlm.ai/llm-pricing-trends)</small>

---

# Quais os impactos disso?

### Marcos na Precificação
- **O "Reset" do Tier Premium (Fev 2026):** Lançamento do Claude Opus 4.6 com redução de 67% no preço.
- **Fim da Sobretaxa de Contexto:** Requisições > 200k tokens não pagam mais dobro.
- **Janela de 1M de Tokens:** Preço padrão para Opus e Sonnet 4.6.
- **O Novo Tokenizador (Abril 2026):** O Claude Opus 4.7 pode gerar <span v-mark="{ at: 1, color: 'rgb(255, 0, 0)' }">até 35% mais tokens</span> para o mesmo texto, elevando o custo real.

<small>fontes: [silicondata](https://www.silicondata.com/use-cases/anthropic-claude-api-pricing-2026/) [evolink](https://evolink.ai/blog/claude-api-pricing-guide-2026)</small>

---

O custo real não está no token individual, mas na **quantidade consumida em tarefas agenticas**.

A transição de chats simples para **Tool Calling** e agentes autônomos disparou o consumo. Uma tarefa complexa (pesquisa $\rightarrow$ redação $\rightarrow$ validação) pode consumir de 50k a 500k tokens.

**Os principais "vilões":**
- **Execução Encadeada:** Múltiplas iterações para a mesma tarefa.
- **Contexto Acumulado:** Protocolos stateless que revisita todo o histórico.
- **Taxas de Entrada de Ferramentas:** 
  - Bash: +245 tokens/chamada
  - Editor de Texto: +700 tokens/chamada

<small>fontes: [intuitionlabs](https://intuitionlabs.ai/articles/claude-pricing-plans-api-costs) [evolink](https://evolink.ai/blog/claude-api-pricing-guide-2026)</small>
---

# Paradoxo de Jevons

William Stanley Jevons observou que as melhorias tecnológicas que aumentaram a eficiência do uso do carvão levaram ao aumento do consumo de carvão em uma ampla gama de indústrias.

À medida que o custo por token cai (ficando até 1.000 vezes mais barato em 3 anos), as empresas criam fluxos de trabalho que antes seriam economicamente inviáveis, como assistentes que processam milhões de tokens por desenvolvedor por dia. O uso deixou de ser uma interação pontual e passou a ser um fluxo de execução contínua em segundo plano

<small>fontes: [theshift](https://theshift.info/hot/a-economia-da-ia-mudou/) [wikipedia](https://en.wikipedia.org/wiki/Jevons_paradox)</small>

---

# Não tem mais dados para treinar??

O que é o Data Wall?

O "Data Wall" na Inteligência Artificial é visto como um obstáculo ou limite para o progresso da IA.
O avanço da IA depende de grandes volumes de dados de alta qualidade para o treinamento.
No entanto, o estoque de dados utilizáveis na internet está diminuindo ou tornando-se mais caro de coletar.

A escassez de dados para o treino de Inteligência Artificial é real e debatida no setor. Sam Altman (CEO da OpenAI) reconheceu que a "raspagem" massiva da internet pública atingiu um limite.

Ou seja, o avanço da IA de agora em diante não será mais na inteligencia, mas no uso de ferramentas autonomas que gastam cada vez mais tokens e geram cada vez mais codigo e artefatos no projeto

<small>fonte: [Microsoft](https://www.microsoft.com/en-us/research/articles/synthllm-breaking-the-ai-data-wall-with-scalable-synthetic-data/) [Wikipedia](https://pt.wikipedia.org/wiki/Colapso_de_modelos) [Unisinos](https://ihu.unisinos.br/sobre-o-ihu/78-noticias/638264-a-curva-de-aprendizagem-da-inteligencia-artificial-esta-em-risco-a-sua-ansia-por-dados-e-infinita-mas-nao-as-obras-humanas)</small>

---

# O que é tool caling

![tool-call](https://docs.temporal.io/assets/images/tool-call-openai-python-assets-tool-calling-flow-616ae35809d90e7f788fe8b3702204cf.png)

---

# Finalmente: Como reduzir o uso de tokens?

Existem duas abordagens principais:

1. **Terminal Proxy**
   - Filtragem de saída bruta do shell antes de enviar ao LLM.
   - Ex: [Rust Token Killer (RTK)](https://github.com/rtk-ai/rtk)

2. **SKILLS**
   - Especialização de contexto para evitar redundância.
   - Ex: [ponytail](https://github.com/DietrichGebert/ponytail)



---

# Como RTK funciona?

````md magic-move [git status]
```bash
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
```
```bash
* main
~ Modified: 1 files
   README.md
```
````

````md magic-move [ls -l]
```bash
total 248
drwxr-xr-x. 1 oburato oburato    204 jun 17 02:27 .
drwxr-xr-x. 1 oburato oburato     74 jun 17 02:55 ..
-rw-r--r--. 1 oburato oburato     81 jun 17 00:35 .gitignore
drwxr-xr-x. 1 oburato oburato     22 jun 17 02:28 img
-rw-r--r--. 1 oburato oburato    230 jun 17 00:42 netlify.toml
drwxr-xr-x. 1 oburato oburato  10422 jun 17 01:20 node_modules
-rw-r--r--. 1 oburato oburato    381 jun 17 01:20 package.json
-rw-r--r--. 1 oburato oburato 221290 jun 17 01:20 pnpm-lock.yaml
-rw-r--r--. 1 oburato oburato    302 jun 17 00:35 pnpm-workspace.yaml
-rw-r--r--. 1 oburato oburato   6222 jun 17 03:41 slides.md
-rw-r--r--. 1 oburato oburato    144 jun 17 00:42 vercel.json
```
```bash
img/
node_modules/
.gitignore  81B
netlify.toml  230B
package.json  381B
pnpm-lock.yaml  216.1K
pnpm-workspace.yaml  302B
slides.md  6.1K
vercel.json  144B

Summary: 7 files, 2 dirs (2 .yaml, 2 .json, 1 .md, 1 .gitignore, 1 .toml)
```
````

---


# Como funciona as SKILLS

Injetando o arquivo de skill no contexto antes da execução

Escreva um código em Python que leia o arquivo sales.csv e some os valores da coluna 'amount'.
````md magic-move [CSV Sum]
```python
import pandas as pd

try:
    df = pd.read_csv('sales.csv')
    total_amount = df['amount'].sum()
    print(f"Total amount: ${total_amount:,.2f}")
except FileNotFoundError:
    print("Error: sales.csv not found")
except KeyError:
    print("Error: 'amount' column not found")
```
```python
import csv

total = sum(float(row['amount']) for row in csv.DictReader(open('sales.csv')))
print(total)
```
````
