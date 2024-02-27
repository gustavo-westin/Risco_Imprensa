# Risco_Imprensa_2023

:droplet:@Sabesp:droplet:, 

Este é um projeto embrionário para avaliação computacional de notícias de imprensa. Para isso, há análise de tokens dos títulos de notícias relacionadas a Sabesp em duas metodologias: análise de sentimento e aprendizado de máquina não supervisionado em LDA. Ambas passam por pré-processamento de linguagem natural no modelo tokenização.

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

Além da avaliação e eliminação de noticias idênticas, o principal processo de tratamento realizado é a preparação para utilização de técnicas de NLP (processamento de linguagem natural). Esse processo é dividido em dois momentos: 
*  Transformação dos textos em corpus e documentos
*  Atribuição de tokens para os elementos unitários que compõem os documentos

De forma simplificada, o corpus é o texto corrido que será atribuido a um documento identificável, isto é, com um índice. É como se colocassemos uma chave que identificasse que qualquer trecho de uma notícia pertence a notícia 12.343, por exemplo. Isso é importante porque como as palavras ou expressões serão transformadas em unidades (tokens), é importante que deixemos um lastro para avaliar o conjunto de uma notícia.

Exemplo da transformação em corpus:
![image](https://github.com/gustavo-westin/Risco_Imprensa/assets/113940727/0086204c-e03a-4fef-b64b-ef5ceaa54b81)


A **tokenização** é a divisão dos documentos em unidades semânticas, normalmente palavras, mas que preservem a classe gramatical e as relações de dependência sintática. Isso é importante porque a distância (associação) entre diferentes palavras pode trazer significados distintos: lixo comum tem significado distinto das palavras sozinhas "lixo" e "comum". 

Aos tokens precisam ser eliminados as chamadas "stop words", palavras sem valor semântico, como pontuação. Isso é um processo de várias rodadas e até certo ponto manual, pois é necessário entender o contexto para saber se, por exemplo, um $ tem valor semântico.
```
# Tokenizar o all_text
# objetivo é criar um corpus para avaliação a partir de parâmetros
nltk.download('punkt')
tokens = word_tokenize(all_text)
# Converter todas as palavras para lowercase
# aglutina termos e evita duplicidades ou subnotificação de tokens
tokens = [word.lower() for word in tokens]
# Remover stopwords
# as stopwords são palavras como preposições ou artigos que não acrescentam sentido ao texto
nltk.download('stopwords')
stop_words = set(stopwords.words("portuguese"))
cleaned_tokens = [word for word in tokens if word not in stop_words]
```


Resultado de uma primeira limpeza, que ainda guarda elementos sujos, como ";" e "23,2".
![image](https://github.com/gustavo-westin/Risco_Imprensa/assets/113940727/3a1d5737-a81a-4c4d-963f-0580a87fa03a)

Nova limpeza de "stop words":
```
# Adicionar caracteres especiais à lista de stop words
# algumas "stopwords" precisam ser acrescentadas
stop_words.update([",", ":", "rt","@","?","(", ")",".","!", "%"])
cleaned_tokens = [word for word in cleaned_tokens if word not in stop_words]
```

Após as sucessivas etapas de limpeza, temos um corpus e tokens prontos para serem avaliados.

## Análise de Sentimento






