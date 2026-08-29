# OpenCode setup

Este repositório contém meu setup reutilizável do OpenCode:

- `AGENTS.md` global;
- skills reutilizáveis;
- workflows customizados;
- um exemplo seguro da configuração recomendada.

## Organização

```text
AGENTS.md
→ comportamento global

commands/
→ workflows explícitos

skills/
→ capacidades especializadas carregadas quando relevantes

MCP
→ conhecimento ou sistemas externos

CLI
→ execução local determinística

tests/evals
→ validação
```

Neste ambiente, os caminhos globais usados pelo OpenCode são:

```text
~/.config/opencode/AGENTS.md
~/.config/opencode/skills/
~/.config/opencode/commands/
~/.config/opencode/opencode.jsonc
```

Para reconstruir um ambiente novo, clone o repositório, revise o conteúdo e copie os arquivos:

```bash
mkdir -p ~/.config/opencode/skills ~/.config/opencode/commands
cp AGENTS.md ~/.config/opencode/AGENTS.md
cp -a skills/. ~/.config/opencode/skills/
cp -a commands/. ~/.config/opencode/commands/
cp examples/opencode.example.json ~/.config/opencode/opencode.jsonc
```

Se já houver configuração nesses caminhos, compare e faça backup antes de substituir arquivos. O exemplo de configuração é JSON válido e também pode ser usado como JSONC.

## Fluxo por projeto

1. Abrir o repositório no OpenCode.
2. Executar `/init`.
3. Executar `/setup-project`.
4. Revisar as recomendações.
5. Aprovar apenas MCPs ou integrações realmente necessárias.

`/init` cria ou atualiza instruções específicas do repositório. `/setup-project` analisa o projeto e adiciona somente o delta que não é coberto pelo setup global; ele não deve duplicar as skills globais.

## Workflows

- `/investigate` — investigar bugs, regressões e comportamentos inesperados.
- `/implement` — implementar mudanças de forma incremental e validada.
- `/review` — revisar código e diffs buscando problemas reais e regressões.
- `/experiment` — criar comparações técnicas controladas e gerar evidência.
- `/audit` — auditar sistemas ou subsistemas e priorizar riscos.
- `/setup-project` — configurar o ambiente OpenCode específico do projeto.

Exemplos:

```text
/investigate "Why did model performance degrade after deployment?"
/review "Review the current git diff"
```

## Skills e roteamento

Normalmente não é necessário chamar uma skill manualmente. O agente vê as skills disponíveis e carrega automaticamente apenas as que forem relevantes. A `description` determina quando a skill se aplica; o `SKILL.md` descreve como executar a especialidade. Múltiplas skills podem ser combinadas, usando sempre o menor conjunto necessário.

```text
/investigate "Model performance degraded after deployment"

Possible routing:

debugging
+
ml-engineer
+
model-evaluator
```

Skills atuais:

- `agent-engineer` — agentes com tools, planejamento, orquestração, memória e confiabilidade.
- `computer-vision-engineer` — classificação, detecção, segmentação, OCR e outros sistemas de visão.
- `context-engineering` — seleção, montagem, compressão e avaliação de contexto para LLMs e agentes.
- `debugging` — investigação sistemática de bugs, regressões, falhas e comportamento inesperado.
- `eda-specialist` — análise exploratória, qualidade de dados, distribuições, anomalias e leakage.
- `experiment-designer` — experimentos controlados, hipóteses, baselines, métricas e ablações.
- `feature-engineering` — criação, transformação, seleção e validação de features preditivas.
- `llm-evaluation` — avaliação de LLMs, RAG, agentes, prompts, tools e saídas estruturadas.
- `ml-engineer` — ciclo de vida de ML, treinamento, serving, deployment, monitoring e reprodutibilidade.
- `model-evaluator` — desempenho, calibração, robustez, estabilidade e utilidade de modelos de ML.
- `nlp-engineer` — classificação de texto, extração, embeddings, busca e entendimento de linguagem.
- `optimization-engineer` — otimização matemática com objetivos, variáveis de decisão e restrições.
- `python-engineer` — software Python de produção, arquitetura, testes, typing e manutenção.
- `rag-engineer` — ingestão, retrieval, reranking, contexto, grounding e avaliação de RAG.
- `statistician` — inferência, incerteza, testes, efeito, amostragem e validade estatística.
- `tool-design` — schemas, descrições, seleção, permissões e confiabilidade de tools para agentes.

## Global e projeto

```text
GLOBAL
→ conhecimento e workflows reutilizáveis

PROJECT
→ somente o delta específico daquele projeto
```

Exemplos globais: `AGENTS.md`, skills, workflows, Context7 e `gh_grep`.

Exemplos de projeto: MLflow MCP, banco específico, observabilidade, infraestrutura, skill de domínio proprietária ou workflow específico daquele sistema.

> Global capabilities are the reusable baseline. Project configuration contains only the delta.

```text
Local deterministic operation
→ CLI

External reusable knowledge
→ global MCP

External project-specific state
→ project MCP
```

Em geral, `pytest`, `git`, `uv` e `docker` são CLI; Context7 e `gh_grep` são MCPs globais; MLflow, banco, observabilidade e infraestrutura de cloud são normalmente MCPs do projeto.

## Segurança

Nunca versione tokens, API keys, credentials, `.env`, dados OAuth, dados de sessão ou outros secrets. O `.gitignore` reduz o risco de inclusão acidental, mas não substitui uma auditoria de secrets antes de cada publicação.
