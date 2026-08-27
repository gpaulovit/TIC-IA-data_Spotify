# TIC-IA-data_Spotify

## Guiding Questions (GQ)

> Estrutura de cada GQ: **Pergunta** → **Ação** (Atividade + Recurso + Responsáveis/Prazo).
> Tags: `[EXISTENTE]` = já trazida pela equipe · `[MELHORADA]` = aprimorada/reformulada a partir de algo trazido pela equipe · `[NOVA]` = proposta pelo agente.
> Preencher `[Dupla a definir]` e `[Prazo a definir]` conforme a divisão de trabalho da equipe.

---

## 1 - Base de Dados

**[EXISTENTE]**
**Pergunta:** Quantas músicas existem no dataset e quantos gêneros musicais estão representados?
**Ação:** Rodar `df.shape` e `df['track_genre'].nunique()` em notebook exploratório (Python/Pandas) → **[Dupla a definir]**, prazo **[Prazo a definir]**.

**[EXISTENTE]**
**Pergunta:** Como as músicas estão distribuídas entre os gêneros?
**Ação:** Gerar `value_counts()` por `track_genre` e plotar histograma/barplot (Pandas + Matplotlib/Seaborn) → **[Dupla a definir]**, prazo **[Prazo a definir]**.

**[EXISTENTE]**
**Pergunta:** Existem valores ausentes nas principais variáveis?
**Ação:** Rodar `df.isnull().sum()` e mapear % de missing por coluna (Pandas) → **[Dupla a definir]**, prazo **[Prazo a definir]**.

**[EXISTENTE]**
**Pergunta:** Existem músicas duplicadas ou artistas/faixas repetidas, e como lidar com isso no modelo?
**Ação:** Checar duplicatas por `track_id` e por `(track_name, artists)`; documentar critério de deduplicação (Pandas `duplicated()`) → **[Dupla a definir]**, prazo **[Prazo a definir]**.

**[EXISTENTE]**
**Pergunta:** A distribuição dos dados é equilibrada entre os gêneros?
**Ação:** Analisar balanceamento via `value_counts(normalize=True)` e decidir se há necessidade de balanceamento/estratificação (Pandas) → **[Dupla a definir]**, prazo **[Prazo a definir]**.

**[EXISTENTE]**
**Pergunta:** Existem outliers nas principais características musicais, e como eles afetam ou mudam o padrão dos dados?
**Ação:** Boxplots + IQR/z-score nas variáveis acústicas (`loudness`, `tempo`, `duration_ms`, etc.) (Pandas + Seaborn) → **[Dupla a definir]**, prazo **[Prazo a definir]**.

**[EXISTENTE]**
**Pergunta:** A variável "popularity" apresenta distribuição equilibrada ou está concentrada em determinadas faixas ou tipos musicais?
**Ação:** Plotar histograma/KDE de `popularity` geral e por `track_genre`; checar concentração em faixas específicas (Pandas + Seaborn) → **[Dupla a definir]**, prazo **[Prazo a definir]**.

**[MELHORADA]**
**Pergunta:** Qual é o período temporal coberto por cada dataset (`dataset.csv`, sem coluna de data, vs. `Spotify Most Streamed Songs.csv`, cobrindo 1930–2023), e essa diferença de cobertura temporal compromete a validade do modelo frente aos dados "recentes" que pretendemos buscar via API do Spotify?
**Ação:** Confirmar fonte/versão do `dataset.csv` no Kaggle; documentar a lacuna de data como risco assumido; decidir se `Spotify Most Streamed Songs.csv` vira dataset de validação/teste — análise deixada para a etapa exploratória da equipe (Kaggle + Spotify Web API) → **[Dupla a definir]**, prazo **[Prazo a definir]**.

**[NOVA]**
**Pergunta:** Os dois datasets têm schemas e escalas compatíveis (ex: `danceability` 0–1 vs. `danceability_%` 0–100, `popularity` vs. `streams`) — como vamos padronizá-los para usar um como treino e o outro como validação/teste?
**Ação:** Mapear coluna a coluna a correspondência entre os dois arquivos e definir pipeline de normalização/renomeação (Pandas) → **[Dupla a definir]**, prazo **[Prazo a definir]**.

---

## 2 - Usuário

