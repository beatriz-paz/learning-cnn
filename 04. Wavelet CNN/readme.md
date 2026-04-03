# <p align="center">WaveletCNN para Classificação de Imagens Mamográficas</p>

Este projeto investiga o uso de **transformadas wavelet integradas ao pipeline de CNNs** para melhorar a extração de características em imagens mamográficas.

A proposta central é substituir operações tradicionais como **MaxPooling** por **decomposições wavelet**, explorando representações no domínio da frequência.

<p align="center">
<i>
Validação Cruzada Estratificada — 5-Fold <br>
10 Modelos Avaliados | Métricas: Acc, F1-Score, AUC-ROC
</i>i>
</p>

---

# Métricas de Avaliação

As seguintes métricas foram utilizadas para avaliar o desempenho dos modelos:

| Métrica   | O que mede                                   | Quando usar                  |
|----------|----------------------------------------------|-------------------------------|
| Acurácia | Proporção de acertos gerais                  | Dados balanceados             |
| Precisão | Confiabilidade das predições positivas       | Evitar falsos positivos       |
| Recall   | Capacidade de encontrar positivos reais      | Evitar falsos negativos       |
| F1-score | Equilíbrio entre precisão e recall           | Dados desbalanceados          |
| AUC-ROC  | Capacidade de separar as classes             | Avaliação global do modelo    |

### Definições

- **Acurácia**  

  $\text{acc} = \frac{\text{acertos}}{\text{total de amostras}}$

- **F1-score**  
  Mede o quão bem o modelo:
  - evita falsos positivos
  - não deixa passar positivos reais  

- **AUC-ROC**  
  Mede a capacidade do modelo de **distinguir entre classes** ao variar o limiar de decisão.

---

# Experimentos

Todos os modelos que utilizam wavelet foram implementados com **filtros treináveis**.

Os experimentos investigam o impacto da incorporação de transformadas wavelet como mecanismo de pooling, bem como variações nas estratégias de detecção de região de interesse (ROI) e realce de características via mecanismo de atenção CBAM.

## Resultados dos Experimentos — Comparação de Modelos

| Modelo | Descrição | Acc (%) ± σ | F1 ± σ | AUC ± σ |
|--------|-----------|:-----------:|:------:|:-------:|
| A | CNN Baseline simples | 55.31 ± 9.25 | 0.4991 ± 0.0823 | 0.4923 ± 0.0217 |
| B | CNN Baseline apenas com CBAM | 51.25 ± 8.75 | 0.5073 ± 0.0773 | 0.4443 ± 0.0944 |
| C | CNN Baseline apenas com ROI | 90.94 ± 1.53 | 0.9074 ± 0.0156 | 0.9230 ± 0.0176 |
| D | CNN Baseline com ROI + CBAM | 91.88 ± 2.30 | 0.9176 ± 0.0231 | 0.9282 ± 0.0280 |
| E | Wavelet Haar como Pooling | 91.25 ± 1.25 | 0.9110 ± 0.0134 | 0.9145 ± 0.0378 |
| F | Wavelet Biorthogonal 1.3 como Pooling | 89.69 ± 2.12 | 0.8961 ± 0.0207 | 0.9225 ± 0.0245 |
| G | Wavelet Daubechies (db4) como Pooling | 92.81 ± 0.77 | 0.9265 ± 0.0083 | 0.9129 ± 0.0321 |
| H | Wavelet Coiflet como Pooling | 92.50 ± 1.82 | 0.9235 ± 0.0189 | 0.9109 ± 0.0349 |
| I | Wavelet Híbrida (haar, db4, bior, coiflet) como Pooling | 90.62 ± 1.98 | 0.9053 ± 0.0203 | 0.9228 ± 0.0262 |
| **J** | **CNN Dual Branch — ResNet + Wavelet Coiflet** | **94.06 ± 1.53** | **0.9393 ± 0.0160** | **0.9363 ± 0.0365** |

> **Observações:**
> - Validação cruzada estratificada (5-Fold)
> - Resultados apresentados como média ± desvio padrão (σ)
> - Melhor modelo destacado em **negrito**
> - ROI: Region of Interest
> - CBAM: Convolutional Block Attention Module

---


# Modelos Avaliados

Os modelos estão organizados em três grupos conforme abaixo:

- Grupo I — Baselines: modelos sem wavelet, com variações de ROI e CBAM.
- Grupo II — Wavelet como Pooling: substituição do pooling tradicional por wavelets (ROI + CBAM).
- Grupo III — Modelos Avançados: arquiteturas mais complexas com wavelet híbrida ou dual branch.

## Grupo I — Modelos Baseline (sem Wavelet)

Os modelos baseline servem como referência comparativa. Avaliam o impacto isolado de ROI e CBAM sem a incorporação de wavelets.

