# 🚀 NeuroStartup: VesteAI — Classificação Automática de Roupas com Keras

Projeto desenvolvido para a **Atividade Prática 4** da disciplina de **Inteligência Artificial e Machine Learning** (Prof. Hercules Ramos).

## Integrantes do Grupo
- **Nome:** Cauã Tomas da Paixão — **RA:** 1696689
- **Nome:** Bruno William Pereira Ferrer — **RA:** 1696323
- **Nome:** Walison Araujo Santana  — **RA:** 1696860

---

## O Problema e o Cliente
Nosso cliente fictício é a **VesteAI**, uma plataforma de e-commerce de moda (inspirada em marketplaces como Mercado Livre e Shein). Pequenos lojistas perdem muito tempo cadastrando peças manualmente e frequentemente escolhem a categoria errada, o que atrapalha a busca dos compradores.

Nossa solução usa uma **Rede Neural em Keras** que olha a foto do produto e sugere automaticamente uma entre **10 categorias** (Camiseta/Top, Calça, Suéter, Vestido, Casaco, Sandália, Camisa, Tênis, Bolsa ou Bota).

---

## Produto Funcionando (Print da Tela)
Abaixo está a interface interativa criada no Google Colab onde é possível navegar pelas fotos do catálogo e ver a classificação e o nível de confiança da rede em tempo real:


---

## Como Atendemos aos Critérios da Rubrica de Avaliação

Abaixo explicamos como construímos cada parte do código seguindo exatamente o que foi pedido na rubrica da disciplina:

### 1. Conhecer os Dados
- **O que fizemos no código:** Carregamos o dataset `fashion_mnist` direto pelo Keras (70.000 imagens de roupas 28×28 pixels). Geramos dois gráficos iniciais: um de barras mostrando que todas as 10 categorias têm exatamente 6.000 imagens no treino (base equilibrada) e outro exibindo uma foto real de cada peça para entendermos a dificuldade visual entre itens parecidos (como camisa e suéter).
- **Normalização e Regra do Cofre:** Dividimos os valores dos pixels por `255.0` para que todos ficassem entre `0` e `1`, o que ajuda a rede a aprender mais rápido. Antes de qualquer treino, deixamos as 10.000 imagens de teste trancadas no "cofre" e separamos o restante em **48.000 para treino** e **12.000 para validação** (nosso simulado).

### 2. Baseline × Rede Neural 
- **Modelo Simples (Baseline):** Primeiro treinamos uma Árvore de Decisão (`DecisionTreeClassifier`) como referência básica para saber o mínimo que nossa rede precisava superar.
- **Bônus:** Também incluímos uma Regressão Logística (`LogisticRegression`) para comparar a rede neural com mais de um algoritmo clássico.
- **Testes no Keras e `experimentos.csv`:** Montamos três arquiteturas diferentes de rede neural (com 32 neurônios, 128 neurônios e duas camadas de 128 + 64 neurônios) usando ativação `relu` nas camadas internas e `softmax` na saída. Todas as tentativas foram salvas automaticamente no arquivo `experimentos.csv`.
- **Controle de Overfitting:** Colocamos camadas de `Dropout(0.2)` (para a rede não depender de poucos neurônios) e `EarlyStopping(patience=4, restore_best_weights=True)`, que interrompe o treino assim que o erro na validação para de cair. Também plotamos as curvas de erro e acurácia para comprovar que a rede aprendeu sem decorar.

### 3. Abrir o Cofre
- **Avaliação Única:** Só usamos as 10.000 imagens do teste no final, uma única vez, comparando o baseline com a melhor rede neural escolhida na validação.
- **Resultado em Linguagem de Negócio:** A cada 1.000 peças cadastradas pelos lojistas, nossa rede neural acerta cerca de **880 a 890 roupas** automaticamente (contra cerca de **800** do modelo simples), reduzindo em mais de **40%** os erros de cadastro manual.

### 4. Produto 
- **Salvamento e Interface:** Salvamos a melhor rede treinada no arquivo `modelo_vesteai.keras`, carregamos esse arquivo novamente e criamos a função `classificar_peca()`. Usando `ipywidgets.interact`, montamos um controle deslizante que exibe a foto da roupa, a categoria sugerida pela IA, a porcentagem de confiança e a etiqueta real.

### 5. Documentação e Model Card
- **Documentação:** Além deste `README.md` com instruções claras e declaração de uso de IA, o notebook gera automaticamente o arquivo `MODEL_CARD.md` ("bula" do modelo), detalhando onde o sistema deve ser usado e **onde não deve ser usado** (como punir ou bloquear lojistas automaticamente sem revisão humana).

---

## Declaração de Uso de IA
Para desenvolver este projeto, nosso grupo utilizou ferramentas de Inteligência Artificial como apoio para destravar algumas partes técnicas mais difíceis do código:
1. **Formato das imagens na entrada da rede:** Tivemos dúvida de como passar uma matriz de imagem `28x28` para camadas `Dense`. Usamos a IA para entender como funciona a camada `layers.Flatten()` para transformar a imagem em uma linha de 784 números, além de confirmar a importância de dividir os pixels por `255.0`.
2. **Configuração do Early Stopping:** Pedimos ajuda para ajustar os parâmetros do `EarlyStopping` (`monitor="val_loss"` e `restore_best_weights=True`) para garantir que o código parasse no melhor momento sem travar ou demorar muito na CPU do Google Colab.
3. **Painel interativo com imagens:** Usamos a IA para estruturar a função `classificar_peca` junto com `np.expand_dims` e `ipywidgets.interact`, fazendo a imagem aparecer corretamente toda vez que mexemos na barra de seleção.

Todo o restante da estrutura, análise dos gráficos, escolha das arquiteturas testadas e regras do Model Card foram revisados e validados pelo grupo.

---

## Como Rodar o Projeto
1. Acesse o Google Colab (https://colab.new) e faça o upload do notebook `.ipynb` deste repositório ou abra direto pelo link do GitHub.
2. No menu superior, clique em **Ambiente de execução > Executar tudo**.
3. Não é necessário instalar bibliotecas extras nem baixar arquivos manualmente, pois o dataset já vem integrado ao Keras.
4. Ao final da execução, os arquivos `experimentos.csv`, `modelo_vesteai.keras` e `MODEL_CARD.md` serão gerados na pasta do Colab e o painel interativo aparecerá na última célula de código.
