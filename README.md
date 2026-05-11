# 🧠 GoogLeNet no Tiny ImageNet

Projeto de Deep Learning focado na otimização da arquitetura **GoogLeNet (InceptionV1)** no dataset **Tiny ImageNet 200**, utilizando experimentação controlada para maximizar a acurácia Top-1.

---

## 🚀 Resultados

- **Top-1 Accuracy:** 45.27%  
- **Top-5 Accuracy:** 70.31%  
- **Melhoria:** +5.39 pp sobre o baseline  

🏆 Resultado equivalente ao **2º lugar em benchmark externo**

---

## 📦 Dataset: Tiny ImageNet

O Tiny ImageNet é uma versão reduzida do ImageNet:

- 200 classes  
- 100.000 imagens de treino  
- 10.000 imagens de validação  
- Resolução: **64×64**

### ⚠️ Desafios

- Baixa resolução → perda de detalhes  
- Poucas amostras por classe → overfitting  
- Classes semelhantes → alta complexidade  
- Arquiteturas padrão não adaptadas  

---

## 🧹 Pré-processamento e Dados

### ✔️ Tratamento

- Normalização dos pixels `[0,1]`  
- One-hot encoding das classes  
- Padronização para 64×64  

### 🔀 Divisão

- Treino: 100k  
- Validação: 10k  
- Teste: não utilizado  

### 🔄 Data Augmentation

- Random Rotation  
- Random Brightness  
- Random Zoom  
- Mixup (testado)

📌 Observação:
> Augmentation agressiva causou instabilidade em alguns cenários.

---

## ⚙️ Configuração

- **Framework:** TensorFlow 2.19  
- **GPU:** Tesla P100 / Tesla T4  
- **Batch size:** 128  
- **Épocas:** até 120  
- **Parâmetros:** ~10.3M  

---

## 📏 Métricas

- **Top-1 Accuracy**
- **Top-5 Accuracy**

📌 Justificativa:
> Em datasets com muitas classes, Top-5 fornece uma visão mais robusta do desempenho.

---

## 🔬 Metodologia

Abordagem experimental iterativa:

> **Hipótese → Implementação → Resultado → Análise**

- 9 experimentos realizados  
- Mudanças controladas  
- Comparação direta entre execuções  

---

## 🧪 Experimentos

### Baseline
- GoogLeNet adaptado para 64×64  
- Top-1: **39.88%**

---

### 🔧 Ajustes realizados

#### ✔️ Redução de parâmetros (exp_001)
- Redução de canais nos blocos finais  
- Melhor eficiência computacional  

---

#### ⚠️ Regularização (exp_002, exp_007)
- Testes com diferentes níveis de dropout  

📌 Resultado:
> Regularização excessiva prejudica desempenho

---

#### ❌ Label Smoothing incorreto (exp_003)
- Aplicado em múltiplas saídas  
- Resultado: **colapso do treinamento**

---

#### ✔️ Correção + Warmup (exp_004)
- Label smoothing apenas na saída principal  
- Adição de LR warmup  

---

#### 🚀 Nova Stem (exp_005) — PRINCIPAL RESULTADO

- Substituição de MaxPool por **Conv stride=2**
- Inicialização: `he_normal`

📈 Resultado:
- **Top-1: 45.27%**

💡 Insight:
> Melhor fluxo de gradiente → maior ganho do projeto

---

#### 🔄 Mixup (exp_006)
- Não trouxe ganho significativo  

---

#### ⚖️ Ajuste de regularização (exp_007)
- Redução de dropout  
- Ajuste de peso auxiliar  

---

#### ⚙️ Otimizador (exp_008)
- SGD → AdamW  

📌 Resultado:
> Queda de performance → SGD superior nesse cenário

---

## 📊 Análise dos Resultados

### 🔍 Observações principais

- Maior ganho veio de **mudança arquitetural**
- Ajustes de hiperparâmetros tiveram impacto limitado
- Regularização excessiva prejudica aprendizado
- Gap entre saída principal e auxiliar é um bom diagnóstico

---

## 📈 Visualização (Análise)

- Comparação entre experimentos (baseline → exp_008)
- Evolução da acurácia Top-1
- Análise do comportamento de treinamento

📌 Sugestões futuras:
- Curvas de loss/accuracy por época  
- Matriz de confusão  
- Análise por classe  

---

## 🏆 Comparação com Benchmark

| Modelo | Top-1 |
|------|------|
| VGG16_bn | 48.15% |
| **GoogLeNet (este projeto)** | **45.27%** |
| ResNet34 | 43.11% |

📌 Destaque:
> +22.68 pp sobre InceptionV1 não adaptado

---

## 💡 Principais Insights

- 🔥 Arquitetura > tuning de hiperparâmetros  
- 📊 Gap main vs aux é indicador importante  
- ⚠️ Label smoothing mal aplicado pode colapsar treino  
- ⚙️ SGD > AdamW neste problema  
- 🚫 Regularização excessiva é prejudicial  

---

## 🚀 Próximos Passos

- Adicionar Batch Normalization  
- Testar InceptionV3-style (fatoração)  
- Ajustar Mixup + reduzir dropout  
- Aumentar resolução (ex: 96×96)  

---

## 📌 Reprodutibilidade

Todos os experimentos foram conduzidos com controle de variáveis, garantindo comparabilidade entre execuções.

---

## 👨‍💻 Autor

Pedro Henrique  
Estudante de Ciência de Dados  
