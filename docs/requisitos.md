# Requisitos — Hit Maker (HitPredict + A&R Intelligence)


## Convenções usadas neste documento

- **RF (Requisito Funcional):** título no padrão *verbo no infinitivo + objeto* (ex.: "Calcular score de potencial"), com o comportamento detalhado logo abaixo.
- **RNF (Requisito Não Funcional):** classificado por **URPS** (Usabilidade, Confiabilidade, Performance, Suportabilidade) e descrito com uma métrica mensurável — nunca só uma intenção qualitativa.
- **RN (Regra de Negócio):** critério "duro" de dado ou de negócio (schema mínimo, faixa de valores válida, definição de duplicidade, formato do score etc.). RFs e RNFs **referenciam** RNs em vez de redefinir a regra a cada vez.

---

## 1. Regras de Negócio (RN)

Critérios de dado e de negócio que eram citados dentro do texto de RFs na versão anterior e agora ficam centralizados aqui — qualquer RF/RNF que dependa deles aponta para o número da RN, em vez de repetir a regra.

**RN01 — Completude mínima de um registro**
Um registro de música só é válido para análise se contiver: nome da faixa, artista, gênero, `danceability`, `energy`, `loudness`, `speechiness`, `acousticness`, `instrumentalness`, `liveness`, `valence`, `tempo`, duração e `explicit`.

**RN02 — Faixa de valores válida por feature**
Features normalizadas (`danceability`, `energy`, `speechiness`, `acousticness`, `instrumentalness`, `liveness`, `valence`) só são válidas no intervalo `[0, 1]`; `tempo` (BPM) e duração só são válidos se `> 0`.
> Conferido contra `archive/dataset.csv`: o dataset real **já contém faixas com `tempo = 0` e `duration_ms = 0`** — ou seja, esta regra, se aplicada hoje, reclassificaria registros hoje tratados como válidos. Vale rodar essa checagem antes de assumir o dataset como "limpo" (ver também `data_analysis/03_tratamento_limpeza_dados.ipynb`, que hoje trata nulos mas não esses valores fora do domínio esperado).

**RN03 — Definição de duplicidade**
Duas faixas são duplicatas se e somente se compartilharem o mesmo identificador de faixa (`track_id`) **ou** a mesma combinação (artista, nome da música). Duas faixas do mesmo artista com nomes diferentes **não** são duplicatas.

