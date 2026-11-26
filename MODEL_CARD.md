# Model Card — Classificador de Roupas VesteAI

- **Uso pretendido:** Sugerir automaticamente a categoria de peças de vestuário (entre 10 tipos) a partir de fotos enviadas por lojistas durante o cadastro de produtos em marketplaces.
- **Não usar para:** Bloquear contas de vendedores automaticamente, aplicar multas por erro de cadastro ou avaliar qualidade/originalidade de marcas sem revisão humana.
- **Dados:** 70.000 imagens em tons de cinza (28x28 pixels) do dataset público Fashion-MNIST, divididas em 48.000 para treino, 12.000 para validação e 10.000 para teste final.
- **Modelo:** Rede Neural (Rede 2: Média (128 neurônios)) construída em Keras com camadas Dense, ReLU, Dropout (0.2) e Early Stopping, comparada a um baseline de Árvore de Decisão.
- **Desempenho no teste:** Acurácia de 88.7% (contra 80.8% do baseline simples), reduzindo em 41% os erros de classificação.
- **Limitações:** Treinado apenas com imagens centralizadas em fundo neutro e escala de cinza (28x28). Fotos reais tiradas por celular com fundo bagunçado, iluminação ruim ou ângulos diferentes exigirão re-treino com fotos reais da plataforma.
