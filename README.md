# Classificação de Poses de Yoga com OpenCV
Este projeto apresenta uma **prova de conceito (POC)** para classificação de poses de yoga a partir de imagens estáticas, utilizando técnicas clássicas de **Processamento Digital de Imagens**, **extração de características visuais** e **aprendizado de máquina**.
---
## Objetivo
O objetivo do projeto é desenvolver uma metodologia capaz de identificar poses de yoga em imagens, tratando o problema como uma tarefa de **classificação multiclasse**.
---
## Visão geral da solução
O pipeline desenvolvido segue uma abordagem clássica de visão computacional:
```text
Imagem de entrada
→ Leitura do dataset
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

A etapa mais importante do pipeline é a extração de características com HOG, pois ela transforma a imagem em um vetor numérico baseado nas orientações dos gradientes, capturando informações relacionadas à forma, bordas e estrutura corporal da pose.

⸻

Como instalar e rodar

1. Criar ambiente virtual

macOS / Linux

python3 -m venv .venv
source .venv/bin/activate

Windows PowerShell

python -m venv .venv
.venv\Scripts\Activate.ps1

⸻

2. Instalar as dependências

pip install --upgrade pip
pip install -r requirements.txt

⸻

3. Registrar o kernel no Jupyter

python -m ipykernel install --user --name yoga-t1 --display-name "Python (yoga-t1)"

Esse comando registra o ambiente virtual como um kernel do Jupyter, garantindo que o notebook utilize o Python e os pacotes instalados no ambiente do projeto.

⸻

Dependências

O arquivo requirements.txt deve conter:

opencv-python
numpy
pandas
matplotlib
seaborn
scikit-learn
jupyter
ipykernel
joblib

⸻

Estrutura do notebook

O notebook pipeline.ipynb está organizado conforme as etapas principais do pipeline:

1. Aquisição da imagem

Nesta etapa, o dataset de poses de yoga é carregado a partir da pasta dataset. As imagens utilizadas no projeto estão organizadas em subpastas, em que cada subpasta representa uma classe de pose. Essa organização permite que o próprio nome da pasta seja utilizado como rótulo da imagem.

2. Leitura e organização dos dados

O notebook percorre automaticamente as pastas do dataset, identifica as classes disponíveis e armazena os caminhos das imagens junto com seus respectivos rótulos. Essa etapa transforma a organização em pastas em uma estrutura tabular, facilitando as próximas fases do pipeline.

3. Padronização espacial

Como as imagens possuem tamanhos e proporções diferentes, todas são redimensionadas para um tamanho fixo. Para evitar distorções na pose, é utilizado padding, preservando a proporção original da imagem e preenchendo as áreas restantes. Isso garante que todas as imagens tenham a mesma dimensão antes da extração de features.

4. Transformação para Grayscale

As imagens coloridas são convertidas para escala de cinza. Essa transformação reduz a quantidade de informação processada, pois elimina os canais de cor e mantém apenas a intensidade dos pixels. Essa etapa é adequada para o uso do HOG, que trabalha principalmente com variações de intensidade e gradientes da imagem.

5. Filtragem passa-baixa

É aplicado um filtro Gaussiano leve para suavizar a imagem e reduzir pequenos ruídos. Como o HOG depende de bordas e gradientes, a filtragem é usada com cuidado para não remover informações importantes da estrutura da pose. O objetivo é tornar a imagem mais estável antes da extração das características.

6. Extração de features com HOG

Nesta etapa, cada imagem é transformada em um vetor numérico usando o descritor HOG. O HOG analisa a distribuição das orientações dos gradientes da imagem, capturando informações relacionadas a bordas, contornos, inclinação do corpo e posição geral dos membros. Essas features são a principal representação usada pelo modelo para diferenciar as poses.

7. Treinamento do modelo

Após a extração das features, os dados são separados em treino e teste. As features são padronizadas com StandardScaler, garantindo que todas tenham escala semelhante. Em seguida, um classificador SVM é treinado com os vetores extraídos das imagens, aprendendo padrões associados a cada classe de pose.

8. Avaliação do modelo

O desempenho do modelo é avaliado no conjunto de teste, utilizando métricas como acurácia, precisão, recall e F1-score. Também é gerada uma matriz de confusão para visualizar quais poses foram classificadas corretamente e quais foram confundidas entre si. Essa etapa permite analisar tanto o desempenho geral quanto os principais erros do modelo.

9. Demo

Por fim, o notebook apresenta uma demonstração com uma nova imagem. Essa imagem passa pelas mesmas etapas do pipeline: padronização espacial, conversão para grayscale, filtragem passa-baixa e extração de features com HOG. Depois disso, o modelo treinado realiza a predição da classe, demonstrando como a metodologia pode ser aplicada a imagens externas.