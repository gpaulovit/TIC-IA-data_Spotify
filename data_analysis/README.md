# data_analysis

Notebooks do projeto **Hit Maker**, refatorados a partir de [`hitmakerprjct`](../hitmakerprjct) e do notebook Colab já limpo pela equipe, organizados no fluxo de trabalho do projeto:

```
01_guiding_questions  →  02_requisitos  →  03_tratamento_limpeza_dados  →  04_ideacao_solucao  →  05_prototipo_produto
```

| Notebook | Frente(s) das GQs | Conteúdo |
|---|---|---|
| [`01_guiding_questions.ipynb`](01_guiding_questions.ipynb) | — | Consolidação das Guiding Questions (`docs/README.md` + `docs/refinamentoGQ.md`), cobertura pelo código existente, síntese da ideia |
| [`02_requisitos.ipynb`](02_requisitos.ipynb) | — | Requisitos de dados, funcionais, decisões de negócio em aberto e requisitos éticos pendentes |
| [`03_tratamento_limpeza_dados.ipynb`](03_tratamento_limpeza_dados.ipynb) | 1. Auditoria e Diagnóstico | Carga, auditoria, outliers e tratamento de nulos sobre `archive/dataset.csv` |
| [`04_ideacao_solucao.ipynb`](04_ideacao_solucao.ipynb) | 2. Anatomia do Sucesso / 3. Modelagem Preditiva | Definição de hit, EDA comparativa, modelo Random Forest, clustering KMeans |
| [`05_prototipo_produto.ipynb`](05_prototipo_produto.ipynb) | 4. Produto e Inteligência de Mercado | Perfil de referência por gênero, faixas subestimadas (modelo e similaridade), lacunas para virar produto |

Os notebooks 03–05 são sequenciais: cada um lê os artefatos (`artifacts/*.csv`, `*.pkl`) salvos pelo anterior, em vez de reprocessar tudo do zero. `artifacts/` não é versionado com dados (apenas `.gitkeep`) — é gerado ao rodar os notebooks em ordem.

Os notebooks 01 e 02 são só markdown (documentação/análise), sem código.
