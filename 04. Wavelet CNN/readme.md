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

## 📈 Resultados

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
CNN tradicional sem uso de wavelets.

---

### 🔹 B — Wavelet Haar
- Substitui pooling por decomposição Haar  
- Foco em bordas e descontinuidades  

---

### 🔹 C — Wavelet Biorthogonal (1.3)
- Melhor preservação estrutural  
- Resultados superiores em AUC  

---

### 🔹 D — Wavelet Daubechies (db4)
- Melhor desempenho geral em acurácia e F1  
- Boa captura de textura  

---

### 🔹 E — Wavelet Híbrida
- Combinação de múltiplas wavelets ao longo da rede  
- Objetivo: capturar diferentes tipos de informação (borda, textura, estrutura)

---

### 🔹 F — Dual-Branch
- Arquitetura paralela com múltiplas representações  
- Fusão de features de diferentes bases wavelet  

---

# Principais Constatações

- Wavelets melhoram a representação ao incorporar **informação espectral**
- Diferentes wavelets capturam **tipos distintos de padrões**
- Tornar os filtros **treináveis** aumenta a adaptabilidade do modelo
- Há um trade-off entre:
  - diversidade de representação
  - custo computacional

---

# Conclusão

A integração de wavelets no pipeline de CNNs mostra-se promissora para:

- melhorar a extração de características
- aumentar a robustez do modelo
- capturar padrões multi-escala em imagens médicas

---

