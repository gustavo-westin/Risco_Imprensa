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

:books: Bibliotecas utilizadas: numpy, pandas, matplot, spacy, wordcloud, nltk, sentiment analyzer, sklearn, pca, pyLDAvis, os, beautiful soup.

# RESUMO
Breve resumo da metodologia, aplicação e resultados dividido nas seguintes seções:

*  Amostra: informações sobre a amostra e suas variáveis
*  Limpeza e tratamento dos dados: breve exemplificação das técnicas aplicadas
*  Análise de sentimento
*  Latent Drichlet Allocation

## Amostra
Para esse teste de conceito foram utilizadas 16411 notícias entre 2022 e 2023, todas transformadas em texto, mas oriundas da TV, rádio, impresso e digital. Esse primeiro processamento é disponibilizado pela Boxnet, empresa especializada em clipping. Dessas 16 mil notícias, utilizamos o título e subtítulo, de modo a entender a dinâmica de valor atribuído para o principal fio condutor da narrativa.

A palavra-chave para identificar a notícia é SABESP, e suas variações de denomaminação: SBSP3 e Companhia de Saneamento Básico de São Paulo. As notícias são de origem brasileira, em todo território nacional, em português. Isso é um fator importante, pois as técnicas empregadas de processamento de linguagem natural são voltadas para as relações sintáticas da lingua portuguesa. 

Como é comum dentro do contexto do jornalismo, releases e publicações em grandes empresas jornalísticas acabam por reverberar, de forma integral, em portais menores a mesma notícia. Deste modo, por definição metodológica, optou-se por eliminar as notícias que reproduziam, de forma integral, o texto. Ao todo, foram eliminadas 4073 notícias, restando 12339 para análise.

## Limpeza e tratamento dos dados

Além 







