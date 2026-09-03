# Interface do Hit Maker

## 1. Visão geral

O Hit Maker é um protótipo de inteligência musical desenvolvido para apoiar artistas, produtores, gravadoras, curadores e profissionais de A&R na análise e priorização de músicas.

A interface permite representar as características de uma faixa, comparar sua estrutura com perfis musicais de referência e gerar um score demonstrativo.

Acesse a interface:

[Hit Maker — Music Intelligence](https://hit-maker-intelligence.dida0982.chatgpt.site/)

> O Hit Maker é uma ferramenta de apoio à decisão. O score representa similaridade com padrões musicais de referência e não garante o sucesso comercial de uma música.

---

## 2. Menu lateral

O menu lateral permite navegar entre as três áreas principais da aplicação:

### Analisar faixa

Área utilizada para informar as características de uma música e calcular seu score demonstrativo.

### Oportunidades

Apresenta um ranking de faixas que podem merecer atenção durante a análise de um catálogo musical.

### Como funciona

Explica as etapas utilizadas pelo produto e as limitações que precisam ser consideradas.

Na parte inferior do menu, a mensagem “Protótipo ativo — Resultados demonstrativos” informa que a interface representa uma demonstração do produto.

---

## 3. Analisar faixa

Esta é a principal área da aplicação. Nela, o usuário informa os dados e as características de uma música.

### Nome da faixa

Permite identificar a música que será analisada.

O nome não interfere no cálculo. Ele é utilizado somente para identificar a faixa na apresentação do resultado.

### Gênero musical

Permite selecionar o gênero da música:

* Pop;
* Rock;
* Hip-Hop;
* Eletrônica;
* Latina;
* Indie.

O gênero é importante porque os padrões musicais variam entre diferentes estilos. Uma faixa de rock, por exemplo, não deve ser comparada diretamente com os mesmos valores utilizados como referência para uma música eletrônica.

---

## 4. Características musicais

A interface permite ajustar nove características da música.

### Dançabilidade

Indica quanto a música é adequada para dança, considerando elementos como ritmo, estabilidade e regularidade das batidas.

### Energia

Representa a intensidade e a atividade percebida na música. Faixas rápidas, intensas e com maior volume normalmente apresentam energia elevada.

### Valência

Representa a sensação emocional transmitida pela música.

Valores elevados estão associados a atmosferas mais positivas ou alegres. Valores menores estão associados a atmosferas mais introspectivas, tensas ou melancólicas.

### Tempo — BPM

Indica a velocidade estimada da música em batidas por minuto.

### Loudness

Representa a intensidade sonora da gravação.

### Acústica

Indica a presença de características acústicas na música.

### Instrumentalidade

Representa a possibilidade de a faixa possuir pouco ou nenhum conteúdo vocal.

### Presença ao vivo

Indica a possibilidade de a gravação ter sido realizada na presença de público.

### Fala

Representa a quantidade de palavras faladas identificadas na faixa.

Valores elevados podem estar associados a rap, discursos, entrevistas, podcasts ou outras formas de conteúdo falado.

---

## 5. Botão “Calcular potencial”

Depois de preencher as informações, o usuário pode clicar em “Calcular potencial”.

Na versão demonstrativa, a interface:

1. captura os valores informados;
2. identifica o gênero selecionado;
3. consulta um perfil de referência do gênero;
4. calcula a proximidade entre a faixa e esse perfil;
5. gera um score entre 0 e 100;
6. apresenta os fatores com maior aderência;
7. sugere uma próxima ação.

Atualmente, esse cálculo é realizado diretamente no navegador e ainda não está conectado ao modelo de Machine Learning treinado nos notebooks.

Em uma versão de produção, a interface deverá enviar os dados para uma API responsável por executar o modelo treinado.

---

## 6. Score de potencial

O resultado é apresentado como um score entre 0 e 100.

Exemplo:

```text
82/100 — Alta aderência
```

O score representa o nível de proximidade entre as características informadas e o perfil de referência do gênero.

Ele não deve ser interpretado como:

```text
82% de chance de a música virar um hit.
```

A interpretação correta é:

```text
A faixa recebeu 82 pontos de similaridade com o perfil musical utilizado como referência.
```

A interface divide o resultado em três categorias:

* alta aderência: score igual ou superior a 80;
* aderência moderada: score entre 65 e 79;
* baixa aderência: score inferior a 65.

---

## 7. Contribuição por característica

A seção “Contribuição por característica” explica quais elementos da música ficaram mais próximos do perfil do gênero.

As barras podem apresentar resultados como:

```text
Energia: 98%
Dançabilidade: 95%
Valência: 91%
Tempo: 88%
```

Esses valores não medem a qualidade artística da música. Eles representam somente a proximidade entre cada característica informada e o perfil demonstrativo do gênero.

Essa explicação evita que o usuário receba apenas um score sem compreender sua composição.

---

## 8. Próxima ação

Depois de calcular o score, o sistema apresenta uma sugestão.

Dependendo do resultado, a interface pode recomendar:

* priorizar uma escuta editorial;
* testar a música com uma amostra de público;
* investigar as características com menor aderência;
* avaliar melhor o posicionamento de gênero;
* manter a faixa em observação.

A recomendação não substitui a avaliação realizada por artistas, produtores ou profissionais de A&R.

---

## 9. Restaurar exemplo

O botão “Restaurar exemplo” devolve todos os campos aos valores iniciais da demonstração.

Essa função permite repetir a apresentação sem precisar configurar manualmente todos os controles.

---

## 10. Oportunidades

A área “Oportunidades” simula uma ferramenta de triagem de catálogos musicais.

O objetivo é ajudar profissionais de A&R a identificar quais músicas devem ser analisadas primeiro.

A&R significa Artists and Repertoire. É a área responsável por procurar artistas, analisar músicas e auxiliar nas decisões de investimento e desenvolvimento artístico.

---

## 11. Ranking de faixas

A tabela apresenta as seguintes informações:

### Posição

Indica a colocação da música no ranking demonstrativo.

### Faixa

Apresenta o nome e o identificador da música.

### Artista

Mostra o responsável pela faixa.

### Gênero

Informa o gênero utilizado como contexto da análise.

### Popularidade atual

Representa a situação atual da música no mercado ou na plataforma.

### Score

Apresenta a similaridade calculada pelo sistema.

### Leitura

Transforma o score em uma interpretação mais simples:

* alta oportunidade;
* investigar;
* observar.

Uma situação relevante para profissionais de A&R seria:

```text
Popularidade atual baixa + score elevado
```

Essa combinação pode indicar uma faixa subestimada que merece investigação adicional.

Os nomes, artistas e resultados apresentados atualmente na tabela são dados fictícios utilizados exclusivamente para demonstrar a interface.

---

## 12. Filtro de gênero

O filtro permite visualizar somente as músicas de determinado gênero.

Isso facilita a análise de um catálogo e reduz comparações entre estilos musicais com estruturas muito diferentes.

---

## 13. Como funciona

A área “Como funciona” apresenta quatro etapas do Hit Maker.

### 1. Validar

O sistema verifica se os dados e características da faixa estão preenchidos corretamente.

Em uma versão completa, essa etapa também deverá identificar:

* valores ausentes;
* valores fora dos limites;
* tipos de dados incorretos;
* possíveis registros duplicados.

### 2. Comparar

A faixa é comparada com padrões identificados em músicas populares do mesmo gênero.

O projeto considera combinações de características porque nenhuma variável musical isolada é suficiente para explicar a popularidade.

### 3. Explicar

O sistema apresenta os fatores que contribuíram positiva ou negativamente para o score.

Essa etapa aumenta a transparência e reduz o funcionamento do modelo como uma caixa-preta.

### 4. Priorizar

O resultado é transformado em uma próxima ação de investigação.

O objetivo não é decidir automaticamente se uma música deve receber investimento, mas reduzir o número de faixas que precisam ser analisadas manualmente.

---

## 14. Limitações do protótipo

### Validação independente

O modelo precisa ser testado com músicas que não participaram do treinamento.

Caso os mesmos dados sejam utilizados para treinar e avaliar o modelo, pode acontecer vazamento de dados, também chamado de data leakage.

### Contexto por gênero

Um perfil de referência somente deve ser apresentado como confiável quando houver uma quantidade suficiente de músicas daquele gênero.

Os requisitos atuais sugerem um mínimo de 30 músicas classificadas como hits para gerar um perfil por gênero.

### Uso responsável

O score não deve ser utilizado sozinho para:

* contratar ou dispensar um artista;
* definir investimentos;
* decidir se uma música será lançada;
* interromper um projeto musical;
* substituir avaliações artísticas e comerciais.

O Hit Maker deve funcionar como ferramenta de apoio à decisão.

---

## 15. Situação atual

| Componente                          | Situação               |
| ----------------------------------- | ---------------------- |
| Navegação entre as páginas          | Funcional              |
| Controles das características       | Funcionais             |
| Cálculo demonstrativo do score      | Funcional              |
| Comparação demonstrativa por gênero | Funcional              |
| Filtro do ranking                   | Funcional              |
| Design responsivo                   | Implementado           |
| Dados do ranking                    | Fictícios              |
| Conexão com o modelo treinado       | Ainda não implementada |
| API do modelo                       | Ainda não implementada |
| Banco de dados                      | Ainda não implementado |
| Login de usuários                   | Ainda não implementado |
| Integração com o Spotify            | Ainda não implementada |

---

## 16. Objetivo da demonstração

A interface foi desenvolvida para demonstrar como os resultados das análises e do modelo podem ser transformados em um produto compreensível.

Ela permite apresentar:

* a proposta de valor;
* o fluxo de utilização;
* o score de potencial;
* a explicabilidade do resultado;
* a comparação por gênero;
* o ranking de oportunidades;
* as limitações e o uso responsável da solução.

O próximo passo técnico é conectar a interface ao pipeline real de Machine Learning por meio de uma API.
