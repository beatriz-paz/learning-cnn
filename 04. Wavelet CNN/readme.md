# WaveletCNN para Classificação de Imagens Mamográficas

Este projeto investiga o uso de **transformadas wavelet integradas ao pipeline de CNNs** para melhorar a extração de características em imagens mamográficas.

A proposta central é substituir operações tradicionais como **MaxPooling** por **decomposições wavelet**, explorando representações no domínio da frequência.

---

# Métricas de Avaliação

As seguintes métricas foram utilizadas para avaliar o desempenho dos modelos:

| Métrica   | O que mede                                   | Quando usar                    |
|----------|----------------------------------------------|-------------------------------|
| Acurácia | Proporção de acertos gerais                  | Dados balanceados             |
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

Todos os modelos que utilizam wavelet foram implementados com **filtros treináveis (learnable)**.

## Resultados

| Modelo | Acc (%) ± σ | F1 ± σ | AUC ± σ |
|------|------------|--------|--------|
| A: Baseline (sem wavelet) | 91.88% ± 2.30 | 0.9176 ± 0.0231 | 0.9282 ± 0.0280 |
| B: Wavelet Haar como Pooling | 91.25 ± 1.25 | 0.9110 ± 0.0134 | 0.9145 ± 0.0378 |
| C: Wavelet Bior 1.3 como Pooling | 89.69 ± 2.12 | 0.8961 ± 0.0207 | 0.9225 ± 0.0245 |
| D: Wavelet dB4 como Pooling | 91.88 ± 2.30 | 0.9176 ± 0.0232 | 0.9093 ± 0.0381 |
| E: Wavelet Coiflet como Pooling | 92.50% ± 1.82 | 0.9235 ± 0.0189 | 0.9109 ± 0.0349 |
| F: Wavelet Híbrida no Pipeline | 90.62% ± 1.98 | 0.9053 ± 0.0203 | 0.9228 ± 0.0262 |
| G: Wavelet Dual-Branch | 93.75% ± 1.71 | 0.9360 ± 0.0179 | 0.9345 ± 0.0299 |

---

# Modelos Avaliados

### 🔹 A — Baseline
- CNN tradicional sem uso de wavelets.

**Resultado:**
Apresenta desempenho consistente e competitivo, estabelecendo um forte baseline para comparação com abordagens baseadas em wavelets.
---

### 🔹 B — Wavelet Haar
- Substitui pooling por decomposição Haar  
- Foco em bordas e descontinuidades  

**Resultado:**
Leve degradação em todas as métricas em relação ao baseline, sugerindo que a simplicidade da Haar pode limitar a representação de padrões mais complexos.
---

### 🔹 C — Wavelet Biorthogonal (1.3)
- Propriedade de simetria e melhor preservação estrutural
- Adequada para reconstrução de sinais

**Resultado:**
Apesar de apresentar menor acurácia e F1-score, obteve AUC elevada, indicando boa capacidade de separação entre classes, mesmo com limiar de decisão fixo subótimo.
---

### 🔹 D — Wavelet Daubechies (db4)
- Captura eficiente de padrões locais e texturas
- Maior suporte e complexidade em relação à Haar

**Resultado:**
Desempenho equivalente ao baseline em acurácia e F1, porém com leve queda em AUC, indicando que a melhoria na representação local nem sempre se traduz em melhor separabilidade global.
---

### 🔹 E — Wavelet Coiflet
- Equilíbrio entre localização temporal e frequência
- Boa capacidade de representação multi-escala

**Resultado:**
Melhor desempenho entre os modelos de única wavelet, superando o baseline em acurácia e F1-score, demonstrando maior eficiência na extração de características relevantes.
---
### 🔹 F — Wavelet Híbrida no Pipeline
- Combinação de múltiplas wavelets ao longo da rede  
- Objetivo: capturar diferentes tipos de informação (borda, textura, estrutura)

**Resultado:**
Não apresentou ganhos significativos, sugerindo possível redundância de informações ou dificuldade de otimização do modelo devido ao aumento da complexidade.
---

### 🔹 G — Wavelet Dual-Branch
- Arquitetura com dois ramos paralelos
- Integra múltiplas representações no processo de fusão de features

**Resultado:**
Melhor desempenho geral em todas as métricas (Acc, F1 e AUC), evidenciando que a combinação explícita de diferentes representações melhora significativamente a capacidade discriminativa do modelo.
---

# Principais Constatações

- A integração de wavelets permite incorporar informações no domínio da frequência, enriquecendo a representação aprendida pela CNN
- Diferentes famílias de wavelets capturam características complementares (bordas, texturas, estruturas globais)
- Modelos com uma única wavelet apresentam ganhos limitados e dependem fortemente da escolha da base
- A utilização de múltiplas representações (Dual-Branch) mostrou-se mais eficaz do que simplesmente empilhar wavelets no pipeline
- O aumento da complexidade (ex: modelo híbrido) pode introduzir:
  - redundância de features
  - dificuldade de convergência
- Tornar os filtros wavelet treináveis aumenta a adaptabilidade, permitindo especialização ao domínio das imagens mamográficas
- Sem o ROI o modelo não consegue alcançar acurácias acima de 70%

---

# Conclusão

Os resultados demonstram que a integração de transformadas wavelet em CNNs é uma abordagem promissora para análise de imagens médicas.

Em particular:

- A utilização de representações no domínio da frequência contribui para uma melhor discriminação entre classes
- Arquiteturas que exploram múltiplas visões dos dados (como o modelo Dual-Branch) apresentam ganhos consistentes
- Nem toda adição de complexidade resulta em melhoria, sendo essencial equilibrar:
capacidade representacional
estabilidade de treinamento

De forma geral, o estudo evidencia que wavelets são mais eficazes quando utilizadas de maneira estrutural (multi-branch) do que apenas como substitutas diretas de operações como pooling.

---