**[EXISTENTE]**
**Pergunta:** O que caracteriza uma música de alta popularidade?
**Ação:** Levantamento estatístico (correlação/importância de variáveis) entre `popularity` e as features acústicas do dataset (Pandas + `df.corr()` / feature importance) → **[Dupla a definir]**, prazo **[Prazo a definir]**.

**[EXISTENTE]**
**Pergunta:** Quais características musicais estão mais associadas às músicas de maior popularidade no Spotify?
**Ação:** Ranquear features por correlação/importância com `popularity` (ex: `energy`, `danceability`, `valence`, `loudness`) (Pandas + Seaborn heatmap) → **[Dupla a definir]**, prazo **[Prazo a definir]**.

**[EXISTENTE]**
**Pergunta:** Existe um perfil musical associado aos hits?
**Ação:** Segmentar faixas de alta popularidade (ex: top 10-20%) e comparar suas distribuições de features com o restante (Pandas + boxplots comparativos) → **[Dupla a definir]**, prazo **[Prazo a definir]**.

**[EXISTENTE]**
**Pergunta:** Existe um conjunto de características que define o perfil de uma música potencialmente bem-sucedida no Spotify? *(Ex: Alta popularidade = determinada energia + dançabilidade + valência + gênero)*
**Ação:** Clusterização (K-Means/hierárquico) sobre faixas de alta popularidade para identificar "perfis" recorrentes (Scikit-learn) → **[Dupla a definir]**, prazo **[Prazo a definir]**.

**[EXISTENTE]**
**Pergunta:** O gênero influencia o potencial de sucesso?
**Ação:** Comparar popularidade média por `track_genre` via ANOVA/teste estatístico (Pandas + Scipy) → **[Dupla a definir]**, prazo **[Prazo a definir]**.

**[EXISTENTE]**
**Pergunta:** Como o gênero musical influencia a popularidade das músicas e quais gêneros apresentam maior concentração de hits? *(Nota do time: útil para descobrir gêneros com maior popularidade média, mais hits, maior variabilidade, e gêneros com muitos lançamentos mas poucos hits.)*
**Ação:** Cruzar `track_genre` x `popularity` (média, desvio-padrão, contagem de hits acima de threshold) (Pandas groupby + visualização) → **[Dupla a definir]**, prazo **[Prazo a definir]**.

**[EXISTENTE]**
**Pergunta:** Os fatores associados à popularidade variam entre diferentes gêneros musicais?
**Ação:** Repetir a análise de correlação/importância de features segmentando por `track_genre` e comparar os rankings entre gêneros (Pandas groupby + comparação de correlações) → **[Dupla a definir]**, prazo **[Prazo a definir]**.

**[NOVA]**
**Pergunta:** Quem é o "usuário" final deste produto (artista/gravadora buscando prever sucesso pré-lançamento, curador de playlist, ou o próprio ouvinte) — e essa decisão muda quais features estão disponíveis no momento da predição (já que `popularity` e `streams` só existem depois do lançamento)?
**Ação:** Definir persona e o momento de uso do modelo (pré vs. pós-lançamento) em reunião de escopo com o time, documentando implicações de vazamento de dados (data leakage) → **[Dupla a definir]**, prazo **[Prazo a definir]**.

---

## 3 - Modelo

**[EXISTENTE]**
**Pergunta:** Existe uma "fórmula" para um hit? É possível identificar uma combinação de características capaz de prever se uma música terá alta popularidade?
**Ação:** Treinar modelo baseline (ex: Regressão Logística/Árvore) sobre as features acústicas e avaliar poder preditivo (Scikit-learn) → **[Dupla a definir]**, prazo **[Prazo a definir]**.

**[EXISTENTE]**
**Pergunta:** É possível criar um perfil de referência que permita ao artista comparar as características de sua música com as de músicas populares do mesmo gênero? *(possível dashboard comparando música x hit: danceability, energy, BPM, valence)*
**Ação:** Prototipar visualização tipo radar chart comparando a faixa do usuário à média dos hits do gênero (Pandas + Plotly/Matplotlib) → **[Dupla a definir]**, prazo **[Prazo a definir]**.

**[EXISTENTE]**
**Pergunta:** Quais características podem ser utilizadas para recomendar melhorias?
**Ação:** Extrair feature importance / coeficientes do modelo treinado e traduzir em recomendações acionáveis (Scikit-learn: `feature_importances_` / SHAP) → **[Dupla a definir]**, prazo **[Prazo a definir]**.

