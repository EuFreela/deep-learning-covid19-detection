# Deep Learning para Detecção de COVID-19 em Radiografias Torácicas

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.23504469.svg)](https://doi.org/10.5281/zenodo.23504469)

**Author:** Lameck Silva Fernandes  
**Affiliation:** Independent Researcher  
**Location:** Passos, MG, Brazil  

---

Projeto de aprendizado profundo (*deep learning*) que investiga o uso de **MobileNetV2 com *transfer learning*** para classificar radiografias torácicas em três categorias: COVID-19, Normal e Pneumonia.

Este repositório acompanha o artigo científico *Deep Learning Applied to COVID-19 Detection in Chest X-Rays: A Transfer Learning Approach*, de **Lameck Silva Fernandes**. O repositório reúne o notebook de treinamento, os resultados experimentais, o modelo salvo e materiais complementares relacionados à pesquisa.

> **Aviso e perspectivas futuras:** Este projeto encontra-se em fase de MVP (*Minimum Viable Product*), com resultados experimentais obtidos a partir de uma base pública de radiografias torácicas. O modelo ainda não foi validado clinicamente e não deve ser utilizado para diagnóstico ou tomada de decisões médicas.
>
> As próximas etapas previstas incluem validação externa com conjuntos de dados independentes e, futuramente, estudos de validação em ambiente hospitalar, conduzidos em colaboração com instituições de saúde e pesquisadores qualificados. Essas etapas poderão exigir financiamento para pesquisa, aprovação ética aplicável, definição de protocolos de estudo e cumprimento dos requisitos de proteção de dados e privacidade dos participantes.
>
> O objetivo de longo prazo é investigar a viabilidade da abordagem em contextos clínicos reais, sem presumir antecipadamente sua eficácia ou aplicabilidade médica.

## Visão geral

O projeto investiga a capacidade de um modelo baseado em MobileNetV2, pré-treinado no ImageNet, de reconhecer padrões visuais em radiografias torácicas e distinguir três classes:

- **COVID-19:** imagens rotuladas como COVID-19 na base de dados, utilizadas para estudar padrões radiográficos associados à doença.
- **Normal:** imagens incluídas na categoria *Normal* da base de dados, sem alterações radiográficas identificadas nessa classificação.
- **Pneumonia:** imagens rotuladas como pneumonia na base de dados.

As categorias representam os rótulos da base utilizada. A classificação do modelo não confirma, por si só, o estado clínico de uma pessoa.

O fluxo experimental inclui pré-processamento, aumento de dados (*data augmentation*), treinamento em duas etapas e avaliação por métricas de classificação.

## Resultados experimentais

A avaliação reportada foi realizada em um conjunto de teste com 965 imagens.

| Métrica | Resultado |
|---|---:|
| Acurácia | 94,72% |
| Precisão macro | 94,07% |
| Sensibilidade macro | 95,11% |
| F1-score macro | 94,54% |
| F1-score ponderado | 94,77% |

### Desempenho na classe COVID-19

| Métrica | Resultado |
|---|---:|
| Precisão | 97,67% |
| Sensibilidade (*recall*) | 96,55% |
| F1-score | 97,11% |

Esses valores descrevem o desempenho no conjunto avaliado. Não garantem desempenho equivalente em outras bases, hospitais, populações ou condições de aquisição de imagens.

## Base de dados

O experimento utilizou o [Chest X-Ray Dataset no Kaggle](https://www.kaggle.com/datasets/woleolajide86/chest-xray-dataset), disponibilizado por Woleolajide86.

| Classe | Quantidade de imagens |
|---|---:|
| COVID-19 | 576 |
| Normal | 1.583 |
| Pneumonia | 4.273 |
| **Total** | **6.432** |

A base apresenta desequilíbrio entre as classes, com predominância de imagens de pneumonia.

As imagens originais não são incluídas neste repositório. O acesso, uso e eventual redistribuição estão sujeitos aos termos definidos pelo responsável pela base de dados.

## Metodologia

### Pré-processamento

- Redimensionamento das imagens para 224 × 224 pixels.
- Normalização dos pixels por divisão por 255.
- Aplicação de aumento de dados somente às imagens de treinamento.
- Transformações descritas no experimento: rotação, deslocamentos horizontal e vertical, zoom e espelhamento horizontal.

### Arquitetura

A arquitetura utilizada foi MobileNetV2, pré-treinada no ImageNet, como base convolucional. A cabeça de classificação foi composta por:

1. Global Average Pooling.
2. Camada densa com 256 unidades e ativação ReLU.
3. Dropout com taxa de 0,5.
4. Camada de saída com três unidades e ativação Softmax.

### Estratégia de treinamento

O treinamento foi realizado em duas etapas:

1. **Extração de características:** a base convolucional pré-treinada foi congelada enquanto a camada de classificação era treinada.
2. **Ajuste fino (*fine-tuning*):** as 30 camadas finais foram selecionadas para treinamento, mantendo congeladas as camadas BatchNormalization.

Configurações reportadas:

- Otimizador: Adam.
- Taxa de aprendizado inicial: `1e-3`.
- Taxa de aprendizado no ajuste fino: `1e-5`.
- Tamanho do lote (*batch size*): 16.
- Até 10 épocas por etapa.
- Monitoramento da perda de validação e salvamento do melhor modelo.

### Divisão dos dados

| Conjunto | Quantidade de imagens |
|---|---:|
| Treinamento | 4.502 |
| Validação | 965 |
| Teste | 965 |
| **Total** | **6.432** |

A avaliação incluiu acurácia, precisão, sensibilidade (*recall*), F1-score e matriz de confusão.

## Modelo treinado

O repositório inclui o arquivo `melhor_modelo_fase2.keras`, associado à segunda etapa do treinamento (*fine-tuning*).

O arquivo pode ser utilizado em experimentos de inferência e avaliação, desde que o ambiente de software, a arquitetura e o pré-processamento sejam compatíveis com os utilizados no treinamento. Consulte o notebook para verificar o fluxo de carregamento e uso do modelo.

A presença do arquivo não implica validação clínica nem garante generalização para dados externos.

## Estrutura do repositório

A estrutura abaixo corresponde aos arquivos principais apresentados no projeto. Atualize-a se adicionar ou remover arquivos.

```text
deep-learning-covid19-detection/
├── README.md
├── notebooks/
│   └── mobilenetv2_training.ipynb
├── figures/
│   └── training_curves.png
├── melhor_modelo_fase2.keras
├── requirements.txt
├── CITATION.cff
└── LICENSE
```

## Como executar

### Requisitos

O notebook utiliza bibliotecas Python para aprendizado de máquina, processamento de dados e visualização, incluindo TensorFlow/Keras, NumPy, Matplotlib e scikit-learn.

Instale as dependências a partir da raiz do repositório:

```bash
pip install -r requirements.txt
```

Confira se `requirements.txt` contém as dependências e versões compatíveis com o notebook e com o arquivo de modelo salvo.

### Preparar os dados

1. Obtenha a base de dados na [página do conjunto no Kaggle](https://www.kaggle.com/datasets/woleolajide86/chest-xray-dataset).
2. Organize as imagens conforme a estrutura de diretórios esperada pelo notebook.
3. Ajuste os caminhos de arquivos e diretórios no notebook para seu ambiente.
4. Execute as células na ordem apresentada.

Os caminhos e a organização exata dos arquivos devem ser conferidos no notebook antes da execução.

## Limitações

- A base de dados apresenta desequilíbrio entre as classes.
- O conjunto de teste provém da mesma base utilizada no experimento.
- A generalização para conjuntos de dados independentes não foi estabelecida.
- Os resultados não demonstram segurança clínica nem eficácia diagnóstica.
- O modelo destina-se à pesquisa e ao uso educacional e não substitui a avaliação de profissionais de saúde.

## Reprodutibilidade

O notebook e os materiais complementares permitem inspecionar o fluxo experimental. A reprodução dos resultados pode depender das versões das bibliotecas, do ambiente de execução, da organização dos dados e das configurações de treinamento.

## Artigo científico

**Título:** *Deep Learning Applied to COVID-19 Detection in Chest X-Rays: A Transfer Learning Approach*  
**Autor:** Lameck Silva Fernandes

## Disponibilidade dos dados

A base utilizada está disponível no [Chest X-Ray Dataset do Kaggle](https://www.kaggle.com/datasets/woleolajide86/chest-xray-dataset). A reutilização e a redistribuição das imagens devem respeitar os termos de uso e a licença aplicável à base.

## 🚀 Execução do Notebook no Kaggle

Para reproduzir o treinamento do modelo, utilize o notebook disponível no diretório [`notebooks/`](notebooks/) deste repositório. A execução é recomendada no ambiente Kaggle Notebooks, que oferece acesso a recursos de GPU.

### ⚙️ Configuração do ambiente

Antes de executar as células do notebook, configure o ambiente conforme indicado abaixo:

1. **Habilitar acesso à internet:** no painel de configurações do notebook, ative a opção **`Turn on internet`**. Isso permite baixar dependências e acessar recursos externos necessários ao treinamento.
2. **Selecionar a GPU:** em *Settings → Accelerator*, selecione **`GPU T4 x2`**, caso essa opção esteja disponível na sua conta e sessão do Kaggle.
3. **Verificar as dependências:** confirme se as bibliotecas necessárias estão instaladas conforme o arquivo [`requirements.txt`](requirements.txt).
4. **Executar as células:** execute o notebook sequencialmente, verificando a disponibilidade dos dados, o carregamento do MobileNetV2 e a conclusão das etapas de treinamento e avaliação.

### Observações importantes

- A disponibilidade de GPU e da opção `GPU T4 x2` depende das cotas, das configurações e da disponibilidade de recursos do Kaggle.
- A opção `GPU T4 x2` disponibiliza duas GPUs T4, mas o código não necessariamente utilizará ambas automaticamente. O aproveitamento das duas GPUs depende da implementação do treinamento.
- A ativação da internet pode ser necessária para baixar bibliotecas ou pesos pré-treinados, mas o acesso à base de dados também deve estar configurado corretamente.
- O tempo de execução varia conforme a disponibilidade dos recursos, o volume de dados e a configuração do treinamento.

**Reprodutibilidade:** recomenda-se registrar as configurações utilizadas, as versões das bibliotecas e os resultados obtidos em cada execução para facilitar a reprodução dos experimentos.

## Citação

Caso utilize este repositório em trabalhos acadêmicos, cite o artigo associado e consulte o arquivo `CITATION.cff` para os metadados de citação do projeto. Acrescente DOI, URL da publicação e dados bibliográficos definitivos quando estiverem disponíveis.

## Licença

Consulte o arquivo `LICENSE` para conhecer os termos de reutilização do código. A licença do código não se estende automaticamente ao conjunto de dados, ao artigo ou a materiais de terceiros.