**RN04 — Limiar de "hit" / faixa de popularidade — PENDENTE DE VALIDAÇÃO**
A tabela de faixas por popularidade (0–24 Baixa, 25–49 Média, 50–74 Alta, 75–100 Muito alta) é **ilustrativa, não uma regra decidida** — o próprio levantamento original já registrava que "o ponto de corte deve ser definido após a análise exploratória, e não simplesmente assumido". Esta é a mesma decisão em aberto já documentada como **decisão de negócio nº 1** em [`data_analysis/02_requisitos.ipynb`](https://github.com/gpaulovit/TIC-IA-data_Spotify/blob/main/data_analysis/02_requisitos.ipynb) (threshold de hit — percentil 80 global usado como ponto de partida no pipeline atual, com alternativa por gênero disponível). Esta RN só deve ser fechada quando essa decisão for validada com o time.

**RN05 — Formato e semântica do score**
O score de potencial é um valor inteiro entre 0 e 100. Ele representa **similaridade com padrões observados nos dados de treino** — nunca deve ser exibido, comunicado ou documentado como garantia de sucesso comercial. Regra ligada ao risco ético de profecia autorrealizável já levantado em `docs/README.md` (seção Ética) e no módulo 02 do `data_analysis`.

**RN06 — Amostra mínima para perfil de referência por gênero**
Um perfil de músicas populares (RFM06) só é considerado estatisticamente confiável, e portanto exibível, se o gênero tiver no mínimo 30 faixas classificadas como hit. Abaixo disso, o sistema deve sinalizar a instabilidade em vez de apresentar o perfil como se fosse robusto.

---

## 2. Requisitos do Modelo

Requisitos sobre os dados de treino e sobre o comportamento do modelo preditivo em si — não são telas nem ações do usuário final.

**RFM01 — Importar dados de música para análise**
O sistema deverá permitir a inserção ou importação dos dados de uma música para análise, validando-os contra RN01.
*Origem: GQ03 — verificar se as informações necessárias estão completas.*

**RFM02 — Validar integridade dos dados importados**
O sistema deverá verificar a integridade dos dados antes de realizar a análise, identificando: valores ausentes, valores inválidos, tipos de dados incorretos e valores fora dos limites definidos em RN02.
*Origem: GQ03 e GQ06.*

**RFM03 — Detectar faixas duplicadas**
O sistema deverá identificar possíveis músicas duplicadas conforme o critério definido em RN03.
*Origem: GQ04.*

**RFM04 — Apresentar distribuição de faixas por gênero**
O sistema deverá apresentar a quantidade e a proporção de músicas por gênero, e indicar gêneros com quantidade insuficiente de registros para comparação (ver RN06 para o limite aplicado a perfis de referência).
*Origem: GQ01, GQ02 e GQ05.*

**RFM05 — Apresentar distribuição da popularidade**
O sistema deverá apresentar a distribuição da variável `popularity` (média, mediana, mínimo, máximo, quartis, concentração por faixa), servindo de base empírica para a decisão pendente em RN04 — não para assumi-la.
*Origem: GQ07.*

**RFM06 — Gerar perfil de músicas populares por gênero**
O sistema deverá identificar as características predominantes entre músicas classificadas como altamente populares, sujeito ao mínimo amostral de RN06.

```text
HIGH POPULARITY
Danceability    0.74
Energy          0.78
Valence         0.65
Loudness       -5.2
Acousticness    0.21
```

*Origem: GQ01.*

**RFM07 — Calcular score de potencial da faixa**
O sistema deverá calcular um score de 0 a 100 (formato definido em RN05) indicando o grau de similaridade da música analisada com o perfil de músicas populares do seu gênero.
*Origem: GQ09.*

---

## 3. Requisitos de Produto

Funcionalidades voltadas ao usuário final, construídas sobre a saída do modelo (seção 2). Subdivididas nos dois módulos de produto já nomeados no levantamento original: **HitPredict** (RFP01–RFP02) e **A&R Intelligence** (RFP03–RFP06).

### 3.1 HitPredict

**RFP01 — Explicar a composição do score**
O sistema deverá apresentar quais características contribuíram positivamente ou negativamente para o score (RFM07), respeitando a semântica de RN05.

```text
Seu Score: 82
✓ Danceability     Alta aderência
✓ Energy           Alta aderência
✓ Valence          Boa aderência
⚠️ Acousticness     Baixa aderência
⚠️ Instrumentalness Baixa aderência
```

Transforma o resultado de uma "caixa-preta" em uma recomendação compreensível.
*Origem: GQ02 e GQ09.*

**RFP02 — Comparar faixa com o perfil do gênero**
O sistema deverá comparar uma música com o perfil de popularidade do seu próprio gênero (RFM06) — a pergunta é "sua música se parece com os hits deste gênero?", não "sua música se parece com um hit do Spotify em geral".
*Origem: GQ03 e GQ06.*

### 3.2 A&R Intelligence

**RFP03 — Gerar ranking de músicas por score**
O sistema deverá gerar um ranking de músicas baseado no score de similaridade (RFM07) com o perfil de alta popularidade do gênero.

```text
RANKING DE OPORTUNIDADES
#   Música      Gênero       Score
1   Track A     Pop           94
2   Track B     Hip-Hop       91
3   Track C     Rock          89
```

*Origem: GQ05 e GQ09.*

**RFP04 — Identificar faixas subestimadas**
O sistema deverá identificar músicas de baixa popularidade que apresentam características semelhantes às músicas altamente populares do seu gênero.

```text
🚨 OPORTUNIDADE IDENTIFICADA
Track: Example Song      Popularidade atual: 31
Similaridade com hits do gênero: 87%      Gênero: Pop
```

*Origem: GQ05.*

**RFP05 — Gerar ranking de artistas por desempenho**
O sistema deverá permitir identificar artistas com maior concentração de músicas de alta popularidade ou maior score médio de potencial.
*Origem: GQ04.*

**RFP06 — Filtrar músicas por critérios de A&R**
O sistema deverá permitir filtrar músicas por gênero, faixa de popularidade, score, artista e demais características disponíveis (`energy`, `danceability`, `valence`, `explicit` etc.), transformando o produto em uma ferramenta de descoberta de oportunidades.
*Origem: GQ05 e GQ09 (mesma origem de RFP03/RFP04 — filtro é a interface de acesso ao mesmo ranking/lista de oportunidades).*

---

## 4. Requisitos Não Funcionais (RNF)

Nenhum RNF existia na versão anterior do documento. Proposta inicial — cada um classificado por **URPS** e com métrica mensurável, para o time ajustar os números conforme a realidade do projeto:

| # | RNF | URPS | Métrica mensurável | Aplica-se a |
|---|---|---|---|---|
| RNF01 | Poder preditivo do modelo | Confiabilidade | ROC-AUC ≥ 0,85 no conjunto de teste (pipeline atual já mede 0,89 — ver `data_analysis/04_ideacao_solucao.ipynb`) | RFM07 |
| RNF02 | Tempo de resposta do score | Performance | Score de 1 faixa calculado e retornado em ≤ 2s | RFM07, RFP01 |
| RNF03 | Volume de dados suportado | Performance | Processar bases de até 150.000 faixas sem degradação perceptível de desempenho (dataset atual tem 114.000) | RFM01–RFM06 |
| RNF04 | Compreensibilidade da explicação do score | Usabilidade | ≥ 80% dos usuários testados identificam corretamente o motivo do score em teste de usabilidade | RFP01 |
| RNF05 | Retreinamento do modelo | Suportabilidade | Modelo deve poder ser retreinado/atualizado sem exigir alteração na interface do produto | RFM06, RFM07 |
| RNF06 | Estabilidade do perfil de referência | Confiabilidade | Perfil de gênero só é exibido se RN06 for satisfeita; caso contrário, sistema exibe aviso de amostra insuficiente em vez do perfil | RFM06, RFP02 |
| RNF07 | Transparência sobre a limitação do score | Usabilidade | Disclaimer de RN05 ("não é garantia de sucesso") visível em 100% das telas que exibem o score | RFP01, RFP03, RFP04 |

---

## 5. Rastreabilidade (RF original → requisito novo)

| RF original | Requisito novo | RN relacionada |
|---|---|---|
| RF01 — Cadastro/importação de músicas | RFM01 — Importar dados de música para análise | RN01 |
| RF02 — Validação dos dados | RFM02 — Validar integridade dos dados importados | RN02 |
| RF03 — Detecção de duplicidades | RFM03 — Detectar faixas duplicadas | RN03 |
| RF04 — Análise da distribuição dos gêneros | RFM04 — Apresentar distribuição de faixas por gênero | RN06 |
| RF05 — Análise da distribuição da popularidade | RFM05 — Apresentar distribuição da popularidade | RN04 (pendente) |
| RF06 — Geração do perfil de músicas populares | RFM06 — Gerar perfil de músicas populares por gênero | RN06 |
| RF07 — Cálculo do Score de Potencial | RFM07 — Calcular score de potencial da faixa | RN05 |
| RF08 — Explicação do Score | RFP01 — Explicar a composição do score | RN05 |
| RF09 — Comparação com o gênero | RFP02 — Comparar faixa com o perfil do gênero | — |
| RF10 — Ranking de músicas | RFP03 — Gerar ranking de músicas por score | — |
| RF11 — Identificação de oportunidades | RFP04 — Identificar faixas subestimadas | — |
| RF12 — Ranking de artistas | RFP05 — Gerar ranking de artistas por desempenho | — |
| RF13 — Filtros para profissionais de A&R | RFP06 — Filtrar músicas por critérios de A&R | — |

---

