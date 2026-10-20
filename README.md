# CoffeeLeaf AI — Identificação de Condições Foliares do Cafeeiro

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.23480207.svg)](https://doi.org/10.5281/zenodo.23480207)

**Author:** Lameck Silva Fernandes  
**Affiliation:** Independent Researcher  
**Location:** Passos, MG, Brazil  

---

Aplicação Web Progressiva (*Progressive Web App — PWA*) que integra um modelo de classificação de imagens para identificar cinco categorias de condições foliares do cafeeiro: **Healthy, Miner, Rust, Phoma e Cercospora**.

Este repositório acompanha o artigo científico *Development of a Progressive Web Application for Coffee Leaf Condition Identification Using Artificial Intelligence*. O projeto reúne o código da aplicação e materiais associados à organização dos dados e ao treinamento do modelo.

> **Aviso e perspectivas futuras:** Este projeto encontra-se em fase de protótipo experimental, com uma demonstração funcional baseada em um subconjunto público de imagens de folhas de cafeeiro. O modelo ainda não foi avaliado quantitativamente em um conjunto de teste independente e não deve ser considerado uma ferramenta validada para diagnóstico fitossanitário.
>
> As próximas etapas incluem avaliação com imagens rotuladas não utilizadas no treinamento e validação externa com dados de diferentes origens. Estudos futuros poderão exigir colaboração com especialistas em fitossanidade, protocolos de coleta e avaliação, e recursos para pesquisa.
>
> O objetivo de longo prazo é investigar a viabilidade da abordagem em condições reais de campo, sem presumir antecipadamente sua precisão ou aplicabilidade operacional.

## Visão geral

O CoffeeLeaf AI utiliza um modelo de classificação de imagens para apresentar uma classe prevista e uma pontuação de confiança para imagens de folhas de cafeeiro. As cinco classes correspondem aos rótulos presentes no conjunto de dados:

- **Healthy:** folha saudável.
- **Miner:** folha identificada como afetada por minador.
- **Rust:** ferrugem do cafeeiro.
- **Phoma:** classe Phoma do conjunto de dados.
- **Cercospora:** classe Cercospora do conjunto de dados.

Os nomes das classes refletem os rótulos do conjunto utilizado. A previsão automatizada não confirma, por si só, a condição fitossanitária real da planta.

## Demonstração funcional

Em uma demonstração com uma única imagem, a aplicação exibiu a classe **Healthy** com uma pontuação de confiança de **100,0**. Essa observação demonstra o funcionamento da interface de predição para uma imagem específica, mas não estabelece a acurácia geral do modelo.

Não foram reportadas métricas quantitativas obtidas em um conjunto de teste independente, documentado e separado dos dados de treinamento. Por isso, este projeto não apresenta valores gerais de acurácia, precisão, sensibilidade (*recall*) ou F1-score.

## Base de dados

O projeto utiliza um subconjunto do conjunto público JMuBEN/JMuBEN2, composto por imagens de folhas de cafeeiro Arábica. A publicação associada reporta **58.555 imagens** distribuídas entre cinco classes.

Para o treinamento do CoffeeLeaf AI, foram selecionadas **5.000 imagens**, sendo 1.000 por classe. Em cada classe, foram selecionadas as primeiras 1.000 imagens na ordem existente no conjunto de dados; não foi utilizado um procedimento de amostragem aleatória.

### Distribuição reportada no conjunto original

| Classe | Quantidade de imagens |
|---|---:|
| Healthy | 18.985 |
| Miner | 16.979 |
| Rust | 8.337 |
| Phoma | 6.572 |
| Cercospora | 7.682 |
| **Total** | **58.555** |

### Amostra utilizada no projeto

| Classe | Quantidade de imagens |
|---|---:|
| Healthy | 1.000 |
| Miner | 1.000 |
| Rust | 1.000 |
| Phoma | 1.000 |
| Cercospora | 1.000 |
| **Total** | **5.000** |

O conjunto original foi coletado em uma fazenda em Mutira, no condado de Kirinyaga, Quênia. A generalização para outras regiões, cultivares, condições climáticas ou dispositivos de captura não foi estabelecida.

As imagens originais não são redistribuídas automaticamente por este repositório. O acesso e eventual reutilização devem respeitar os termos e a licença do conjunto de dados original.

## Metodologia

### Seleção e preparação dos dados

- Seleção de 1.000 imagens por classe, totalizando 5.000 imagens.
- Seleção das primeiras 1.000 imagens de cada classe na ordem existente no conjunto original.
- Renomeação e organização das imagens em grupos por classe.
- Envio das imagens ao Google Teachable Machine para treinamento.
- Não foi realizado pré-processamento manual adicional antes do envio. Redimensionamento, normalização e outras transformações internas da plataforma não foram configuradas ou verificadas de forma independente.

### Arquitetura

O modelo foi treinado no Google Teachable Machine, que utiliza uma rede neural convolucional (*Convolutional Neural Network — CNN*) com arquitetura MobileNet como base (*backbone*) e aprendizagem por transferência (*transfer learning*). Os detalhes internos do modelo exportado não foram verificados de forma independente.

### Estratégia de treinamento

O treinamento foi realizado em uma sessão pela interface do Google Teachable Machine.

Configurações informadas na plataforma:

| Parâmetro | Configuração |
|---|---|
| Plataforma | Google Teachable Machine |
| Épocas | 50 |
| Tamanho do lote (*batch size*) | 16 |
| Taxa de aprendizado | 0,001 |
| Número de classes | 5 |
| Imagens selecionadas | 5.000 |

A divisão automática dos dados da plataforma foi utilizada, mas as proporções exatas e os papéis dos subconjuntos resultantes não foram verificados independentemente.

### Avaliação

Após o treinamento, previsões foram inspecionadas na aplicação CoffeeLeaf AI. Não foi realizada avaliação quantitativa em um conjunto de teste independente e documentado. Portanto, o desempenho geral e a capacidade de generalização do modelo permanecem não estabelecidos.

## Aplicação Web Progressiva

O CoffeeLeaf AI integra o modelo treinado a uma aplicação acessível pelo navegador. O fluxo documentado consiste em enviar uma imagem, processá-la com o modelo integrado e exibir a classe prevista com a pontuação de confiança associada.

A demonstração apresenta a integração funcional entre o classificador e a interface PWA; não constitui validação da confiabilidade do modelo em condições reais de campo.

## Arquitetura do sistema

O fluxo geral do projeto compreende duas etapas:

1. **Desenvolvimento do modelo:** seleção e organização de 5.000 imagens em cinco classes, seguida de treinamento no Google Teachable Machine.
2. **Uso da aplicação:** envio de uma imagem à PWA, inferência pelo modelo integrado e exibição da classe prevista e da pontuação de confiança.

Os detalhes internos da arquitetura exportada e do pré-processamento não foram verificados de forma independente; por isso, não são especificados além das informações documentadas neste projeto.

## Resultados e discussão

A demonstração funcional mostrou a aplicação exibindo a classe **Healthy** com uma pontuação de confiança de **100,0** para uma única imagem. Esse exemplo ilustra a interface de predição, mas não permite concluir que o modelo tenha 100% de acurácia.

Como não foram reportadas métricas de um conjunto de teste independente e documentado, não é possível quantificar, com as informações disponíveis, a precisão global, a sensibilidade por classe ou a capacidade de generalização do modelo. Uma avaliação futura deverá utilizar imagens rotuladas que não tenham sido usadas no treinamento e reportar métricas apropriadas.

## Limitações

- A demonstração de uma única previsão não estabelece a acurácia geral do modelo.
- Não foram reportadas métricas quantitativas de um conjunto de teste independente e documentado.
- As proporções e os papéis da divisão automática de dados do Teachable Machine não foram verificados de forma independente.
- A seleção das primeiras 1.000 imagens de cada classe não foi aleatória e pode limitar a representatividade da amostra.
- O conjunto de dados tem uma origem geográfica específica; a generalização para outras regiões e condições não foi estabelecida.
- O modelo não foi validado em campo por meio de um estudo externo documentado.
- A pontuação de confiança exibida pela interface não deve ser interpretada automaticamente como probabilidade calibrada de acerto.

## Perspectivas futuras

As próximas etapas incluem avaliar o modelo com imagens rotuladas que não tenham sido utilizadas no treinamento, documentar a divisão entre treinamento, validação e teste, reportar métricas por classe e realizar validação externa com imagens de diferentes origens. A confiabilidade em condições reais de campo deverá ser investigada antes de qualquer afirmação de aplicabilidade operacional.

## Artigo científico

**Título:** *Development of a Progressive Web Application for Coffee Leaf Condition Identification Using Artificial Intelligence*  
**Autor:** Lameck Silva Fernandes

**DOI:** [10.5281/zenodo.23480207](https://doi.org/10.5281/zenodo.23480207)

## Disponibilidade dos dados

O conjunto original JMuBEN/JMuBEN2 está disponível no [Mendeley Data](https://data.mendeley.com/datasets/t2r6rszp5c/1). O projeto utiliza um subconjunto de 5.000 imagens, selecionadas e reorganizadas a partir do conjunto original. O uso e a redistribuição das imagens devem respeitar os termos de licença do conjunto original.

O conjunto BRACOT, citado no artigo como recurso relacionado à segmentação de folhas de cafeeiro, pode ser consultado em [Mendeley Data — BRACOT](https://data.mendeley.com/datasets/pmkbyjpf6k/1). Ele foi desenvolvido para segmentação de instâncias e não foi utilizado como conjunto principal para a classificação de condições foliares neste projeto.

## Disponibilidade do código

O código-fonte da aplicação está disponível em [GitHub — CoffeeLeaf AI](https://github.com/EuFreela/coffeeleaf-ai). Consulte o repositório para verificar os arquivos, as instruções de execução e os materiais efetivamente disponibilizados.

## Como acessar

- **Código-fonte:** [GitHub — CoffeeLeaf AI](https://github.com/EuFreela/coffeeleaf-ai)
- **Artigo e registro DOI:** [Zenodo — 10.5281/zenodo.23480207](https://doi.org/10.5281/zenodo.23480207)
- **Conjunto de dados JMuBEN/JMuBEN2:** [Mendeley Data](https://data.mendeley.com/datasets/t2r6rszp5c/1)
- **Conjunto BRACOT:** [Mendeley Data](https://data.mendeley.com/datasets/pmkbyjpf6k/1)

## Reprodutibilidade

A reprodução dos experimentos pode depender da disponibilidade da base original, da seleção exata das imagens, da versão da plataforma de treinamento e dos arquivos de modelo exportados. Como as proporções da divisão automática e algumas transformações internas da plataforma não foram verificadas, a reprodução integral do treinamento não está totalmente documentada.

## Financiamento

Esta pesquisa não recebeu financiamento externo.

## Conflitos de interesse

O autor declara não haver conflitos de interesse.

## Declaração de uso de inteligência artificial

Durante a preparação do manuscrito, o autor utilizou o ChatGPT (OpenAI) para auxiliar na redação e estruturação do texto, no aprimoramento do inglês e na orientação técnica para escrever o código e treinar e implementar o modelo. O autor revisou e editou o conteúdo e o código gerados e assume responsabilidade pelo conteúdo final.

## Referências

1. Fernandes, Lameck Silva. *CoffeeLeaf AI: Image classification application and training data*. GitHub, 2026. [Repositório do projeto](https://github.com/EuFreela/coffeeleaf-ai).
2. Jepkoech, J.; Mugo, D. M.; Kenduiywo, B. K.; Too, E. C. *Arabica coffee leaf images dataset for coffee leaf disease detection and classification*. Data in Brief, 36, 107142, 2021. [https://doi.org/10.1016/j.dib.2021.107142](https://doi.org/10.1016/j.dib.2021.107142).
3. JMuBEN/JMuBEN2 dataset. *Arabica coffee leaf disease classification dataset: Jmuben/jmuben2*. Mendeley Data, 2021. [Dataset](https://data.mendeley.com/datasets/t2r6rszp5c/1).
4. Krohling, R.; Tozzi de Souza, J. E.; Tassis, L. M. *BRACOT — a Brazilian Arabica coffee tree images dataset for instance segmentation of coffee leaves*. Mendeley Data, 2021. [Dataset](https://data.mendeley.com/datasets/pmkbyjpf6k/1).