**[EXISTENTE]**
**Pergunta:** O que diferencia as músicas de alta popularidade das músicas de baixa popularidade?
**Ação:** Comparação estatística (teste t / Mann-Whitney) entre grupos de alta e baixa popularidade por feature (Scipy) → **[Dupla a definir]**, prazo **[Prazo a definir]**.

**[EXISTENTE]**
**Pergunta:** É possível desenvolver um modelo capaz de estimar a probabilidade de uma música atingir alta popularidade?
**Ação:** Treinar classificador probabilístico (ex: Regressão Logística, Random Forest) e validar via `predict_proba` + curva ROC (Scikit-learn) → **[Dupla a definir]**, prazo **[Prazo a definir]**.

**[EXISTENTE]**
**Pergunta:** Existe um conjunto de características capaz de representar o perfil de uma música potencialmente bem-sucedida?
**Ação:** Combinar clusterização (da Frente Usuário) com feature importance do modelo para validar convergência dos dois métodos (Scikit-learn) → **[Dupla a definir]**, prazo **[Prazo a definir]**.

**[EXISTENTE]**
**Pergunta:** É possível comparar uma música com o perfil dos hits de seu gênero e identificar pontos de aproximação ou diferença?
**Ação:** Calcular distância (ex: euclidiana/cosine) entre a faixa e o centróide de hits do gênero (Scikit-learn/Numpy) → **[Dupla a definir]**, prazo **[Prazo a definir]**.

**[NOVA]**
**Pergunta:** O problema será modelado como classificação binária ("hit" vs. "não-hit", exigindo definir um threshold de `popularity`) ou como regressão contínua sobre `popularity`? E qual métrica define "modelo bom o suficiente" (ex: F1, AUC-ROC, MAE, R²)?
**Ação:** Decidir formulação do problema e métrica-alvo em reunião técnica, documentando a justificativa e o threshold escolhido (caso classificação) → **[Dupla a definir]**, prazo **[Prazo a definir]**.

**[NOVA]**
**Pergunta:** Como o `track_genre` (114 categorias) será codificado no modelo (one-hot, target/embedding encoding, agrupamento em macro-gêneros), e isso gera risco de overfitting dado o volume de dados por gênero?
**Ação:** Testar 2-3 estratégias de encoding e comparar performance/overfitting via validação cruzada (Scikit-learn `cross_val_score`) → **[Dupla a definir]**, prazo **[Prazo a definir]**.

---

## 4 - Produto

**[EXISTENTE]**
**Pergunta:** Quais características diferenciam as músicas de alta popularidade das músicas de baixa popularidade? *(GQ que possibilita separação direta por grupos)*
**Ação:** Análise comparativa de features entre grupos de alta/baixa popularidade, traduzida em critérios de produto (Pandas + Scipy) → **[Dupla a definir]**, prazo **[Prazo a definir]**.

**[EXISTENTE]**
**Pergunta:** As músicas populares possuem um perfil musical diferente das demais? *(poderá ser usado para definir o perfil de preferência)*
**Ação:** Consolidar o perfil de hits (da Frente Modelo) em um "perfil de preferência" documentado para uso no produto (Pandas + documentação) → **[Dupla a definir]**, prazo **[Prazo a definir]**.

**[EXISTENTE]**
**Pergunta:** Existem músicas pouco populares semelhantes aos hits do próprio gênero? *(pode criar oportunidades para gravadoras)*
**Ação:** Calcular similaridade entre faixas pouco populares e o perfil de hits do mesmo gênero, para gerar lista de "candidatas a redescoberta" (Scikit-learn: distância/similaridade) → **[Dupla a definir]**, prazo **[Prazo a definir]**.

**[EXISTENTE]**
**Pergunta:** Quais características estão mais associadas à popularidade? *(para definir as variáveis do produto)*
**Ação:** Selecionar, a partir da feature importance do modelo, quais variáveis serão expostas na interface do produto (ex: dashboard/score) (Scikit-learn + design de produto) → **[Dupla a definir]**, prazo **[Prazo a definir]**.

**[EXISTENTE]**
**Pergunta:** Como o gênero musical influencia a popularidade e a concentração de hits?
**Ação:** Traduzir a análise por gênero (Frente Usuário/Modelo) em segmentação de produto (ex: benchmarks específicos por gênero) (Pandas groupby) → **[Dupla a definir]**, prazo **[Prazo a definir]**.

