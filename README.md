# Risco_Imprensa_2023

:droplet:@Sabesp:droplet:, 

Este é um projeto embrionário para avaliação computacional de notícias de imprensa para fins de avaliação do risco de reputação. Para isso, há análise dos títulos de notícias relacionadas a Sabesp em duas metodologias: análise de sentimento e aprendizado de máquina não supervisionado em LDA. Ambas passam por pré-processamento de linguagem natural no modelo tokenização.

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

## Análise de Sentimento :heartbeat:
A análise de sentimento é feita por um algoritmo "SentimentIntensityAnalyzer", pertencente ao pacote de aprendizado de máquina NLTK (Natural Language Toolkit). A documentação está disponível no site da biblioteca: https://www.nltk.org/howto/sentiment.html.

De forma sucinta, o algoritmo utiliza cada documento (conjunto de tokens de uma mesma notícia) e associa um rótulo de sentimento, em um score entre -1 e 1, sendo -1 o mais negativo possível e 1 o mais positivo. Isso é feito em um procedimento que avalia de forma estatística a frequência de palavras e o conjunto de seus valores semânticos. Como se trata de um modelo de aprendizado de máquina, o processo é feito de forma iterada em todos os documentos, para identificar quais grupos de palavras são maais relevantes para compossição do significado e para a pontuação de cada documento. 

Veja na imagem abaixo como o processo é realizado e como cada documento recebe um valor:
![image](https://github.com/gustavo-westin/Risco_Imprensa/assets/113940727/c62c329b-8e5c-49b7-bfce-e385195b52f8)

Ao final do processo é possível contabilizar (somar) as ocorrências e verificar a distribuição.

```
# verificar qtde por sentimento e percentual relativo
neutro = df_titulo.query('sentimento == 0').count()[0]
neutrop = round(neutro/df_titulo.shape[0], 2)
negativo = df_titulo.query('sentimento < 0').count()[0]
negp = round(negativo/df_titulo.shape[0], 2)
positivo = df_titulo.query('sentimento > 0').count()[0]
posp = round(positivo/df_titulo.shape[0], 2)
print(f'Avalição dos títulos de notícias (12 meses):\n'
      f'Neutras: {neutro} ({neutrop}%)\n'
      f'Negativas: {negativo} ({negp}%)\n'
      f'Positivas: {positivo} ({posp}%)')

```

![image](https://github.com/gustavo-westin/Risco_Imprensa/assets/113940727/a5ad3e45-cfb9-49a5-8853-9e7ccab757b3)


## LDA :microscope:
Diferente da análise de sentimento, nesse método o interesse não é avaliar o valor da notícia, mas o que é a notícia. Isto é, identificar padrões textuais e agrupá-los. Isso é particularmente útil para identificar rapidamente quais são os principais temas e o que está sendo falado sobre eles, algo especialmente útil em um ambiente de análise de risco na imprensa. 

Antes de entrar diretamente no LDA, é feito um trabalho de observação de recorrência, o que ajudará a validar o processo de aprendizado de máquina por comparação. Na recorrência identificamos, de forma simplificada, quais são as palavras com maior incidência no corpus textual. Isso ajuda a identificar, ainda que sem considerar a intricada relação semântica entre as palavras, o contexto geral do que está sendo dito.

A primeira forma de visualização é o gráfico simples de recorrência, que após alguns ajustes para eliminar palavrás óbvias, como Sabesp, tem esse formato:
```
# como a palavra mais frequente será sempre a de valor buscado (sabesp) será removida
# também será retirada as palavras "paulo" "SP" porque são atributos do Estado de prestação dos serviços
# também a palavra sobre (preposição) que não traz significado sozinha
most_common = freq.most_common(14)
# exclusão de palavras
del most_common[7]
del most_common[5]
del most_common[3]
del most_common[0]
# utilizar a lista para criar os padrões do gráfico em um loop
words = [word for (word, freq) in most_common]
frequencies = [freq for (word, freq) in most_common]
# plotar um gráfico de linha com as frequências
plt.title('Top 10 Palavras Mais Frequentes (considerando exceções)')
plt.xlabel('Palavras')
plt.ylabel('Frequência')
plt.plot(words, frequencies)
# fazer uma rotação do eixo em 45 para exibir "melhor" os rótulos
plt.xticks(rotation=45)
plt.show()
```

![image](https://github.com/gustavo-westin/Risco_Imprensa/assets/113940727/2f2af390-9251-4ae1-a65c-68a8ebdc51c0)

Outra forma de visualizar é pela nuvem de palavras, que permite um olhar um pouco mais profundo da recorrência. 

```
# Gerar a nuvem de palavras para melhor visualização da recorrência

freq = nltk.FreqDist(cleaned_tokens)
wordcloud = WordCloud(width=800, height=800, min_font_size=10).generate_from_frequencies(freq)
plt.figure(figsize=(8, 8), facecolor=None)
plt.imshow(wordcloud)
plt.axis("off")
plt.tight_layout(pad=0)
plt.show();
```

![Sem título](https://github.com/gustavo-westin/Risco_Imprensa/assets/113940727/e014f040-9e2d-4336-9333-82f2d3db3f41)


O LDA fará uma abordagem mais complexa, pois considerará a relação entre as palavras para associá-las em grupos de acordo com a probabilidade. De forma bastante simplificada, o LDA é um modelo matemático aplicado no aprendizado de máquina *não supervisionado* que busca revelar os tópicos subjacentes do conjunto de documentos, de acordo com um parâmetro fornecido pelo usuário (quantidade de tópicos desejados).

O modelo matemático é bastante complexo, pois é uma técnica generativa, que envolve cruzamentos sucessivos de escolha aleatória de tópicos para uma composição aleatória de tokens de N documentos, sendo feita sucessivas vezes. O resultado é a probabilidade de uma palavra estar associada a um determinado tópico, dada sua recorrência, e a probabilidade dela estar próxima a outra palavra deste mesmo tópico.

Para o usuário, o resultado é a composição de tópicos, cujo significado é dado pelo contexto, de conhecimento do analista de negócios. Na imagem abaixo vemos o experimento para cinco tópicos, que nos revela, pelo contexto, que os tópicos principais se associam a:
* mutirão de dívidas
* mercado acionário
* reclamações relacionadas a contas falsas
* evento de fevereiro de 2023 no litoral norte
* privatização da companhia

![image](https://github.com/gustavo-westin/Risco_Imprensa/assets/113940727/8950ffca-0048-49ed-9cf6-62e7e80d23e8)