### Modelo A: CNN Baseline simples
- CNN tradicional sem uso de wavelets.

**Resultado:**
Apresenta um baixo desempenho, comparada com outros modelos. Por mais que seja simples, a falta de técnicas para tratar apenas as partes importantes da imagem geram impactos na avaliação do modelo.


### Modelo B: CNN Baseline apenas com CBAM
- CNN apenas com técnica CBAM de realce.

**Resultado:**
O uso isolado do CBAM sem ROI também falha em superar o acaso. O alto desvio padrão (σ = 8.75) indica instabilidade entre os folds, sugerindo que o mecanismo de atenção depende de uma região de entrada mais focada para ser efetivo.


### Modelo C: CNN Baseline apenas com ROI
- Modelo CNN apenas com ROI.

**Resultado:**
A aplicação do ROI na imagem gera um resultado expressivo, pois evidência que a CNN consegue ser melhor utilizada quando recebe para seu treino apenas parte de interesse da imagem mamogŕafica.


### Modelo D: CNN Baseline com ROI + CBAM
- Modelo de CNN que combina as duas técnicas, de foco (ROI) e realce (CBAM).

**Resultado:**
A combinação de ROI e CBAM estabelece o **baseline principal do trabalho**. Apresenta a maior AUC entre os modelos sem wavelet (0.9282), servindo como referência para avaliar o ganho das arquiteturas com wavelet.

---

## Grupo II — Wavelet como Pooling (ROI + CBAM)

Todos os modelos deste grupo substituem o pooling tradicional por uma transformada wavelet, mantendo ROI e CBAM. O objetivo é identificar qual família wavelet oferece maior ganho de representação.

### Modelo E: Wavelet Haar como Pooling
- Haar: Rápida, quadrada, para bordas secas.

**Resultado:**
A wavelet Haar apresenta desempenho ligeiramente inferior ao baseline (Modelo D) em acurácia, porém com menor desvio padrão (σ = 1.25), indicando maior estabilidade. A AUC relativamente baixa (0.9145) sugere limitação da Haar para capturar variações finas de textura.


### Modelo F: Wavelet Biorthogonal 1.3 como Pooling
- Biorthogonal: Simétrica, perfeita para reconstrução de imagens.

**Resultado:**
A Biorthogonal 1.3 apresenta a menor acurácia entre os modelos com wavelet como pooling (89.69%). Contudo, sua AUC (0.9225) é comparável ao baseline, o que pode indicar boa discriminabilidade em detrimento de precisão ponto-a-ponto.


### Modelo G: Wavelet Daubechies db4 como Pooling
- Daubechies: Versátil, assimétrica, padrão da indústria.
  
**Resultado:**
A Daubechies db4 apresenta o menor desvio padrão de acurácia entre todos os modelos (σ = 0.77), indicando alta estabilidade. Com acurácia de 92.81% e F1 de 0.9265, supera o baseline em acurácia, sendo a família wavelet mais consistente como pooling isolado.

### Modelo H: Wavelet Coiflet como Pooling
- Coiflets: Mais simétricas que as dbN, focadas em precisão de aproximação.

**Resultado:**
A Coiflet apresenta desempenho próximo à Daubechies db4, com acurácia de 92.50% e F1 de 0.9235. O maior desvio padrão em relação à db4 indica menor estabilidade entre folds, mas ainda superior ao baseline.

---

## Grupo III — Modelos Avançados

Os modelos avançados exploram estratégias mais sofisticadas de integração wavelet, incluindo pipeline híbrido com múltiplas famílias e arquitetura dual branch com processamento paralelo.

### Modelo I: Wavelet Híbrida no Pipeline (Haar + db4 + Bior + Coiflet)
- Modelo que aplica diferentes wavelets no lugar do Max Pooling tradicial.

**Resultado:**
A combinação de múltiplas wavelets no pipeline não supera os melhores modelos individuais (G e H) em acurácia, sugerindo que a diversidade de famílias introduz ruído ao invés de complementaridade quando usadas sequencialmente. A AUC (0.9228) é, contudo, competitiva.

### Modelo J: CNN Dual Branch — ResNet + Wavelet Coiflet (ROI + CBAM)
- CNN dual branch, sendo um ramo aplicando transfer learning (ResNet) e o outro usando o modelo que já havíamos testado.

**Resultado:**
**O modelo Dual Branch é o melhor desempenho geral do estudo, com 94.06% de acurácia, F1 de 0.9393 e AUC de 0.9363. O processamento paralelo entre o branch ResNet e o branch Wavelet Coiflet permite capturar tanto características profundas quanto informações de frequência local, resultando em um modelo que se completa de maneira efetiva entre as duas representações.**

