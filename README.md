# Risco_Imprensa_2023

:droplet:@Sabesp:droplet:, 

Este é um projeto embrionário para avaliação computacional de notícias de imprensa. Para isso, há análise de tokens dos títulos de notícias relacionadas a Sabesp em duas metodologias: análise de sentimento e aprendizado de máquina não supervisionado. Todas passam por pré-processamento de linguagem natural.

:file_folder: Files: há dois arquivos principais, um com a análise de sentimento e outro com o modelo de LDA (Latent Dirichlet Allocation)


:computer: Processo de ETL:
*  extração dos dados por dois métodos: web scrapping e por processamento de texto da boxnet.
*  transformação em csv
*  carga em ambiente Python para manipulação (Jupyter Notebook)
*  nova transformação para disponibilização local em xlsx com dados tratados


:chart_with_upwards_trend: Processo de análise de dados:
*  limpeza e manipulação dos dados 
*  transformação de dados textuais em tokens
*  manipulação com técnicas de processamento de linguagem natural
*  aplicação das metodologias descritas

:books: Bibliotecas utilizadas: numpy, pandas, matplot, spacy, wordcloud, nltk, sentiment analyzer, sklearn, pca, os, beautiful soup.

# RESUMO
Breve resumo da metodologia, aplicação e resultados. Dividido nas seguintes seções:

*  Pesquisa e amostra: informações sobre a amostra e suas variáveis
*  Limpeza e tratamento dos dados: breve exemplificação das técnicas aplicadas
*  Análise de sentimento
*  Latent Drichlet Allocation