**[MELHORADA]** *(originalmente uma nota solta: "variável de feats, artista under x artista mais famosos para aumentar a possibilidade de hit")*
**Pergunta:** Incluir uma variável de "feat" (colaboração entre artista pouco conhecido e artista renomado) deveria ser uma feature recomendada pelo produto como fator de aumento de chance de hit?
**Ação:** Testar se dados de colaboração/artistas estão disponíveis via API do Spotify; se sim, avaliar correlação com `popularity` como candidato a nova feature (Spotify Web API + Pandas) → **[Dupla a definir]**, prazo **[Prazo a definir]**.

**[NOVA]**
**Pergunta:** Qual é o formato de entrega do produto final (dashboard interativo, relatório estático, API de score) e como mediremos sucesso do ponto de vista do usuário (ex: adoção, satisfação, utilidade percebida) — distinto da métrica técnica do modelo definida na Frente Modelo?
**Ação:** Definir wireframe/formato do produto e KPI(s) de produto em reunião de escopo, separando explicitamente de métricas de modelo (F1/AUC/etc.) → **[Dupla a definir]**, prazo **[Prazo a definir]**.

---

## 5 - Ética

**[NOVA]**
**Pergunta:** O dataset tem 114 gêneros com distribuição desigual — existe viés que sistematicamente sub-representa ou penaliza gêneros associados a certas culturas/regiões (ex: gêneros não-ocidentais, nichos)?
**Ação:** Auditar `popularity` média e volume de faixas por gênero/cluster cultural, verificando se o modelo herda desigualdades presentes nos dados de treino (Pandas + análise de fairness) → **[Dupla a definir]**, prazo **[Prazo a definir]**.

**[NOVA]**
**Pergunta:** O uso do dataset do Kaggle e da API oficial do Spotify para treinar e validar este modelo respeita as respectivas licenças e Termos de Serviço (ex: restrições de uso comercial, proibição de treinar ML com dados da Spotify API)?
**Ação:** Revisar a licença do dataset no Kaggle e o Spotify Developer Terms of Service, documentando o que é permitido para este projeto (acadêmico) → **[Dupla a definir]**, prazo **[Prazo a definir]**.

**[NOVA]**
**Pergunta:** Se o produto influenciar decisões reais de promoção/investimento, o modelo pode criar um efeito de retroalimentação (profecia autorrealizável) que reforça quem já é popular e dificulta a descoberta de novos artistas?
**Ação:** Discutir o risco em reunião de time e propor mitigação (ex: nunca usar o score como critério único/automático de decisão) → **[Dupla a definir]**, prazo **[Prazo a definir]**.

**[NOVA]**
**Pergunta:** Se artistas otimizarem suas músicas para se encaixar no "perfil de hit" identificado pelo modelo, isso pode reduzir a diversidade e a criatividade musical no ecossistema — como comunicamos essa limitação?
**Ação:** Documentar esse risco explicitamente na descrição do produto/limitações, como aviso ao usuário final → **[Dupla a definir]**, prazo **[Prazo a definir]**.

**[NOVA]**
**Pergunta:** Como evitamos que o produto seja usado de forma indevida por gravadoras/plataformas para decisões automatizadas prejudiciais a artistas (ex: descontinuar contrato só por causa do score)?
**Ação:** Definir política de uso responsável / termo de uso do produto deixando claro que é ferramenta de apoio à decisão, não decisão automática → **[Dupla a definir]**, prazo **[Prazo a definir]**.

**[NOVA]**
**Pergunta:** Mesmo sem dados pessoais de ouvintes, o produto expõe nomes e obras de artistas específicos (ex: em comparações "sua música x hits do gênero") — isso levanta alguma questão de consentimento ou uso de imagem?
**Ação:** Verificar se a exposição de artistas nominais no produto requer tratamento especial (anonimização em exemplos, ou confirmação de que é dado já público) → **[Dupla a definir]**, prazo **[Prazo a definir]**.

**[NOVA]**
**Pergunta:** O produto será transparente/explicável para o usuário (mostrando por que uma música recebeu determinado score) ou funcionará como caixa-preta — e isso afeta a confiança e o uso responsável da ferramenta?
**Ação:** Garantir que a interface exponha explicações (ex: feature importance/SHAP) junto ao score, não apenas um número (Scikit-learn/SHAP + design de produto) → **[Dupla a definir]**, prazo **[Prazo a definir]**.
