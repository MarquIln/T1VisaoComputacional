# Classificação de Poses de Yoga com OpenCV

Este projeto apresenta uma **prova de conceito (POC)** para **classificação de poses de yoga a partir de imagens estáticas**, utilizando técnicas clássicas de **processamento de imagens**, **extração de features** e **aprendizado de máquina**.

A proposta foi desenvolvida para atender aos requisitos do trabalho da disciplina, incluindo:
- uso de **OpenCV**
- uso de **features extraídas das imagens**
- construção de um **pipeline explicável**
- avaliação quantitativa dos resultados

---

## Objetivo

Desenvolver uma metodologia capaz de identificar poses de yoga em imagens, tratando o problema como uma tarefa de **classificação multiclasse**, em que cada imagem pertence a uma classe correspondente a uma postura específica.

---

## Estrutura do projeto

```text
seu_projeto/
├── pipeline.ipynb
├── requirements.txt
└── dataset/
    ├── classe_1/
    ├── classe_2/
    └── ...
```

### Organização esperada do dataset

O notebook assume que existe uma pasta chamada `dataset` no mesmo nível do arquivo `pipeline.ipynb`.

Cada subpasta dentro de `dataset` representa uma classe, por exemplo:

```text
dataset/
├── postura_1/
│   ├── img1.jpg
│   ├── img2.jpg
│   └── ...
├── postura_2/
│   ├── img1.jpg
│   ├── img2.jpg
│   └── ...
```

---

## Como instalar e rodar

### 1. Criar ambiente virtual

#### macOS / Linux

```bash
python3 -m venv .venv
source .venv/bin/activate
```