---

# Principais Constatações

**1. O ROI é o fator mais determinante:** ao comparar a baseline mais simples com outra aplicando a mais o ROI na imagem já tem-se uma salto de eficiência, o que evidencia que restringir a entrada da rede neural à região mamográfica de interesse é condição fundamental para o aprendizado efetivo de características relevantes.

**2. O CBAM isolado não contribui:** agora se aplicar apenas a técnica de realce CBAM em uma imagem mamográfica inteira causa confusão e o modelo fica com baixo desempenho. Já ao ser combinado com ROI (Modelo D), houve ganho consistente em todas as métricas. Isso indica que o mecanismo de atenção só opera de forma significativa quando a entrada já está focada na região relevante.

**3. Wavelets como pooling superam o Max Pooling tradicional:** verificado que dependendo o a Wavelet mãe escolhida para substituição do tradicional Max Pooling pode sim melhorar o modelo de CNN. Os modelos que usam Daubechies db4 e Coiflet superaram o baseline com ROI + CBAM (Modelo D: 91.88%) em acurácia e F1. A substituição do pooling tradicional por transformadas wavelet com filtros treináveis se mostrou uma estratégia válida para preservar informações de frequência local que o Max Pooling pode descartar.

**4. A Daubechies db4 é a família wavelet mais estável como pooling isolado:** O Modelo G apresentou o menor desvio padrão de acurácia de todo o estudo (σ = 0.77) e acurácia de 92.81%, confirmando a eficiência da família Daubechies como referência na análise de sinais e imagens. A Coiflet (Modelo H) ficou próxima, mas com maior inconstância entre folds.

**5. A combinação sequencial de múltiplas wavelets não traz ganho adicional:** O Modelo I (wavelet híbrida com quatro famílias em pipeline) não superou os melhores modelos individuais, sugerindo que a diversidade sequencial de famílias introduz ruído em vez de correlação. A variedades de filtros, nesse caso, não se traduziu em maior capacidade classificatória.

**6. A arquitetura Dual Branch é o modelo mais eficaz:** O Modelo J foi o melhor de todos, atingindo a maior precisão nos testes com 94,06% de acurácia. O grande diferencial foi usar dois caminhos que trabalham ao mesmo tempo, em vez de um após o outro. De um lado, a rede ResNet identifica formas e objetos complexos. Por outro lado, a Wavelet Coiflet foca nos detalhes de textura e padrões de frequência. Ao juntar essas duas visões diferentes no final, o modelo consegue entender a imagem de forma muito mais completa e equilibrada, tornando-se mais eficiente e confiável do que as outras versões.

---

# Conclusão

Os estudos mostraram que usar as Wavelets (uma técnica que analisa detalhes de frequência) ajuda muito a classificar imagens de mamografia, especialmente quando focamos apenas na área importante do exame. Simplesmente trocar o método comum de reduzir a imagem pelo uso de Wavelets já traz resultados melhores, pois essa técnica não descarta detalhes importantes de textura que outros métodos perder.

Porém, o grande destaque foi o Modelo J, que usa dois caminhos trabalhando juntos:

- Um caminho usa uma inteligência já treinada (ResNet) para identificar formas gerais.
- O outro caminho usa as Wavelets para focar especificamente nos padrões de frequência e detalhes minuciosos.

Em vez de um caminho vir depois do outro, eles trabalham lado a lado. Isso provou que esses dois tipos de informação se completam. O Modelo J não foi apenas o mais preciso, mas também o mais equilibrado e confiável em todos os testes, mostrando que não foi sorte.

Em resumo, o que gerou melhores resultados na classificação de imagens mamograficas foi: focar na região correta do exame (ROI), usar mecanismos de atenção (CBAM) e combinar a força das redes neurais modernas (ResNet) com a precisão dos detalhes das Wavelets em caminhos paralelos.

---

## Ambiente de Desenvolvimento

Os experimentos deste projeto foram executados em um notebook **Acer Nitro V 15**, com as seguintes especificações técnicas:

| Componente | Detalhes |
| :--- | :--- |
| **Processador (CPU)** | AMD Ryzen 7 7735HS (8 Cores / 16 Threads) |
| **Memória RAM** | 16 GB DDR5 4800 MT/s |
| **Placa de Vídeo (Dedicada)** | NVIDIA GeForce RTX 4050 Mobile (6GB VRAM) |
| **Placa de Vídeo (Integrada)** | AMD Radeon 680M |
| **Sistema Operacional** | Ubuntu 24.04.4 LTS |
| **Kernel** | Linux 6.17.0 |

### Stack Tecnológica
- **Linguagem:** Python
- **Drivers GPU:** NVIDIA Driver 535.288
- **Ambiente:** Linux (x86_64)

---
