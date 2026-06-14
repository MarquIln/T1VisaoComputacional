# Classificação de Poses de Yoga com Visão Computacional

## Grupo

Ana Carolina Poletto  
Bárbara Dapper  
Marcos Vinicius Raach  

## Objetivo do trabalho

Este projeto tem como objetivo desenvolver uma prova de conceito para **classificação automática de poses de yoga a partir de imagens estáticas**, utilizando técnicas de Visão Computacional e Aprendizado de Máquina.

O trabalho foi desenvolvido em duas partes. Na primeira, foi construído um pipeline clássico com OpenCV, extração de features com HOG e classificação com SVM. Na segunda, foi criada uma continuação do pipeline utilizando uma abordagem baseada em rede neural, com MediaPipe para extração de landmarks corporais, cálculo de ângulos e classificação com uma MLP.

## Como criar e usar o ambiente virtual

### 1. Criar o ambiente virtual

No macOS/Linux:

```bash
python3 -m venv .venv
source .venv/bin/activate
```

No Windows PowerShell:

```bash
python -m venv .venv
.venv\Scripts\Activate.ps1
```

### 2. Instalar as dependências

Com o ambiente virtual ativado, execute:

```bash
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

### 3. Registrar o kernel no Jupyter

```bash
python -m ipykernel install --user --name yoga-visao --display-name "Python (yoga-visao)"
```

Depois, ao abrir os notebooks, selecione o kernel:

```text
Python (yoga-visao)
```

## Notebooks do projeto

### `pipelineTeste.ipynb`

Notebook utilizado para testar técnicas experimentais durante o desenvolvimento do trabalho.

Esse pipeline serviu para avaliar alternativas de pré-processamento e extração de características que, ao final, não foram mantidas na solução principal. Ele foi importante para comparação e escolha das etapas mais adequadas, mas não representa o pipeline final adotado.

### `pipelineFinal.ipynb`

Notebook referente à Parte 1 do projeto.

Ele contém o pipeline clássico final, composto pelas seguintes etapas:

```text
Leitura do dataset
→ Redimensionamento com padding
→ Conversão para escala de cinza
→ Suavização com filtro Gaussiano
→ Extração de features com HOG
→ Construção da base de treino
→ Separação treino/teste
→ Padronização das features
→ Treinamento com SVM
→ Predição
→ Avaliação dos resultados
```

Esse pipeline utiliza técnicas clássicas de processamento de imagens e aprendizado de máquina para classificar as poses de yoga.

### `pipelineRedeNeural_MediaPipe_MLP_corrigido.ipynb`

Notebook referente à Parte 2 do projeto.

Ele dá continuidade ao trabalho anterior utilizando uma abordagem mais adequada para classificação de poses corporais. O pipeline mantém o uso de OpenCV no pré-processamento e utiliza MediaPipe para extrair pontos do corpo, que são transformados em features geométricas, como coordenadas e ângulos articulares.

O fluxo principal é:

```text
Imagem
→ Pré-processamento com OpenCV
→ Extração de landmarks corporais com MediaPipe
→ Cálculo de features geométricas e ângulos
→ Treinamento de uma rede neural MLP
→ Predição
→ Avaliação dos resultados
```

Essa abordagem busca representar melhor a estrutura corporal das poses, utilizando informações sobre alinhamento, postura, articulações e ângulos do corpo.