#### Windows (PowerShell)

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
```

---

### 2. Instalar dependências

```bash
pip install --upgrade pip
pip install -r requirements.txt
python -m ipykernel install --user --name yoga-t1 --display-name "Python (yoga-t1)"
```

Esse último comando registra o ambiente virtual como um **kernel do Jupyter**, para garantir que o notebook use exatamente o Python e os pacotes instalados nesse ambiente.

---

### 3. Rodar o notebook

```bash
jupyter notebook
```

Depois, abra o arquivo:

```text
pipeline.ipynb
```

No Jupyter, selecione o kernel:

```text
Python (yoga-t1)
```

---

## Dependências

O arquivo `requirements.txt` deve conter:

```txt
opencv-python
numpy
pandas
matplotlib
seaborn
scikit-image
scikit-learn
tqdm
joblib
jupyter
ipykernel
```

---

## Estrutura do notebook

O notebook `pipeline.ipynb` está organizado em seções que acompanham o pipeline completo do trabalho:

1. **Definição do problema**  
2. **Aquisição do dataset**  
3. **Leitura e organização dos dados**  
4. **Análise exploratória inicial**  
5. **Pré-processamento das imagens**  
6. **Extração de features**  
7. **Construção da base de treino**  
8. **Separação em treino e teste**  
9. **Normalização dos dados**  
10. **Treinamento do classificador**  
11. **Predição**  
12. **Avaliação dos resultados**  
13. **Análise qualitativa dos erros**  
14. **Inferência em novas imagens**  
15. **Salvamento do modelo**  
16. **Conclusão, limitações e melhorias**

---

## Pipeline do trabalho

A metodologia proposta segue o fluxo abaixo:

**Imagem de entrada**  
→ **Leitura do dataset**  
→ **Pré-processamento com OpenCV**  
→ **Extração de features**  
→ **Construção do vetor de características**  
→ **Divisão treino/teste**  
→ **Padronização das features**  
→ **Treinamento com SVM**  
→ **Predição**  
→ **Avaliação com métricas e matriz de confusão**  
→ **Análise dos erros**

---

## Descrição detalhada da pipeline

### 1. Definição do problema

O problema proposto consiste em desenvolver uma metodologia de processamento de imagens capaz de **identificar ou classificar poses de yoga a partir de imagens estáticas**. A proposta do trabalho é construir uma **prova de conceito (POC)** utilizando técnicas clássicas de visão computacional, com foco em um pipeline interpretável e compatível com os requisitos da disciplina.

A tarefa é tratada como um problema de **classificação multiclasse**, em que cada imagem pertence a uma classe correspondente a uma postura de yoga específica.

---

### 2. Aquisição do dataset

O conjunto de dados utilizado é o **Yoga Posture Dataset**, composto por imagens organizadas em pastas. Cada subpasta representa uma classe, isto é, uma pose de yoga.

Cada imagem representa uma amostra visual de determinada pose. Como as classes apresentam quantidades diferentes de imagens, o conjunto é naturalmente **desbalanceado**, o que deve ser considerado na avaliação dos resultados.

Além disso, o dataset pode conter variações de:
- iluminação
- fundo
- escala da pessoa na imagem
- ângulo de captura
- posição da câmera
- roupas
- semelhança visual entre poses

Esses fatores tornam a tarefa mais desafiadora e justificam a necessidade de pré-processamento e extração de características.

---

### 3. Leitura e organização dos dados

O sistema percorre automaticamente a pasta raiz do dataset, identifica as subpastas e interpreta cada uma delas como uma classe.

Em seguida:
- os caminhos das imagens são armazenados
- os rótulos das classes são associados a cada imagem
- é realizada uma contagem do número de imagens por classe
- são exibidas amostras visuais para inspeção inicial

Essa etapa é importante para compreender a distribuição dos dados e detectar problemas como:
- classes com poucas imagens
- arquivos corrompidos
- nomes inconsistentes
- imagens fora do padrão esperado

---

### 4. Análise exploratória inicial

Antes do treinamento, é realizada uma análise exploratória para entender melhor a base utilizada e apoiar decisões sobre o restante do pipeline.

A análise inclui:
- número total de classes
- número de imagens por classe
- visualização de amostras de cada classe
- identificação de classes com desbalanceamento
- observação qualitativa de diferenças e semelhanças entre poses

---

### 5. Pré-processamento das imagens

Cada imagem passa por uma etapa de pré-processamento para reduzir ruídos, padronizar a entrada e facilitar a extração das features.

As etapas adotadas incluem:

#### 5.1 Redimensionamento
Todas as imagens são redimensionadas para um tamanho fixo, como **128 x 128 pixels**.

#### 5.2 Conversão para escala de cinza
As imagens são convertidas para tons de cinza, reduzindo a complexidade dos dados.

#### 5.3 Suavização com filtro Gaussiano
Aplica-se um filtro Gaussiano para reduzir ruídos de alta frequência.

#### 5.4 Realce de contraste com CLAHE
Utiliza-se **CLAHE** para melhorar o contraste local da imagem.

#### 5.5 Binarização automática com Otsu
A imagem é binarizada usando o método de Otsu.

#### 5.6 Operações morfológicas
São aplicadas operações como:
- abertura
- fechamento

Essas operações ajudam a remover ruído e melhorar a definição das regiões de interesse.

---

### 6. Extração de features

A extração de features transforma cada imagem em um vetor numérico que pode ser utilizado por algoritmos de aprendizado de máquina.

Neste trabalho, são utilizadas **features clássicas de visão computacional**, tornando a abordagem mais interpretável.

#### 6.1 HOG (Histogram of Oriented Gradients)
Captura a distribuição das orientações dos gradientes da imagem, representando padrões locais de bordas e estrutura.

É útil para descrever diferenças relacionadas a:
- alinhamento dos membros
- abertura de braços e pernas
- inclinação do tronco
- contorno geral da postura

#### 6.2 Momentos de Hu
Descritores matemáticos relacionados à forma do objeto presente na imagem, com robustez a escala, translação e rotação.

#### 6.3 Features geométricas de contorno
A partir da imagem binarizada, extrai-se o maior contorno e calculam-se características como:
- área relativa
- perímetro relativo
- largura e altura normalizadas
- razão entre largura e altura
- extent
- solidity
- posição do contorno na imagem
- tamanho do contorno

#### 6.4 Densidade de bordas com Canny
A detecção de bordas com **Canny** é utilizada para estimar a densidade de bordas da imagem.

#### 6.5 Vetor final de características
Todas as features são concatenadas em um único vetor numérico para cada imagem.

---

### 7. Construção da base de treino

Depois da extração de features, são construídas três estruturas principais:
- **X**: matriz de características
- **y**: vetor de rótulos
- **paths**: caminhos das imagens

Essa etapa transforma o problema de classificação de imagens em um problema de classificação supervisionada em espaço vetorial.

---

### 8. Separação em treino e teste

A base é dividida em dois subconjuntos:
- **treinamento**
- **teste**

A divisão é feita de forma **estratificada**, preservando a proporção das classes.

Exemplo:
- 80% das imagens para treino
- 20% das imagens para teste

---

### 9. Normalização dos dados

Antes do treinamento, aplica-se **padronização das features** com `StandardScaler`.

Essa etapa ajusta cada feature para média próxima de zero e desvio padrão unitário, evitando que atributos com escalas maiores dominem o modelo.

---

### 10. Treinamento do classificador

Como baseline, utiliza-se um **SVM (Support Vector Machine)** com kernel RBF.

A escolha se justifica porque:
- funciona bem com dados tabulares
- apresenta bom desempenho com múltiplas features
- é eficiente em bases de tamanho moderado
- é adequado para POCs com features manuais

---

### 11. Predição

Após o treinamento, o modelo é utilizado para prever as classes das imagens do conjunto de teste.

Cada imagem recebe uma classe prevista, que é comparada ao rótulo real.

---

### 12. Avaliação dos resultados

A avaliação é feita com métricas clássicas de classificação:

#### 12.1 Acurácia
Proporção total de imagens classificadas corretamente.

#### 12.2 Precisão
Quantas previsões positivas estavam corretas.

#### 12.3 Recall
Quantas amostras reais de cada classe foram corretamente identificadas.

#### 12.4 F1-score
Combinação entre precisão e recall.

#### 12.5 Matriz de confusão
Permite visualizar:
- quais classes são mais confundidas
- quais poses são melhor reconhecidas
- quais erros ocorrem com maior frequência

